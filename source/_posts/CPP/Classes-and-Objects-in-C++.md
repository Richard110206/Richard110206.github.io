---
title: Classes and Objects in C++
tags: [C++,OOP]
index_img: /medias/Object-Oriented-Programming-OOP-in-CPP.png
category: 
- CPP
- OOP
category_bar: true
---

This post dives deep into object-oriented programming (OOP), the true essence of C++!
<!--more-->

 {% note primary%}
看到过一个很讽刺的笑话：大多数国内高校学生学到的C++只是**C语言+cin/cout**。这当然只是一句玩笑话，但很深刻地反映出对于**C++精髓面向对象编程**的忽视。这是有理可依的：编程语言的学习本身就和传统的课堂授课模式存在较大的出入，**编程重视实践**，枯燥的语法讲解如同天书一般晦涩难懂，更不用提OOP所涉及的都是比较**大规模的项目工程**，如果只是在课堂上乏味地讲解“什么是析构函数，什么是继承，什么是多态···”，很容易将C++学成死记硬背的无聊学科。{% endnote %}

因此，笔者希望通过博客的方式，记录自己**OOP in CPP**的学习笔记，并分享给各位小伙伴！

# Why OOP?

- 面向过程的局限性：将描述事物的数据与处理数据的函数分开。
  - 若所描述事物的数据结构发生变化时，这些成员函数也必须重新设计！  
  - 在主函数中对数据进行修改，仅仅需要执行一条赋值语句，数据安全性得不到保障！

- 面向对象的优势：将描述事物的数据与处理函数**封装**成一个整体，称为类。  
  - 封装在类中的函数和数据不受外界的影响，即类使数据具有良好的**独立性**和**可维护性**！
  - 类中的数据在类的外部不能直接调用，外部只能通过**公共接口函数**来处理类中的数据，从而保障了**数据的安全性**！


# 类和对象
## 类：
&emsp;&emsp;类描述了某一类事物应该具有哪些**特征**和**行为**。比如我们想描述一个“商品”类别，类就会告诉我们商品的名称、编号、进货价格、售出价格等特征，但是这个“商品”并不是一真正的商品，类只告诉了我们“商品”是什么样的、应该具有的特征。类是对象的抽象表示，它**本身不占用内存空间**。

类```class```与结构体```struct```形式相似，关键字不同！

- ```class```的成员默认是```private```的
- ```struct```的成员默认是```public```的

```cpp
  class 类名 {
     private:
	//私有数据成员和成员函数
     protected:
	//保护数据成员和成员函数
     public:
	//共有数据成员和成员函数
};
```

- ```private```:只能被类本身的成员函数、友元函数、友元函数的成员函数访问，派生类也无法访问。
- ```protected```:除派生类可以进行访问，其余与```private```相同。
- ```public```:可以被程序中任意代码访问。


```cpp
class cgoods{//商品类
     private:
        string ID; //商品编号
        string name; //商品名称
        double Purchasingprice; //进货价格
        double Sellingprice; //售出价格
        int SellCount; //售出数量
        static double Profit; //总利润
     protected://无
     public:
        cgoods(string id, string name, double purchasingprice){//构造函数
        //函数体
        }
        ~cgoods(){//析构函数
        } 
        void SetPurchasingprice(double purchasingprice){//设置进货价格
        //函数体
        }
        void SetSellingprice(double sellingprice){//设置出货价格
        //函数体
        }
        void setSellingcount(int sellcount){//设置出货商品价格
        //函数体
        }
        void Sell(double sellingprice,int sellcount){
        //函数体
        }
        static double getProfit(){//获取总利润，静态成员函数
        //函数体
        }
        void display(){
        //函数体
        }
}
```

## 对象：
&emsp;&emsp;对象则是根据类创建出来的具体实例。比如我们创建一个叫“手机”的“商品”，他有自己的具体的编号、进货价格、售出价格等，所有的特征都是独一无二的，不会与其他商品相同。每一个对象都是根据类创建的具有其属性和方法的实例，并且具有唯一的身份标识（如内存地址）和自己独特的属性值。 **对象是占用内存空间的**，它的**属性值可以在运行时动态地改变**。

定义对象的三种方法：
- 先定义类的类型，再定义对象：
 ```cpp
 class 类名{
     成员表;
 };
 [class]可选 类名 对象名列表;
 ```
