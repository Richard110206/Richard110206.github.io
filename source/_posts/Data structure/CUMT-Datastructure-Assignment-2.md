---
title: CUMT-Datastructure-Assignment 2
date: 2025-06-18 13:02:54
tags: [stack,binary tree,algorithm]
index_img: /medias/CUMT-Datastructure-Assignment-2.png
category: Data Structure
category_bar: true
---

**数据结构作业2，涉及二叉树的构建，前序、中序、后序遍历问题，以及栈相关的问题，包含重要知识点的解析、解题思路和完整代码。**

 <!-- more -->

## A题：统计回文字符串
#### 题目描述
现在给你一个字符串S，请你计算S中有多少连续子串是回文串。
#### 输入
输入包含多组测试数据。每组输入是一个非空字符串，长度不超过5000。
#### 输出
对于每组输入，输出回文子串的个数。
#### 样例输入
```
aba
aa
```
#### 样例输出
```
4
3
```

#### 问题分析
先读取字符串```s```，采取从中心向两边进行扩展的方法，若回文字符串为奇数个字符则起始时中心均为```i```；若回文字符串为偶数个字符则起始时中心```i```和```i+1```，直至字符不匹配或者数组越界。
#### 注意点
```cpp
while (left >= 0 && right < n && s[left] == s[right])
```
和
```cpp
while (s[left] == s[right]&&left >= 0 && right < n )
```
在编译器中并不等同：应该先检查是否越界，否则可能出现未定义行为而报错
**短路求值规则**：逻辑运算符 && 会从左到右依次求值，如果前面的条件为 false，后面的条件不会被计算。第一种写法利用了短路规则，避免了非法内存访问；第二种写法则可能引发问题。
#### 完整代码
```cpp
#include <iostream>
#include <string>
using namespace std;
int countnumber(string s) {
	int n = s.size();
	int num=0;
	for (int i = 0;i < n;i++) {
		int left = i;
		int right = i;
		
		while (left >= 0 && right < n && s[left] == s[right]) {
			num++;
			--left;
			++right;
		}
		left = i, right = i + 1;
		while (left >= 0 && right < n && s[left] == s[right]) {
			num++;
			--left;
			++right;
		}
	}
	return num;
}
int main() {
	string s;
	while (cin >> s) {
	cout << countnumber(s) << endl;
}
	return 0;
}
```
## B题：构建矩阵
#### 题目描述
现请你构建一个N*N的矩阵，第i行j列的元素为i与j的乘积。（i，j均从1开始）
#### 输入
输入的第一行为一个正整数C，表示测试样例的个数。
然后是C行测试样例，每行为一个整数N（1<=N<=9），表示矩阵的行列数。
#### 输出
对于每一组输入，输出构建的矩阵。
#### 样例输入
```
2
1
4
```
#### 样例输出
```
1
1 2 3 4
2 4 6 8
3 6 9 12
4 8 12 16
```
#### 问题分析
1. 构造```printmatrix```的函数，主函数中每读入一个数，调用一次函数
2. 观察矩阵，相当于每行是一个等差数列，公差等于所在行行数，每换一次行其公差+1即可。

#### 完整代码
```cpp
#include <iostream>
using namespace std;
void matrix(int i) {
	int d = 1;
	for (int n = 1;n <= i * i && d<=i;n += d) {
		cout << n << " ";
			if (n == i * d) {
				cout << endl;
				d++;
				n = 0;
			}
	}
}
int main() {
	int n;
	cin >> n;
	int i;
	for (int j = 0;j < n;j++) {
		cin >> i;
		matrix(i);
	}
	return 0;
}
```
构造打印矩阵函数的另外一种做法，双层嵌套**for**循环，这个方法显然更加直观:
```cpp
void printMatrix(int N) {
    for (int i = 1; i <= N; ++i) {
        for (int j = 1; j <= N; ++j) {
            cout << i * j;
            if (j < N) {
                cout << " ";
            }
        }
        cout << endl;
    }
}
```
## C题：找规律填数字
#### 题目描述
小宇正在读小学，今天老师布置了几道数学题目。小宇平时上课经常不专心，这些他可发愁了，怎么办呢？看看你能不能帮帮他。
题目是给你一组有规律序列的前面5个整数，请你给出它后面跟着的5个整数，如：1,2,3,4,5,_, _, _, _, _, _。这是个等差数列，后面应该是6,7,8,9,10，就这么简单。而且现在小宇已经知道这串序列要么是等差数列，要么是等比数列或者是斐波那契数列。
#### 输入
输入包含多组测试数据。每组输入5个整数，每个数字之间隔一个空格，当5个数字都为0时输入结束。
#### 输出
对于每组输入，输出这串数列的后面5个数字，每个数字之间隔一个空格。
#### 样例输入
```
1 2 3 4 5
1 2 4 8 16
1 2 3 5 8
0 0 0 0 0
```
#### 样例输出
```
6 7 8 9 10
32 64 128 256 512
13 21 34 55 89
```
#### 注意点
参数变量较多，在```if```条件判断语句中要尽可能的将每个参数都包含进去，减小数据巧合而错判数列类型发生的可能性，博主一开始只通过三个参数就判断了数列的类型，就被特殊情况爆破了。

