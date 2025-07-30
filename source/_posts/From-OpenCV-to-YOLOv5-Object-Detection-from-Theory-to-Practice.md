---
title: From OpenCV to YOLOv5
tags: [opencv,yolov5,object detection]
index_img: /medias/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice.png
category_bar: true
---


**本文系统性地研究了目标检测技术从传统方法到深度学习的演进过程，对比分析了OpenCV级联分类器与YOLOv5算法全套及其衍生玩法。**

 <!-- more -->
 
 # From OpenCV to YOLOv5: Object Detection from Theory to Practice
{% note primary %}**摘要**：本文系统性地研究了目标检测技术从传统方法到深度学习的演进过程，重点对比分析了 ***OpenCV级联分类器*** 与 ***YOLOv5*** 的核心原理、技术特点及适用场景。通过深入研究***Haar特征*** 与 ***卷积神经网络*** 的特征提取机制，揭示了两种方法在检测精度、计算效率等方面的本质差异。在实践层面，详细探讨了使用***Labelme***进行***数据标注***的方法论，以及如何利用 ***Roboflow*** 等平台获取和优化训练数据集。基于上述理论研究，我们将YOLOv5模型部署至 ***树莓派嵌入式平台*** ，实现了垃圾分类目标检测系统的工程化应用。本研究不仅梳理了目标检测技术的发展脉络，更通过完整的 ***"理论-数据-训练-部署"*** 闭环验证了深度学习人工智能在资源受限设备上的实用价值。{% endnote %}


{% note primary %}**关键词**：***OpenCV；YOLOv5；目标检测；树莓派***{% endnote %}

{% note primary %}**引言**：每天清晨，当我们拿起智能手机解锁时，人脸识别功能会瞬间完成身份验证；走进机场安检区，摄像头会自动标记旅客的面部位置；甚至社交软件中的“美颜滤镜”，也需要先精准定位五官。这些看似简单的功能，其实背后都依赖于图像处理的目标检测技术！{% endnote %}

&emsp;&emsp;而在早期，这类技术并非基于复杂的深度学习，而是通过OpenCV的级联检测器（如Haar级联）实现的。

### 一、OpenCV中Python 环境搭建
&emsp;&emsp;Python 环境搭建是实现目标检测的前提基础，而其环境也并不复杂，仅仅是在Python解释器的基础上添加诸如numpy、matplotlib等库即可。

![图1 OpenCV环境搭建所需要的软件包示例](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片1.png)

### 二、OpenCV的级联分类器
&emsp;&emsp;Haar分类器是一种基于机器学习的目标检测算法，它使用Haar特征描述图像中的目标。Haar特征是基于图像亮度的局部差异计算得出的，可以用来描述目标的边缘、角落和线条等特征。
&emsp;&emsp;将一系列简单的分类器按照一定的顺序级联到一起就构成了级联分类器，使用级联分类器的程序可以通过一系列简单的判断来对样本进行识别。OpenCV提供一些已经训练好的级联分类器，有人脸检测、身形检测、车牌检测等，如下图所示。想要实现哪一种图像检测，在程序启动时加载对应的级联分类器即可。

![图2 OpenCV自带的级联分类器XML文件](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片2.png)

```python
import cv2
import numpy as np
def face_detection(frame):
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')
    faces = face_cascade.detectMultiScale(gray, scaleFactor=1.05, minNeighbors=9, minSize=(30, 30))
    for (x, y, w, h) in faces:
        cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
        cv2.putText(frame, "Face", (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.9, (0, 255, 0), 2)
    return frame
cap = cv2.VideoCapture(0)
while True:
    ret, frame = cap.read()
    result = face_detection(frame)
    cv2.imshow("Face Detection", result)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
cap.release()
cv2.destroyAllWindows()
```

### 三、调用级联分类器实现检测
&emsp;&emsp;作为传统机器视觉的AI方法，人脸级联分类器采用提取眼睛区域（上暗下亮）、鼻梁区域（两侧暗中间亮）、嘴巴区域（上唇暗下唇亮）、面部轮廓（与背景的明暗对比）等多个特征的方法进行检测，效果图如下图所示。

![图4 使用OpenCV人脸级联分类器进行人脸检测效果图](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片3.png)

&emsp;&emsp;尽管Haar级联在早期表现出色，但是随着场景扩展，传统方法OpenCV图像处理的弊端逐渐开始显现，级联的方法遇到了瓶颈，其局限性在复杂场景中暴露无遗：
(1)光照敏感：强光下人脸过曝时，特征对比度消失，导致漏检。
(2)姿态与遮挡：侧脸或戴口罩时，矩形特征失效，误检率飙升。
(3)多目标处理困难：需手动调整参数适应不同目标（如猫脸检测需重新训练）。
Haar级联的失败案例：背光导致检测失败：树叶被误检测为人脸。

&emsp;&emsp;从“人工规则”到“智能学习”，随着OpenCV传统图像处理方法的失宠，一种运用前沿AI——深度学习算法的新目标检测方法应运而生：YOLO算法！YOLOv5的诞生标志着目标检测从人工设计规则转向数据驱动学习，他们有着诸多的不同：
(1)特征提取的自动化：Haar级联依赖工程师设计的黑白矩形块，而YOLOv5通过训练自动提取更复杂的特征（例如纹理、轮廓、语义信息）。
(2)全局理解图像：传统方法需滑动窗口逐区域扫描，YOLOv5将图像划分为网格，单次预测所有目标的位置和类别，效率提升百倍。
### 四、YOLOv5运行环境的搭建
&emsp;&emsp;这个的步骤可能略显复杂，主要是安装anaconda，为YOLOv5的运行创建虚拟环境，安装cuda，再根据电脑具体配置是否有GPU选择pytorch的版本。

