---
title: Essential Sorting Algorithms Explained
date: 2025-07-24 23:28:06
tags: [sort,algorithm]
index_img: /medias/Essential-Sorting-Algorithms-Explained.png
category: Data Structure
category_bar: true
---

This article explores some of the most essential sorting algorithms in Datastructure!
<!--more-->

## 题目
文章中排序算法均已经通过此题的OJ测试点！

#### 题目描述
给你N个自然数，编程输出排序后的这N个数。

#### 输入
第一行是整数的个数N（N<=100）。第二行是用空格隔开的N个数。

#### 输出
排序输出N个数，每个数间用一个空格间隔。

#### 样例输入
```
5
9 6 8 7 5
```
#### 样例输出
```
5 6 7 8 9
```

*** 
![图1 排序算法图](images\Essential-Sorting-Algorithm-Explained\图片1.png)

## 1.直接插入排序（Straight Insertion Sort）
### 核心思想
&emsp;&emsp;将待排数组分为“已排序”和“未排序”两个部分，`R[0,1...i-1]`前面序列是已经排好的有序区，`R[i,...n]`后面的序列是未排序的无序区，直接插入排序每次操作将当前无序区的首元素`R[i]`插入到有序区`R[0,1...i-1]`的适当位置，使得`R[0,1...i]`成为新的有序区，减小无序区,直至无序区为空，从而全部数据有序！
### 完整代码
```cpp
#include <iostream>
#include <vector>
using namespace std;
bool cmp(int a, int b) { return a < b; }
int main() {
    int N;
    cin >> N;
    vector<int> R;
    for (int i = 0; i < N; i++) {
        int a;
        cin >> a;
        R.push_back(a);
    }
    for (int i = 1; i < N; i++) {
        int temp = R[i];//用temp临时存储待排元素
        int j = i - 1;//让temp从i-1开始逐个向前比较
            while (j >= 0 && cmp(temp, R[j])) {
                R[j + 1] = R[j];
                j--;
            }
            R[j + 1] = temp;
    }
    for (int i = 0; i < N; i++) {
        cout << R[i] << " ";
    }
    return 0;
} 
```
这里将比较逻辑模块化：
```cpp
bool cmp(int a, int b) { return a > b; }  
```
若未来需要修改排序规则（降序排序）只需要将`cmp`函数中的`>`修改为`<`即可！
### 算法分析
&emsp;&emsp;直接插入排序由**两重循环**构成，对于具有`n`个元素的数组`R`，外循环要进行`n-1`趟排序（`1到n-1`），在每趟排序中，仅当待插入序列元素`R[i]`小于有序区尾元素时才进入内层循环，因此直接插入排序的**时间性能与初始排序表相关**。

- 比较次数 
- 元素移动次数 

1. 最好情况分析：初始排序表正序，无需进入内层循环时间复杂度为`O(n)`
2. 最坏情况分析：初始排序表反序，每次排序均需要进入内层循环进行`i`次比较，等差数列`n(n-1)/2`，时间复杂度为`O(n^2)`
3. 平均情况分析：在每趟排序中，平均情况是将`R[i]`插入到有序区的中间位置`R[0,1...i-1]`，等差数列`n(n-1)/4`，时间复杂度为`O(n^2)`。

&emsp;&emsp;由于其**平均时间性能接近最坏性能**，所以是一种**低效**的排序方法。在该算法中只使用了`i`,`j`,`temp`三个辅助变量，与问题规模`n`无关，故空间复杂度为`O(1)`，是一个**就地排序**算法，同时相等时排序不变，是种**稳定**的排序算法。