#### 完整代码
```cpp
#include <iostream>
using namespace std;

void predictNext(int a, int b, int c, int d, int e) {
    if (a == 0 && b == 0 && c == 0 && d == 0 && e == 0) return;

    int d = b - a;
    if (b + d == c && c + d == d && d + d == e) {
        cout << e + d << " " << e + 2 * d << " " << e + 3 * d
            << " " << e + 4 * d << " " << e + 5 * d << endl;
        return;
    }

    if (a != 0 && b != 0 && c != 0 && d != 0 && e != 0) {
        int q = b / a;
        if (a * q == b && b * q == c && c * q == d && d * q == e) {
            int next = e;
            for (int i = 0; i < 5; ++i) {
                next *= q;
                cout << next << " ";
            }
            cout << endl;
            return;
        }
    }

    if (a + b == c && b + c == d && c + d == e) {
        int x = d, y = e;
        for (int i = 0; i < 5; ++i) {
            int z = x + y;
            cout << z << " ";
            x = y;
            y = z;
        }
        cout << endl;
        return;
    }

    int x = d, y = e;
    for (int i = 0; i < 5; ++i) {
        int z = x + y;
        cout << z << " ";
        x = y;
        y = z;
    }
    cout << endl;
}

int main() {
    int a, b, c, d, e;
    while (cin >> a >> b >> c >> d >> e) {
        if (a == 0 && b == 0 && c == 0 && d == 0 && e == 0) break;
        predictNext(a, b, c, d, e);
    }
    return 0;
}
```
## D题：复原二叉树
#### 题目描述
小明在做数据结构的作业，其中一题是给你一棵二叉树的前序遍历和中序遍历结果，要求你写出这棵二叉树的后序遍历结果。
#### 输入
输入包含多组测试数据。每组输入包含两个字符串，分别表示二叉树的前序遍历和中序遍历结果。每个字符串由不重复的大写字母组成。
#### 输出
对于每组输入，输出对应的二叉树的后续遍历结果。
#### 样例输入
```
DBACEGF ABCDEFG
BCAD CBAD
```
#### 样例输出
```
ACBFGED
CDAB
```
#### 问题分析
题目给前序和中序求后序遍历结果，前序是根左右（NLR），中序是左根右（LNR），也就是说前序遍历的第一个字母是二叉树的根节点，在中序遍历根节点前面的是根的左子树，后面的是右子树，这样我们可以不断找到树的前序遍历和中序遍历结果，从而进行递归。
#### 完整代码
```cpp
#include <iostream>
#include <string>
using namespace std;
void postOrder(string pre, string in) {
	if (pre.empty()) return;
	char root;
	root = pre[0];
	int position;
	position = in.find(root);
	postOrder(pre.substr(1, position), in.substr(0, position));
	postOrder(pre.substr(position + 1), in.substr(position + 1));
	cout << root;
}
int main() {
	string pre, in;
	while (cin >> pre >> in) {
		postOrder(pre, in);
		cout << endl;
	}
	return 0;
}
```