例如：
 ```cpp
 class cgoods goods1("1001001","元气森林");//创建对象goods1
 cgoods goods("1001001","元气森林");//两种方法等价
 ```
- 在定义类类型时**同时创建对象**：
```cpp
 class 类名{
     成员表;
 }对象名表;
 ```
例如：
 ```cpp
 class cgoods{
     private:
     public:
 }goods1("1001001","元气森林")；
 ```
- 不出现类名**直接定义对象**：
```cpp
class {
     成员表;
 }对象名表;
```
例如：
```cpp
 class {
     private:
     public:
 }goods1("1001001","元气森林")；
 ```
此方法由于没有类名，所以只能**一次性声明多个对象**，此后再无法声明此类对象！

## 类的成员访问

- 对于数据成员的访问
```cpp
对象名.成员名//数据成员访问
对象指针名->成员名
(*对象指针名).成员名
```
- 对于成员函数的访问
```cpp
对象名.成员函数名(参数表)//成员函数访问
对象指针名->成员函数名(参数表)
(*对象指针名).成员函数名(参数表)
```

## 类的构造函数(Constructor)

- 类的对象太过复杂，一个对象可能有许许多多的数据成员，这就意味着我们要对许许多多的数据成员进行初始化，实现这一过程并不容易。构造函数的作用就是在对象被创建时**利用特定的初始值**构造对象，把对象**置于某一个初始状态**。

1. 有**与类完全相同的名字** 
2. **没有类型说明**，不允许有返回值
3. **可以进行重载**，即一个类中允许定义多个参数不同的构造函数 
4. 可以在声明时的**参数表里给予初始值**
5. 每个类都必须至少有一个构造函数，如果没有显式的为类提供构造函数，则C++**提供一个默认的无参构造函数**，只负责对象的创建，而不做任何初始化的工作
6.  一旦类定义了构造函数，C++不再提供默认的无参构造函数
7.  程序中不能直接调用构造函数，他是在**创建类的对象时自动调用**的

***

### 1. 无参构造函数
```cpp
#include <iostream>
using namespace std;

class Time {
public:
    Time() { // 定义构造成员函数，函数名与类名相同
        hour = 22;
        minute = 22;
        sec = 22;
    }
    void set_time();  // 函数声明
    void show_time(); // 函数声明
private: // 私有数据成员
    int hour;
    int minute;
    int sec;
};

void Time::set_time() { // 定义成员函数，向数据成员赋值
    cin >> hour;
    cin >> minute;
    cin >> sec;
}

void Time::show_time() { // 定义成员函数，输出数据成员的值
    cout << "时间为: " << hour << ":" << minute << ":" << sec << endl;
}

int main() {
    Time t1;       // 建立对象t1，同时调用构造函数t1.Time()
    t1.set_time(); // 对t1的数据成员赋值
    t1.show_time(); // 显示t1的数据成员的值
    Time t2;       // 建立对象 t2，同时调用构造函数 t2.Time()
    t2.show_time(); // 显示t2的数据成员的值
    return 0;
}
```
```
12 30 40
时间为: 12:30:40
时间为: 22:22:22
```

### 2. 含参构造函数

```cpp
构造函数名（类型1 形参1，类型2 形参2，...）
类名 对象名（实参1，实参2，...）
```
```cpp
#include <iostream>
using namespace std;

class Cuboid {
public:
    Cuboid(int, int, int);  // 带有三个参数的构造函数
    int volume();
private:
    int height;
    int width;
    int length;
};

// 构造函数的实现
Cuboid::Cuboid(int h, int w, int len) {
    height = h;
    width = w;
    length = len;
}

// volume成员函数的实现
int Cuboid::volume() {
    return (height * width * length);
}

int main() {
    Cuboid cuboid1(15, 45, 30);  // 定义对象时需要根据构造函数形参提供实参
    cout << "cuboid1的体积为: " << cuboid1.volume() << endl;

    Cuboid cuboid2(10, 30, 22);
    cout << "cuboid2的体积为: " << cuboid2.volume() << endl;

    return 0;
}
```

```
cuboid1的体积为: 20250
cuboid2的体积为: 6600
```