***
## 2.折半插入排序（Binary Insertion Sort）
### 核心思想
&emsp;&emsp;在直接插入排序的基础上，用折半查找的方法找到无序区元素插入的位置。
### 完整代码
```cpp
#include <iostream>
#include <vector>
using namespace std;
int main() {
    int N;
    cin >> N;
    vector<int> R; 
    for (int i = 0; i < N; i++) {
        int a;
        cin >> a;
        R.push_back(a); 
    }
    for (int i = 1; i < N; i++) {
        int temp = R[i];
        int low = 0, high = i - 1;
        while (low <= high) {//退出循环时low=high+1
            int mid = (low + high) / 2;
            if (temp > R[mid]) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }
        for (int j = i - 1; j >= high + 1; j--) {  
            R[j + 1] = R[j];
        }
        R[high + 1] = temp;
    }
    for (int i = 0; i < N; i++) {
        cout << R[i] << " ";
    }
    return 0;
}
```
### 算法分析
&emsp;&emsp;平均情况下时间复杂度为`O(n^2)`,从时间复杂度来看，折半插入与直接插入排序相同，但是当**元素数量较多时，折半查找优于顺序查找**，减少了关键字比较的次数，所以折半插入排序优于直接插入排序。同时其空间复杂度为`O(1)`，也是种**稳定**的排序算法。
*** 
## 3.希尔排序（Shell Sort）
### 核心思想
&emsp;&emsp;希尔排序是一种**采用分组插入排序**的方法，先取一个小于`n`的整数${d}_{1}$作为第一个增量，将全部元素`R`中所有相距为 ${d}_{1}$的元素分成一组，在组内进行直接插入排序，然后取第二个增量 ${d}_{2}$（${d}_{2}$<${d}_{1}$），重复上述的分组和排序，直至增量 ${d}_{t}$=1，即**所有的元素为一组，在进行一次直接插入排序**，从而使得所有元素有序！
&emsp;&emsp;从理论上讲，增量序列的取值只要满足初始值小于`n`再递减并且最后等于`1`就可以了。最常见的是**Shell增量序列**，即取 ${d}_{1}$=`n/2`，${d}_{i+1}$=${d}_{i}$/2，直到 ${d}_{t}$=0为止！
### 完整代码
```cpp
#include <iostream>
#include <vector>
using namespace std;
int main() {
	vector <int> R;
	int N;
	cin >> N;
	for (int i = 0;i < N;i++) {
		int a;
		cin >> a;
		R.push_back(a);
	}
	int d = N / 2;
	while (d != 0) {
		for (int i= d;i< N;i++) {
			int temp = R[i];
			int j = i;
			while (j >= d && R[j - d] > temp) {
				R[j] = R[j - d];
				j -= d;
			}
			R[j] = temp;
		}
		d /= 2;
	}
	for (int i = 0;i < N;i++) {
		cout << R[i] << " ";
	}
	return 0;
}
```
### 算法分析
&emsp;&emsp;由于希尔排序的增量序列不确定，算法的时间复杂度难以分析，我们一般认为其平均时间复杂度为`O(n^1.58)`，**希尔排序通常要比直接插入排序快**，在希尔排序中我们使用了`i`,`j`,`temp`,`d`四个辅助变量，与问题规模`n`无关，故算法空间复杂度为`O(1)`，也就是说是一种**就地排序**。但是希尔排序过程中相同元素的相对位置可能发生变化，因而是一种**不稳定**的排序算法。

