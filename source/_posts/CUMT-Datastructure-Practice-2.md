---
title: CUMT-Datastructure-Practice 2
date: 2025-06-18 12:52:43
tags: [DFS,binary tree,search，graph，sort]
index_img: /medias/CUMT-Datastructure-Practice-2.png
---

数据结构实验2，涉及无向图的深度优先搜索问题问二叉树构建最小堆问题、折半查找和简单排序问题，包含重要知识点的解析、解题思路和完整代码。

 <!-- more -->

## A题：无向图的深度优先搜索
#### 题目描述
已知一个无向图G的顶点和边，顶点从0依次编号，现在需要深度优先搜索，访问任一邻接顶点时编号小的顶点优先，请编程输出图G的深度优先搜索序列。
#### 输入
第一行是整数m和n（1<m,n<100），分别代表顶点数和边数。后边n行，每行2个数，分别表示一个边的两个顶点。
#### 输出
该图从0号顶点开始的深度优先搜索序列。
#### 样例输入 
```
5 5
0 1
2 0
1 3
1 4
4 2
```
#### 样例输出 
```
0 1 3 4 2
```
#### 问题分析
使用邻接表存储图结构，读取数据后，为了保证小编号顶点优先访问，使用```sort()```	将每个邻接表中的数据升序排列，函数调用中使用栈实现非递归DFS：
将```start```压入栈内，若有邻接的顶点未被访问，将其输出，使用```visited```数组记录访问过的顶点并将其压入栈内；若其邻接的顶点均被访问，则回溯，弹出栈顶元素。
#### 完整代码
```cpp
#include <iostream>
#include <vector>
#include <algorithm>//sort()函数的调用
#include <stack>
using namespace std;
//深度优先搜索DFS函数
void DFS(vector<vector<int>>& graph, vector<bool> visited, int start) {
	stack<int> s;
	s.push(start);//将起始点压入栈内
	visited[start] = true;//标记起始点已访问
	cout << start << " ";

	while (!s.empty()) {
		int current = s.top();
		bool found = false;

		for (int neighbor:graph[current]) {
			if (!visited[neighbor]) {//如果邻接顶点未被访问
				visited[neighbor] = true;//标记为已访问
				cout << neighbor << " ";
				s.push(neighbor);//将该顶点压入栈内
				found = true;//标记已访问过的顶点
				break;//跳出循环，优先处理该顶点的邻接顶点
			}
		}
		if (!found) {//若果该顶点没有未被访问的邻接顶点
			s.pop();//弹出栈顶元素，回溯
		}
	}
}

int main() {
	int m, n;
	cin >> m >> n;//读入定点数和边数
	int a, b;
	vector<vector<int>> graph(m);
	//初始化邻接表，大小为m，每个元素是vector<int>
	for (int i = 0;i < n;i++) {
		cin >> a >> b;
		graph[a].push_back(b);
		graph[b].push_back(a);
		//用读入的数据构建邻接表，注意两边都要处理
	}
	for (int i = 0;i < m;i++) {
		sort(graph[i].begin(), graph[i].end());
		//对每个邻接表进行排序，确保DFS优先访问编号小的元素
	}
	vector<bool> visited(m, false);
	//初始化访问标记数组，大小为m，初始值为false（未被访问状态）
	DFS(graph, visited, 0);
	return 0;
}
```
#### 注释
```cpp
vector<vector<int>> graph
```
相当于一个```graph[][]```的二维数组，外层的```vector```存储所有的顶点，内层的```vector<int>```存储每个顶点的邻接顶点列表
```cpp
for (int neighbor:graph[current])
```
这是C++11 引入了范围```for```循环语法，专门用于遍历容器（如 ```vector```、```list```等），每次循环时变量会自动依次选取容器中的下一个元素。（注意中间是```:```）
## B题：最小堆的形成
#### 题目描述
现在给你n个结点的完全二叉树数组存储序列，请编程调整为最小堆，并输出相应最小堆的存储序列。
#### 输入
第一行是n，第二行是n个结点的完全二叉树数组存储序列。
#### 输出
输出相应最小堆的存储序列。
#### 样例输入 
```
8
53 17 78 23 45 65 87 9
```
#### 样例输出 

```
9 17 65 23 45 78 87 53
```
#### 问题分析
解决问题前首先要了解什么是完全二叉树、什么是最小堆以及完全二叉树有什么性质。**完全二叉树**就是除了最后一层外，其他层的节点都是满的，并且最后一层的节点都尽可能靠左排列的二叉树（从上到下、从左到右的顺序进行填充）。**最小堆**就是满足节点的值<=其子叶节点的值的完全二叉树（给定完全二叉树求最小堆时不可直接升序排列，其有特定的存储结构）。
假设完全二叉树某节点编号为```i```,则其父节点为```i/2```，其左子叶节点为```2*i```，其右子叶节点为```2*i+1```。而对于最小堆，其根节点为其全局最小值。有了这些简单的前置知识我们就可以开始解决这道题目了。
由于**下沉法**相对于**上浮法**具有更高的效率，因此本题先选择自底向上的下沉法递归求解。