### 3. 构造函数重载
  定义多个构造函数以便给对象提供不同的初始化方法，这些构造函数具有相同的名字而**参数的个数或参数的类型不相同**。可以为一个类声明的构造函数的个数是**无限制的**，只要每个构造函数的**形参表是唯一的**，定义对象时会根据提供的实参决定调用哪一个构造函数。
```cpp
#include <iostream>
using namespace std;

class Cuboid {
public:
    Cuboid();                  // 默认构造函数（无参数）
    Cuboid(int h, int w, int len);  // 带参数的构造函数
    int volume();              // 计算体积方法
    
private:
    int height;   // 高度
    int width;    // 宽度
    int length;   // 长度
};

// 默认构造函数实现
Cuboid::Cuboid() {
    height = 15;
    width = 15;
    length = 15;
}

// 带参数的构造函数实现
Cuboid::Cuboid(int h, int w, int len) {
    height = h;
    width = w;
    length = len;
}

// 体积计算方法实现
int Cuboid::volume() {
    return (height * width * length);
}

int main() {
    // 创建对象cuboid1，使用默认构造函数
    Cuboid cuboid1;
    cout << "cuboid1的体积为: " << cuboid1.volume() << endl;
    
    // 创建对象cuboid2，使用带参数的构造函数
    Cuboid cuboid2(20, 30, 45);
    cout << "cuboid2的体积为: " << cuboid2.volume() << endl;
    
    return 0;
}
```
```
cuboid1的体积为: 3375
cuboid2的体积为: 27000
```

### 4. 使用默认值的构造函数
```cpp
#include <iostream>
using namespace std;

class Cuboid {
private:
    int height;   // 高度
    int width;    // 宽度
    int length;   // 长度
public:
    Cuboid(int h = 15, int w = 15, int len = 15); // 构造函数，全部参数带默认值
    int volume(); // 计算体积
};

// 构造函数实现（定义时可不再指定默认值）
Cuboid::Cuboid(int h, int w, int len) {
    height = h;   
    width = w;
    length = len;
}

// 计算体积方法实现
int Cuboid::volume() {
    return (height * width * length);
}

int main() {
    Cuboid cuboid1; // 没有给出实参，使用全部默认值 height=15,width=15,length=15
    cout << "cuboid1的体积为: " << cuboid1.volume() << endl;

    Cuboid cuboid2(25); // 只给定一个实参，height=25,width=15,length=15
    cout << "cuboid2的体积为: " << cuboid2.volume() << endl;

    Cuboid cuboid3(25, 40); // 只给定2个实参，height=25,width=40,length=15
    cout << "cuboid3的体积为: " << cuboid3.volume() << endl;

    Cuboid cuboid4(25, 30, 40); // 给定3个实参，height=25,width=30,length=40
    cout << "cuboid4的体积为: " << cuboid4.volume() << endl;

    return 0; 
}
```

```
cuboid1的体积为: 3375
cuboid2的体积为: 5625
cuboid3的体积为: 15000
cuboid4的体积为: 30000
```

### 5.子对象和构造函数
在定义一个新的类时，**将一个已有类作为数据成员**，这个类对象叫做子对象。我们通过调用子对象成员的构造函数来完成对子对象的初始化。
```cpp
#include <iostream>
using namespace std;

class Rectangle {  // 定义矩形类
private:
    int Width, Length;  // 宽度、长度
public:
    Rectangle(int w, int len) {  // 带参构造函数
        Width = w;
        Length = len;
    }
    int Area() {  // 计算面积
        return (Width * Length);
    }
};

class Cuboid {  // 定义长方体类
private:
    int Height;  // 高度
    Rectangle r;  // 使用Rectangle类对象作为成员
public:
    Cuboid(int w, int len, int h) : r(w, len) {  // 初始化列表初始化r对象
        Height = h;  // 原代码这里是Height-h，应该是赋值=
    }
    int Volume() {  // 计算体积
        return (Height * r.Area());
    }
};

int main() {
    Cuboid c1(10, 20, 100);  // 创建长方体对象
    cout << "长方体 c1 的体积是: " << c1.Volume() << endl;
    return 0;
}
```

```
长方体 c1 的体积是: 20000
```

```cpp
 Cuboid(int w, int len, int h) : r(w, len) {  // 初始化列表初始化r对象
        Height = h;  // 原代码这里是Height-h，应该是赋值=
    }
```
实参10、20通过`w`和`len`赋值`r(w, len)`，调用类的成员`Rectangle(int w, int len)`构造函数完成初始化，实参100通过`Height = h`完成对`Cuboid`的初始化。