***
## 4.快速排序（Quick Sort）
### 核心思想
&emsp;&emsp;在排序表中取一个元素为基准（一般是第一个），**将基准归位**（即将基准放在他最终的位置上），同时将所有小于基准的元素放到基准的前面（构成**左子表**），将所有大于基准的元素放到基准的后面（构成**右子表**），这个过程叫作**划分**。然后用递归的思想对左、右子表分别重复上述过程，直至每个子表只有一个元素或空为止。
&emsp;&emsp;快速排序每次**仅将一个元素归位**，在最后一趟排序结束前并不产生明确的连续有序区。
### 完整代码
```cpp
#include <iostream>
#include <vector>
using namespace std;
int partition(vector <int>&arr, int low, int high) {
	int base = arr[low];
	int i = low + 1, j = high;
	while (i <= j) {
		while ( i <= j && arr[i] <= base) {
			i++;
		}
		while (i <= j && arr[j] >= base) {
			j--;
		}
		if (i < j) {
			swap(arr[i], arr[j]);
			i++;
			j--;
		}
	}
	swap(arr[low], arr[j]);
	return i;
}
void quicksort(vector <int>& arr, int low, int high) {
	if (low >= high)return;
	int pi = partition(arr, low, high);
	quicksort(arr, low, pi-1);
	quicksort(arr, pi+1, high);
}
int main() {
	vector <int> R;
	int N;
	cin >> N;
	for (int i = 0;i < N;i++) {
		int a;
		cin >> a;
		R.push_back(a);
	}
	quicksort(R, 0, N - 1);
	for (int i = 0;i < N;i++) {
		cout << R[i] << " ";
	}
	return 0;
}
```
### 算法分析
1. 最好情况分析：如果初始排序表随机分布，使得**每次划分恰好分为两个长度相同的子表**，则递归树最小，性能最好，此时排序的时间复杂度为`O(nlog2n)`。
2. 最坏情况分析：如果初始排序表**正序或反序**，使得**每次划分的两个子表中一个为空**，另一个长度为`n-1`，则递归树的高度最高，性能最差，此时排序的时间复杂度为`O(n^2)`。
3. 平均情况分析：排序的平均时间复杂度为`O(nlog2n)`，这**接近最好情况**，所以快速排序是一种**高效**的排序方法。

&emsp;&emsp;快速排序使用的是**递归算法**，尽管每一次划分仅仅使用固定的几个辅助变量，但是**递归树的高度**最好为`O(log2n)`，对应最好的空间复杂度为`O(log2n)`，最坏情况下递归树的高度为`O(n)`，对应最坏的空间复杂度为`O(n)`。
&emsp;&emsp;另外，快速排序是一种**不稳定**的排序算法。（STL的`sort()`函数就是使用快速排序实现的，当划分的区间长度较小时，采用直接插入排序，所以`sort()`是不稳定的，且时间复杂度为`O(nlog2n)`）

***
## 5.堆排序（Heap Sort）
### 核心思想
&emsp;&emsp;堆排序是对**选择排序的一种改进**，采用**二叉树**来代替简单的选择方法来找最大或者最小元素，属于一种**树形选择排序方法**。我们采用数组隐式构建二叉树：
1. 小根堆：根节点小于其两个子节点，即：${k}_{i}$$\leq$${k}_{2i+1}$ 且 ${k}_{i}$$\leq$${k}_{2i+2}$，显然此时根节点是最小的。
2. 大根堆：根节点大于其两个子节点，即：${k}_{i}$$\geq$${k}_{2i+1}$ 且 ${k}_{i}$$\geq$${k}_{2i+2}$，显然此时根节点是最大的。
### 完整代码
```cpp
#include <iostream>
#include <vector>
using namespace std;
void heapify(vector<int>& arr, int n, int root) {
    int largest = root; 
    int left = 2 * root + 1; 
    int right = 2 * root + 2; 
    if (left < n && arr[left] > arr[largest])
        largest = left;
    if (right < n && arr[right] > arr[largest])
        largest = right;
    if (largest != root) {
        swap(arr[root], arr[largest]);
        heapify(arr, n, largest);
    }
}
void heapSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}

int main() {
    int N;
    cin >> N;

    vector<int> arr(N);
    for (int i = 0; i < N; i++) {
        cin >> arr[i];
    }
    heapSort(arr);
    for (int i = 0; i < N; i++) {
        if (i > 0) cout << " ";
        cout << arr[i];
    }
    cout << endl;
    return 0;
}
```
### 算法分析
&emsp;&emsp;堆排序的时间主要由**建立初始堆**和**反复重建堆**这两部分的时间构成，建立初始堆的时间复杂度为`O(nlog2n)`，后面反复归位元素和重建堆的时间复杂度为`O(nlog2n)`，因此最好、最坏、平均时间复杂度均为`O(nlog2n)`。
&emsp;&emsp;堆排序只使用了固定的几个辅助变量，其算法的空间复杂度为`O(1)`，同时是一种**不稳定**的排序算法。