#### 完整代码
```cpp
#include <iostream>
#include <vector>
using namespace std;
void down(vector<int>& tree, int n, int parent) {
//完全二叉树的性质
	int left = 2 * parent;//左子叶节点
	int right = 2 * parent + 1;//右子叶节点
	int smallest = parent;//初始化最小值为顶点
	if (left <= n && tree[left] < tree[smallest]) {
		smallest = left;
	}
	if (right <= n && tree[right] < tree[smallest]) {
		smallest = right;
	}//找出三个节点中的最小值，将其交换至父节点
    if(smallest!=parent){
		int temp = tree[smallest]; 
		tree[smallest] = tree[parent];
		tree[parent] = temp;
		//交换 也可以使用swap()更简便
		down(tree, n, smallest);
		//由于最小值上移，原位置需要递归重新确保其子树为最小堆
	}
}

int main() {
	int n;
	cin >> n;
	vector <int> tree(n+1);
	//提前为tree分配空间为n+1
    //（由于从1开始索引）
	for (int i = 1;i <= n;i++) {
		cin >> tree[i];
	}
	for (int i = n / 2;i >= 1;i--){
		down(tree, n, i);
	}
	//从最小的非子叶结点开始递归
	//反向遍历，确保处理父节点是，其子树已是最小堆结构
	for (int i = 1;i<=n;i++){
		cout << tree[i] << " ";
	}
	return 0;
}
```

## C题：折半查找次数
#### 题目描述
给你一个无重复数的有序序列，如果采用折半查找的方式，对于给定的数，需要比较几次找到，请编程实现。
#### 输入
第一行是N，表示序列中数的个数，序列最长1000，第二行是一个有序序列，第三行是要找的数x。
#### 输出
如果找到x，输出折半比较的次数，否则输出NO。
#### 样例输入
```
11
5 13 19 21 37 56 64 75 80 88 92
19
```
#### 样例输出
```
2
```
#### 完整代码
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> arr(n);
    for (int i = 0; i < n; ++i) {
        cin >> arr[i];
    }
    int x;
    cin >> x;
    int low = 0, high = n - 1;
    int cnt = 0;
    bool found = false;
    while (low <= high) {
        int mid = (low + high) / 2;
        cnt++; 
        if (arr[mid] == x) {
            found = true;
            break;
        }
        else if (arr[mid] < x) {
            low = mid + 1;
        }
        else {
            high = mid - 1;
        }
    }
    if (found)
        cout << cnt << endl;
    else
        cout << "NO" << endl;
    return 0;
}
```
## D题：N个数的排序
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
#### 问题分析
本题是最基础的的排序问题，这里采用冒泡排序、选择排序、c++封装的```sort()```排序函数三种方法进行求解。
#### 完整代码
冒泡排序算法求解：
```cpp
#include <iostream>
using namespace std;
void bubblesort(int arr[], int n) {
	for (int i = 0;i < n;i++) {
		for (int j = 0;j < n - i - 1;j++) {
			if (arr[j] > arr[j + 1]) {
				int temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
			}
		}
	}
	for (int i = 0;i < n;i++) {
		cout << arr[i] << " ";
	}
}
int main() {
	int arr[100], n;
	cin >> n;
	for (int i = 0;i < n;i++) {
		cin >> arr[i];
	}
	bubblesort(arr, n);
	return 0;
}
```
选择排序算法求解：
```cpp
#include <iostream>
#include <climits>
using namespace std;
void selectsort(int arr[], int n) {
    int begin = 0;
    while (begin < n) {
        int min = INT_MAX; 
        int tag = begin; 

        for (int i = begin; i < n; i++) {
            if (arr[i] < min) {
                tag = i;
                min = arr[tag];
            }
        }
        int temp = arr[begin];
        arr[begin] = arr[tag];
        arr[tag] = temp;
        begin++; 
    }
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
}
int main() {
	int arr[100], n;
	cin >> n;
	for (int i = 0;i < n;i++) {
		cin >> arr[i];
	}
	selectsort(arr, n);
	return 0;
}
```

STL封装的 ```sort()```排序函数求解：
```cpp 
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main() {
	vector <int> arr;
	int n;
	cin >> n;
	for (int i = 0;i < n;i++) {
		int a;
		cin >> a;
		arr.push_back(a);
	}
	sort(arr.begin(), arr.end());
	for (int i = 0;i < n;i++) {
		cout << arr[i] << " ";
	}
	return 0;
}
```
封面来源：[Tree data structures in 2 minutes](https://www.youtube.com/watch?v=Etpc_-br5rI&t=21s)