### 6.拷贝构造函数

设计拷贝构造函数**实现类中的一个对象给另一个对象的每个非静态数据成员赋值**。（**用已经初始化的对象去初始化一个新定义的对象**）


1. 拷贝构造函数的函数名必须**与类名一致**，函数的形式参数是本类型的一个**引用变量**，必须为**引用**！
2. **自定义拷贝构造函数**，能够实现**有选择**的复制原对象中的数据（实现对部分数据的修改）
```cpp
#include <iostream>
using namespace std;

class Sample {        // 定义Sample类
private:
    int nTest;        // 私有成员变量，用于存储测试值
public:
    // 构造函数，初始化nTest
    Sample(int ly) {  // 参数ly用于初始化nTest
        nTest = ly;   // 将参数值赋给成员变量
    }
    
    // 自定义的拷贝构造函数
    Sample(Sample &tS) {  // 参数是对另一个Sample对象的引用
        cout << "拷贝构造函数被调用" << endl;  // 输出提示信息
        nTest = tS.nTest + 8;  // 新对象的nTest值 = 原对象值 + 8
    }
    
    // 获取nTest值的成员函数
    int readtest() {
        return nTest;  // 返回当前对象的nTest值
    }
    
    // 设置nTest值的成员函数
    void settest(int ly) {
        nTest = ly;    // 修改当前对象的nTest值
    }
};

int main() {
    Sample S1(100);    // 创建S1对象，nTest初始化为100
    Sample S2(S1);     // 使用拷贝构造函数创建S2对象
    cout << S2.readtest() << endl;  // 输出S2的nTest值
    return 0;
}
```

```
拷贝构造函数被调用
108
```

## 类的析构函数(Destructor)

相当于创建对象时用new申请了一片内存空间，应在退出前**在析构函数中用delete释放**。析构函数是与构造函数作用相反的函数，当对象生命周期结束时，自动执行析构函数。

1. 有与类完全相同的名字，只是在**函数名前面加一个位取反符“~”**，以区别于构造函数
2. 不带任何参数，没有返回值
3. 一个类最多只能有一个析构函数，**无法进行重载**
4. 如果用户没有编写析构函数，编译系统会**自动的生成一个默认的析构函数**

```cpp
class 类名{
    public:
    ~类名();{//析构函数
    //函数体
    }
};
```

```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
private:
    //私有数据成员
    string name;    //姓名
    int age;        //年龄
    char gender;    //性别，'f'女性，'m'男性
    string idNumber; //身份证号码

public:
    Person(string, int, char, string);
    Person(Person&);
    ~Person();
    string getName();  // 函数声明
    void showInfo();   // 函数声明
};

// 成员函数在类体外实现
Person::Person(string theName, int theAge, char theGender, string theIdNumber) {
    name = theName;
    age = theAge;
    gender = theGender;
    idNumber = theIdNumber;
    cout << name << " Constructor called." << endl;
}

Person::Person(Person& theObject) {
    name = theObject.name;
    age = theObject.age;
    gender = theObject.gender;
    idNumber = theObject.idNumber;
    cout << "Copy Constructor called." << endl; //输出有关信息
}

Person::~Person() { //析构函数
    cout << name << " Destructor called." << endl;
}

string Person::getName() {
    return name;
}

void Person::showInfo() {
    cout << "name: " << name << endl;
    cout << "age: " << age << endl;
    cout << "gender: " << gender << endl;
    cout << "id number: " << idNumber << endl;
}

int main() {
    Person p1("张三", 12, 'm', "12345200006061111"); //建立对象p1
    Person p2("李四", 31, 'f', "12345198111091234"); //建立对象p2

    p1.showInfo();
    p2.showInfo();

    return 0;
}
```
```
张三 Constructor called.
李四 Constructor called.
name: 张三
age: 12
gender: m
id number: 12345200006061111
name: 李四
age: 31
gender: f
id number: 12345198111091234
李四 Destructor called.
张三 Destructor called.
```
**注意观察输出：构造函数和析构函数调用的顺序！**
{% note warning %}
1. 析构函数在对象作为函数值返回之后被调用。{% endnote %}
   