***
## 6.归并排序（Merge Sort）
### 核心思想
&emsp;&emsp;通过多次将两个或两个以上的相邻有序表合并成一个新的有序表。可以分为二路归并、三路归并、多路归并排序。其中二路归并排序又可以分为**自底向上**和**自顶向下**两种方法。
&emsp;&emsp;二路归并先将`R[0...n-1]`看成`n`个长度为`1`的有序子表，然后在进行两两相邻有序子表的合并，得到`n/2`个长度为`2`的有序子表，在进行**两两有序子表的合并**，以此类推，直到得到一个长度为`n`的有序表为止。
&emsp;&emsp;二路归并时，先将两段有序**合并到一个新的局部变量**`R1`中，待合并完成后再将`R1`**复制回**`R`中。
```cpp
#include <iostream>
#include <vector>
using namespace std;
void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp(right - left + 1);
    int i = left, j = mid + 1, k = 0;
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp[k++] = arr[i++];
        }
        else {
            temp[k++] = arr[j++];
        }
    }
    while (i <= mid) {
        temp[k++] = arr[i++];
    }
    while (j <= right) {
        temp[k++] = arr[j++];
    }
    for (int p = 0; p < k; p++) {
        arr[left + p] = temp[p];
    }
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) {
        return;
    }
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);      
    mergeSort(arr, mid + 1, right); 
    merge(arr, left, mid, right); 
}
int main() {
    int N;
    cin >> N;
    vector<int> arr(N);
    for (int i = 0; i < N; i++) {
        cin >> arr[i];
    }
    mergeSort(arr, 0, arr.size() - 1);
    for (int i = 0; i < N; i++) {
        if (i > 0) cout << " ";
        cout << arr[i];
    }
    cout << endl;
    return 0;
}
```
### 算法分析
&emsp;&emsp;在二路归并排序中，长度为`n`的排序表需要做`log2n`趟排序，对应的**归并树**高度为`log2n+1`，每趟归并时间为`O(n)`，故其时间复杂度的最好、最坏、平均情况都是`O(nlog2n)`。
&emsp;&emsp;在归并排序中每次都需要用到**局部变量**`R1`，最后一趟的排序一定是全部`n`个元素参与归并，所以总的辅助空间复杂度为`O(n)`。
&emsp;&emsp;同时`Merge`算法不会改变相同关键字元素的相对次序，所以二路归并算法是一种**稳定**的排序方法！

***
有关**冒泡排序**、**选择排序**和`sort()`函数排序的相关代码在：[数据结构实验2](https://blog.csdn.net/2401_86849688/article/details/148566285?spm=1001.2014.3001.5501)中，有兴趣的可以直接传送门！
***
## 各种排序方法的比较和选择
排序方法  | 平均情况|最坏情况|最好情况|空间复杂度| 稳定性
-------- | -----|-------- | -----|-------- | -----
直接插入排序  | O(n^2)|O(n^2)|O(n)|O(1)|稳定|
  折半插入排序| O(n^2)|O(n^2)|O(n)|O(1)|稳定|
希尔排序  | O(n^1.58)|\ |\ |O(1)|不稳定|
冒泡排序  | O(n^2)|O(n^2)|O(n)|O(1)|稳定|
快速排序  | O(nlog2n)|O(n^2)|O(nlog2n)|O(log2n)|不稳定|
简单选择排序  | O(n^2)|O(n^2)|O(n^2)|O(1)|不稳定|
堆排序  | O(nlog2n)|O(nlog2n)|O(nlog2n)|O(1)|不稳定|
归并排序 | O(nlog2n)|O(nlog2n)|O(nlog2n)|O(n)|稳定|


封面来源：[Explaining EVERY Sorting Algorithm (part 1)](https://www.youtube.com/watch?v=AAwYzYkjNTg)