## E题：子树的后序遍历
#### 题目描述
给你一颗二叉树的中序和后序遍历序列，请编程输出该二叉树左子树或右子树的后序遍历序列。
#### 输入
占三行，第一行表示二叉树的中序遍历序列，第二行表示后序遍历序列。用大写字母标识结点，二叉树的结点最多26个。
第三行是单个字母，L表示要求输出该二叉树的左子树的后序遍历序列，R表示要求输出该二叉树的右子树的后序遍历序列。
#### 输出
按要求输出该二叉树左子树或右子树的后序遍历序列。
#### 样例输入
```
BDCEAFHG
DECBHGFA
R
```
#### 样例输出
```
HGF
```
#### 问题分析
题目给中序和后序遍历，中序是左根右（LNR），后序是左右根（LRN），后序遍历的最后一个即为二叉树的根结点，从而在中序遍历中，根结点前面的是的左子树，后面的是右子树；由于本题输出后序遍历结果，而后序遍历结果题目已经给出，现只需计算左、右子树长度，从后序遍历中提取相应字符串长度即可。
#### 完整代码
```cpp
#include <iostream>
#include <string>
using namespace std;
int main() {
	string in, post;
	cin >> in >> post;
	char side, root;
	cin >> side;
	root=post.back();// 后序最后一个字符是根节点
	int position = in.find(root);
	int length;
	if (side == 'R') { 
		length = in.size() - position - 1;
		cout << post.substr(post.size() - length - 1,length) << endl; }
	else { 
		length = position;
		cout << post.substr(0,length); }
	return 0;
}
```
#### 注意
1. 提取字符串最后一个字符时用 ```post.back()```或```post[post.size() - 1]```
 获取最后一个字符（而不是 ```post[-1]```，C++不支持负数索引）。
2. 有关字符串```string```的函数：

用法示例     | 作用 | 注意事项
-------- | ------- | -------- | 
`str.find()` | 查找字符或子串|返回首次出现字符的位置（size_t）
`str.substr()` | 提取字符串|参数1：起始位置；参数2：长度（可选） 
`str.size()`| 获取字符串长度|返回字符数量（size_t）
`str.empty()` | 判断是否为空|返回值bool类型
`str.front()` | 获取首字符|返回char引用
`str.back()`| 获取末尾字符|返回char引用

 
## F题：迷宫问题
#### 题目描述
小明置身于一个迷宫，请你帮小明找出从起点到终点的最短路程。
小明只能向上下左右四个方向移动。
#### 输入
输入包含多组测试数据。输入的第一行是一个整数T，表示有T组测试数据。
每组输入的第一行是两个整数N和M（1<=N,M<=100）。
接下来N行，每行输入M个字符，每个字符表示迷宫中的一个小方格。
字符的含义如下：
‘S’：起点
‘E’：终点
‘-’：空地，可以通过
‘#’：障碍，无法通过
输入数据保证有且仅有一个起点和终点。
#### 输出
对于每组输入，输出从起点到终点的最短路程，如果不存在从起点到终点的路，则输出-1。
#### 样例输入
```
1
5 5
S-###
-----
##---
E#---
---##
```
#### 样例输出
```
9
```
#### 完整代码
```cpp
#include <iostream>
#include <queue>
#include <cstring>
using namespace std;

const int MAX = 100;
char grid[MAX][MAX];
int dist[MAX][MAX];
int dx[] = {0, 0, 1, -1};
int dy[] = {1, -1, 0, 0};
int n, m;

int bfs(pair<int, int> start, pair<int, int> end) {
    queue<pair<int, int>> q;
    q.push(start);
    dist[start.first][start.second] = 0;

    while (!q.empty()) {
        auto [x, y] = q.front();
        q.pop();

        if (x == end.first && y == end.second)
            return dist[x][y];

        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i], ny = y + dy[i];
            if (nx >= 0 && nx < n && ny >= 0 && ny < m && grid[nx][ny] != '#' && dist[nx][ny] == -1) {
                dist[nx][ny] = dist[x][y] + 1;
                q.push({nx, ny});
            }
        }
    }
    return -1;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int T;
    cin >> T;
    while (T--) {
        cin >> n >> m;
        pair<int, int> start, end;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                cin >> grid[i][j];
                if (grid[i][j] == 'S') start = {i, j};
                if (grid[i][j] == 'E') end = {i, j};
            }
        }

        memset(dist, -1, sizeof(dist));
        cout << bfs(start, end) << '\n';
    }
    return 0;
}
```