## 构造函数和析构函数调用顺序
一般而言，调用构造函数的次序与调用析构函数的**次序相反**，与栈类似：**先调用构造函数的对象，最后调用析构函数**。


### 特殊情况：
{% note warning %}
1. **全局定义**对象（函数体外定义的对象）：程序开始之前调用构造函数，程序结束或调用exit()函数时调用析构函数。
2. **局部定义**的对象（函数体内定义的对象）：程序执行到定义对象的地方时调用构造函数，函数结束时调用析构函数。
3. **static定义**的对象：在首次到达对象定义位置时调用构造函数，程序结束时调用析构函数。
4. **new动态生成**的对象：产生对象时调用构造函数，用delete释放对象时，才调用析构函数。若不使用delete运算符来撤销动态生成的对象，则析构函数不会被调用。{% endnote %}



## 对象的动态建立和释放
- new运算符建立对象：**先为类的对象分配内存空间**，然后**自动调用构造函数初始化**对象的数据成员，最后将变量的起始地址返还给指针变量。
- delete运算符释放对象：**只有在delete运算符释放对象时，才会调用析构函数将对象销毁**。
```cpp
#include <iostream>
using namespace std;

// 定义一个复数类
class Complex {
public:
    // 构造函数：初始化实部和虚部
    Complex(double r, double i) {
        real = r;    // 设置实部值
        imag = i;    // 设置虚部值
        cout << "构造函数被调用" << endl;
    }

    // 析构函数：对象销毁时自动调用
    ~Complex() {
        cout << "析构函数被调用" << endl;
    }

    // 显示复数的方法
    void display() {
        cout << "(" << real << "," << imag << ")" << endl;
    }

private:
    double real;  // 复数的实部
    double imag;  // 复数的虚部
};

int main() {
    // 动态创建一个Complex对象
    Complex* pc1 = new Complex(3, 4);

    // 调用display方法显示复数
    pc1->display();  // 等价于 (*pc1).display();

    // 释放对象内存
    delete pc1;

    cout << "程序结束" << endl;
    return 0;
}
```
```
构造函数被调用
(3,4)
析构函数被调用
程序结束
```
## 静态成员
声明为static的类成员称为静态成员，可以**被类的所有对象共享**。
- 静态数据成员：描述这一类对象所共有的数据，所有对象公用这一部分存储空间。
- 静态数据函数

eg:Profit和 static double getProfit() ，总利润是出售所有商品获得的，并不隶属于哪一个商品对象。

### 为什么不使用全局变量？
{% note warning %} 
1. **违背了OOP封装性的精神**，任何地方都可以对全局变量进行访问，破坏了信息隐藏原则
2. 过多使用全局变量会产生**重名冲突**
3. 能够**明确归属**，直接表明它是类的一部分，便于进行初始化{% endnote %}

### 静态数据成员

在类的定义中的数据成员声明前加上关键字`static`，表示该成员是静态数据成员。由于静态数据成员**由类的所有对象共享**，所以静态成员的存储空间**不会随着对象的产生而分配**，也**不会随着对象的消失而释放**，因此静态数据成员不能在类体内进行初始化，而只能在**类体内进行声明**，在**类体外进行初始化**。

```
数据类型名类名::静态数据成员名=初值;
```

**注意**：
- **不需要加`static`关键字**
- **需要通过作用域运算符`::`限定修饰**

```cpp
class cgoods{ 
    private:
    ......
    static double Profit;
    public:
    ......
}
double cgoods::Profit=0;
```

类外的定义是必要的，若没有明确赋初值，则编译系统会自动赋初值为0。

### 静态成员函数
与类的数据成员相同，在成员函数前加上`static`可以创建一个静态成员函数。静态函数没有`this`指针，通常他只访问属于全体对象的成员————即静态成员。
```cpp
double cgoods::getProfit(){
    return Profit;//使用了静态成员变量
}
```
{% note warning %}
1. 非静态成员函数可以任意地访问静态成员函数和静态数据成员
2. 静态成员函数不能直接访问非静态成员函数和非静态数据成员{% endnote %}

### 静态成员的访问

用类的对象访问 || 直接用作用域运算符“::”通过类名访问

```cpp
类名::静态数据成员名
```

```cpp
对象名.静态数据成员名
//容易让人误认为静态数据成员是属于某个对象的
```
静态成员函数的访问与静态成员数据的访问的形式相同，不做过多阐释。