### 五、YOLOv5的运行

![图 5 PC成功调用GPU进行YOLOv5 目标检测](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片5.png)

![图 6 YOLOv5 默认图像的处理结果（1）](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片6.png)

![图 7 YOLOv5 默认图像的处理结果（2）](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片7.png)

&emsp;&emsp;从OpenCV的“人工经验”到YOLOv5的“数据智能”，目标检测技术实现了质的飞跃。然而，YOLOv5的强大性能离不开高质量的数据支撑，但是现实生活中我们的需求是多样化的，此时我们不满足于仅仅使用YOLOv5默认的数据集进行目标检测，这时我们将深入探讨如何构建专属数据集：
### 六、用labelme进行数据集的标注[4]

![图 8 使用labelme手动进行数据集标注](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片8.png)

&emsp;&emsp;Labelme确实能够适应特定需求进行数据集的划分了，但是一次的训练需要成百上千张图片，每做一次数据集就需要手动划分这么多，显然效率较低，难以大规模推广使用，于是我们可以通过下载网络平台上的各种标注好数据集进行训练实验。
### 七、用roboflow下载数据集[5]

![图 9 在Roboflow中寻找合适的数据集](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片9.png)

### 八、对数据集进行训练和测试

![图 10 对数据集进行训练](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片10.png)

![图 11 获取训练好的特征权重文件](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片11.png)

![在图 12 训练所得混淆矩阵](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片12.png)

&emsp;&emsp;列代表预测的类别，行代表实际的类别。其对角线上的值表示预测正确的数量比例，非对角线元素则是预测错误的部分。混淆矩阵的对角线值越高越好，这表明许多预测是正确的。

![图13 训练所得labels](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片13.png)

![图14 训练所得准确率与置信度](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片14.png)

![图15 训练所得](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片15.png)

![图16 个人“手势”数据集测试（1）](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片16.png)

![图17 个人“手势”数据集测试（2）](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片17.png)

&emsp;&emsp;为了将目标检测技术从理论转化为实际应用，我们决定将YOLOv5模型部署到嵌入式设备中，打造具有实用价值的智能终端。考虑到深度学习算法对计算性能的较高要求，传统的C51、STM32等单片机难以满足运算需求，因此我们选择了性能更为强大的树莓派作为硬件平台，以确保模型能够高效稳定地运行。这一方案不仅提升了系统的实时处理能力，也为后续的功能扩展提供了充足的计算资源保障。

### 九、部署至树莓派装置进行实战检测
&emsp;&emsp;在系统实现过程中，我们针对树莓派的硬件特性进行了适配性开发。由于树莓派采用CSI摄像头接口，其图像采集方式与PC端的USB摄像头存在差异，为此我们专门优化了图像采集模块的代码架构。同时，通过设计高效的通信协议，实现了树莓派与主控芯片STM32的协同工作。

![图18 在树莓派中配置anaconda](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片18.png)

![图19 在树莓派中配置vscode](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片19.png)

![图20 对树莓派进行调试](/images/From-OpenCV-to-YOLOv5-Object-Detection-from-Theory-to-Practice/图片20.png)

&emsp;&emsp;历经反复的调试，最终完成了基于YOLOv5的智能垃圾分类系统，在自主构建的数据集支持下，能够准确识别多种垃圾类型，并通过机械执行实现自动分类压缩回收，充分展现了人工智能深度学习技术的实用价值！

### 参考文献

[1]陈之尧.基于OpenCV-Python的图像分割技术的设计与应用研究[J].中国新通信,2018,20(19):89.
[2]明月科技.《Python OpenCV快速从入门到精通》.
[3][深度学习目标检测：yolov5环境配置，适合0基础小白，超详细-CSDN博客](https://blog.csdn.net/qq_67105081/article/details/138232424?ops_request_misc=%7B%22request_id%22%3A%22b368614c237378c5bf41ba67bb6c3883%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=b368614c237378c5bf41ba67bb6c3883&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-138232424-null-null.142%5Ev102%5Epc_search_result_base2&utm_term=yolov5%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%E6%90%AD%E5%BB%BA&spm=1018.2226.3001.4187)
[4][labelme制作yolov5模型的数据集_labelme yolo-CSDN博客](https://blog.csdn.net/weixin_45736855/article/details/129583272?ops_request_misc=&request_id=&biz_id=102&utm_term=yolov5%E7%9A%84labelme%E6%A0%87%E6%B3%A8%E6%95%B0%E6%8D%AE%E9%9B%86&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-129583272.142%5Ev102%5Epc_search_result_base2&spm=1018.2226.3001.4187)
[5][下载数据集进行yolov5的训练测试_yolov5数据集下载-CSDN博客](https://blog.csdn.net/2401_86849688/article/details/145814831?spm=1001.2014.3001.5501)
[6][yolov5-runs文件中对train结果的说明_train box loss-CSDN博客](https://blog.csdn.net/qq_45305490/article/details/125219937?ops_request_misc=%7B%22request_id%22%3A%22d18d63f66eeb3c007e4fab2cfbb532b2%22%2C%22scm%22%3A%2220140713.130102334.pc_all.%22%7D&request_id=d18d63f66eeb3c007e4fab2cfbb532b2&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-125219937-null-null.142%5Ev102%5Epc_search_result_base2&utm_term=yolov5%E6%96%87%E4%BB%B6%E4%B8%8B%E7%9A%84runs%20train&spm=1018.2226.3001.4187)
[7][将yolov5运用在树莓派上进行目标检测_树莓派yolo5调用摄像头-CSDN博客](https://blog.csdn.net/2401_86849688/article/details/145858990?spm=1001.2014.3001.5501)