```cpp
#include <iostream>
#include <string>
using namespace std;

class CStudent {
private:
    string SName;       // 保存学生姓名
    float Score;        // 保存学生的成绩
    static int studentTotal;    // 静态数据成员，保存学生的总人数
    static float SumScore;      // 静态数据成员，保存所有学生的成绩和

public:
    // 构造函数，当新建一个对象时，人数studentTotal加1
    CStudent(string name, float sc);
    static float average();     // 计算学生的平均分
    void Print();               // 打印输出学生的姓名和分数
    ~CStudent();                // 析构函数，当减少一个对象时，studentTotal减1
};

// 静态数据成员的初始化必须在类外进行
int CStudent::studentTotal = 0;
float CStudent::SumScore = 0;

CStudent::CStudent(string name, float sc) {
    SName = name;
    Score = sc;
    studentTotal++;     // 学生人数加1
    SumScore += sc;     // 总分数增加
    cout << SName << " Constructor called." << endl;
}

void CStudent::Print() {
    cout << SName << ": " << Score << endl;
}

float CStudent::average() {     // 静态成员函数访问静态数据成员
    if (studentTotal == 0) return 0;  // 防止除以0
    return (SumScore / studentTotal);
}

CStudent::~CStudent() {
    studentTotal--;     // 学生人数减1
    SumScore -= Score;  // 总分数减少
    cout << SName << " Destructor called." << endl;
}

int main() {
    // 简单测试
    CStudent stud1("Zhang San", 90);
    CStudent stud2("Li Si", 80);
    stud1.Print();
    stud2.Print();
    cout << "平均分为: " << CStudent::average() << endl;  // 调用静态成员函数
    return 0;
}
```

```
Zhang San Constructor called.
Li Si Constructor called.
Zhang San: 90
Li Si: 80
平均分为: 85
Li Si Destructor called.
Zhang San Destructor called.
```

## this指针
- 用途：当成员函数的**参数名与成员变量名相同**的时候，可以使用`this`来**明确地引用成员变量**
```cpp
Ponit& setPoint(int x,int y){
    this->x=x;
    (*this).y=y+8;
    return *this;
}
```
{% note warning %}
1. this指针是一个**指向对象的指针**
2. this指针是一个隐含于成员函数中的对象指针
3. this指针是一个指向正在调用成员函数的对象的指针
4. **类的静态成员函数没有this指针**{% endnote %}

## 常对象
常对象用`const`进行修饰，常对象必须进行初始化，且不能被更新，常对象的声明如下（两种声明完全相同，没有任何区别）：
```cpp
const 类名 对象名[(实参列表)]；
类名 const 对象名[(实参列表)]；
```
```cpp
const Point P1(1,1);
Point const P2(2,2);
```
以上定义了两个常对象P1、P2。在任何场合，对象P1、P2中的成员值不能进行修改。
常对象不能调用非const成员函数：
```cpp
int Area() const{//常成员函数
    return x*y;
}
``` 
如果一定要修改常对象中的数据成员，可将需要修改的数据成员声明为`mutable`，这样就可以用声明为`const`的成员函数来修改它的值了！

## 友元类
若我们想要一个不属于某个类的函数存取该类中的数据：
1. 将类中的数据成员均设置为`public`
2. **在类内部声明**这个函数为友元（friend），则这个函数可以访问该类的私有成员

第一点有违OOP封装性的精神，显然第二种更好！

```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
private: 
    string name;  
    int age;      
    char gender;  
    string idNumber; 

public:
    Person(string, int, char, string);
    Person(Person&);
    ~Person() {}
    string getName();  // 函数声明
    friend void showInfo(Person& p);  // 声明showInfo为类Person的友元函数
};

Person::Person(string theName, int theAge, char theGender, string theIdNumber) {  
    name = theName;
    age = theAge;
    gender = theGender;
    idNumber = theIdNumber;  
}

Person::Person(Person& theObject) {
    name = theObject.name;
    age = theObject.age;  
    gender = theObject.gender;  
    idNumber = theObject.idNumber;  
}

string Person::getName() {
    return name;
}

void showInfo(Person& p) {  // showInfo为普通函数，是类Person的友元函数
    cout << "name: " << p.name << endl;  
    cout << "age: " << p.age << endl;
    cout << "gender: " << p.gender << endl;  
    cout << "id number: " << p.idNumber << endl; 
}

int main() {
    Person p1("张三", 12, 'm', "12345200006061111");  // 建立对象p1
    showInfo(p1);
    return 0;
}
```

```cpp
name: 张三
age: 12
gender: m
id number: 12345200006061111
```

#### 注意：
{% note warning %}
- 友元函数是类外函数，友元函数不能直接访问类中的私有和保护成员，而**需要通过对象参数进行访问**{% endnote %}

这个案例显然并没有那么好，我们可以将`showInfo`函数设计为类内一个普通的成员函数，这没有显示出友元函数的必要性，仅仅是对友元函数用法的一个初步介绍！

### 友元函数是另一个类的成员函数
```cpp
#include <iostream>
using namespace std;

class Rectangle;  // 前向声明

class Cuboid {
private:
    int Height;
public:
    Cuboid(int h) : Height(h) {}  // 使用初始化列表
    int Volume(Rectangle& r);     // 只能声明，不能定义（因为Rectangle未完全定义）
};

class Rectangle {
private:
    int Width, Length;
public:
    Rectangle(int w, int len) : Width(w), Length(len) {}  // 初始化列表
    friend int Cuboid::Volume(Rectangle& r);  // 声明友元函数
};

// 必须在 Rectangle 定义之后才能定义 Volume
int Cuboid::Volume(Rectangle& r) {
    return r.Length * r.Width * Height;  // 访问 Rectangle 的私有成员
}

int main() {
    Rectangle R(6, 8);
    Cuboid C(20);
    cout << "长方体的体积为：" << C.Volume(R) << endl;
    return 0;
}
```

```
长方体的体积为：960
```

这里将类`Cuboid`的成员函数`Volume()`声明为类`Rectangle`的友元函数，这样在`Volume()`中就可以使用Rectangle中的私有数据成员`Width`、`Length`。

#### 注释：
{% note warning %}
程序第三行对`Rectangle`的**提前声明引用**，只包含类名，不包含类体。提前声明的原因是：在类`Cuboid`中调用`Volume()`函数时，需要使用类`Rectangle`中的数据成员`Length`和`Width`，但是类`Rectangle`还没有定义。那如果将`Rectangle`的定义提到前面呢？同样是不可以的，因为在类`Rectangle`中又包含了`Cuboid`的成员！但是不能因为提前声明，而去定义一个对象！{% endnote %}
例如：

```cpp
class Rectangle;//提前引用声明
Rectangle r1;//紧接着定义一个Rectangle对象，这是不允许的！
class Rectangle{...};
```

### 友元类
将一个类声明为另一个类的友元：
```cpp
class A{
    ...
    friend class B;//类B声明为当前类A的友元类
    ...
};
```
此时，类B中的所有成员函数都是当前类A的友元函数，因此类B中的**所有成员函数**都可以访问当前类A的`private`成员或`protected`成员
```cpp
#include <iostream>
using namespace std;

class Rectangle; // 前向声明

class Cuboid {
private:
    int Height;
public:
    Cuboid(int h) {
        Height = h;
    }
    int Volume(Rectangle& r);
};

class Rectangle {
private:
    int Width, Length;
public:
    Rectangle(int w, int len) {
        Width = w;
        Length = len;  // 添加了分号
    }
    friend class Cuboid; // 声明类Cuboid是类Rectangle的友元类
};

int Cuboid::Volume(Rectangle& r) {
    return r.Length * r.Width * Height;  // 正确的体积计算公式
}

int main() {
    Rectangle r(6, 8);
    Cuboid C(20);
    cout << "长方体的体积为：" << C.Volume(r) << endl;
    return 0;
}
```

```
长方体的体积为：960
```

### 友元关系的限制：
{% note warning %}
1. 友元关系**不具有传递性**，“附庸的附庸不是我的附庸”，比如类A是类B的友元类，类B是类C的友元类，类C不是类A的友元类。
2. 友元关系**不具有交换性**，比如类A是类B的友元类，类B不一定是类A的友元
3. 友元关系是**不能继承的**，比如类A是类B的友元类，类C继承类B，类C不是类A的友元类{% endnote %}


封面来源：[Fundamental Concepts of Object Oriented Programming](https://www.youtube.com/watch?v=m_MQYyJpIjg)

## References
以上大部分代码均取自《C++程序设计基础教程》