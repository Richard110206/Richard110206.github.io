---
title: CUMT-Datastructure-Practice 4
date: 2025-06-27 12:57:31
tags: [hashtable,sort,queue]
index_img: /medias/CUMT-Datastructure-Practice-4.png
category: Data Structure
category_bar: true
---

**数据结构实验4，涉及哈希表、排序、优先队列相关问题，包含重要知识点的解析、解题思路和完整代码。**

 <!-- more -->

## 问题 A: 机器人王国里的路径长度
#### 题目描述
在一个机器人王国里，围绕首都分N层构建卫星城市。以首都为圆心，以路相连分出两个卫星城在第一个层，然后每个卫星城又有路相连分出两个卫星城在下一层，但每条路的长度不同。第N层的卫星城不再分出新的卫星城。现在人类只探知到所有直接相连的城市间的路程，你能计算某个卫星城到达首都的路程吗？
#### 输入
第一行为N，表示机器人王国里有N层卫星城，N最大为10。从第二行开始，共2N+1-2行，每行分别是城市的代号到其分出的卫星城的代号和它们间的路程。 代号用若干个字母表示，直连路程最大为100。最后一行是某卫星城的代号。
#### 输出
根据最后一行的卫星城代号，求该卫星城到首都的路程。
#### 样例输入
```
2
A F 20
B D 100
G A 5
G B 10
A C 6
B E 30
D
```
#### 样例输出
```
110
```
#### 问题分析
本题需要根据当前```target```不断向上回溯寻找其父节点，将```distance```值返回加入到总路程```sum```中即可，这里使用哈希表(unordered_map)存储每个城市的父城市和到父城市的距离。
#### 完整代码
```cpp
#include <iostream>
#include <unordered_map>//哈希表容器
#include <string>//处理字符串
#include <cmath>//这里用于幂运算
using namespace std;
struct city {
	string parent;//父城市名称
	int distance;//到父城市的距离
};
int main() {
	unordered_map<string, city> path;//存储城市路径关系的哈希表
	int N;
	cin >> N;
	int number = pow(2, N + 1) - 2;//计算读取的城市个数
	for (int i = 0;i < number;i ++) {
		string start, end;
		int distance;
		cin >> start >> end >> distance;
		path[end] = { start,distance };//进行存储
	}
	string target;
	cin >> target;
	int sum = 0;
	while (path.find(target)!=path.end()){
	//直至当前的目标城市无法查找到父城市是停止
		sum += path[target].distance;
		target = path[target].parent;//将当前目标城市向上移动
	}
	cout << sum << endl;
	return 0;
}
```
#### 注释
`<unordered_map>`是C++标准库中提供无序关联容器的头文件，它实现了基于哈希表的键值对存储结构，它具有以下特性：
+ 元素无序存储，不会根据关键字值或映射值按任何特定顺序排序
+ 键必须唯一（不允许重复键）
+ 根据关键字来引用，而不是根据索引来引用
+ 使用链表处理哈希冲突（链地址法）

构造哈希表：

``` cpp
unordered_map<KeyType, ValueType> map_name;
```

1. 元素访问：

成员函数     | 说明
-------- | -----
`map[key]`| 访问或插入元素（键不存在时自动插入并值初始化）
`map.at(key)`| 安全访问元素（键不存在时抛出out_of_range异常）
`map.find(key)`| 返回指向元素的迭代器（未找到时返回end()）

2. 容量查询：

成员函数     | 说明
-------- | -----
`map.empty()`| 判断容器是否为空
`map.size()`| 返回元素个数

3. 修改操作：

成员函数     | 说明
-------- | -----
`map.insert({key, value})`| 插入键值对（返回pair<iterator, bool>）
`map.emplace(key, value)`| 直接构造元素（避免临时对象）
`map.erase(key)`| 删除指定键的元素

insert()&&emplace()的区别：

```cpp
map.insert({"apple", 2});
```

```cpp
map.emplace("apple", 3);
 ```

4. 查找与统计

成员函数     | 说明
-------- | -----
`map.count(key)`| 返回关键字出现的次数
`map.contains(key)`| 检查键是否存在（返回bool）

## 问题 D: 寻找第二小的数
#### 题目描述
求n个整数中第二小的数。
相同的整数看成一个数。比如，有5个数分别是1,1,3,4,5，那么第二小的数就是3。
#### 输入
输入包含多组测试数据。输入的第一行是一个整数C，表示有C组测试数据；
每组测试数据的第一行是一个整数n，表示本组测试数据有n个整数（2<=n<=10），接着一行是n个整数（每个数均小于100）。
#### 输出
为每组测试数据输出第二小的整数，如果不存在第二小的整数则输出“NO”，每组输出占一行。
#### 样例输入
```
3
2
1 2
5
1 1 3 4 5
3
1 1 1
```
#### 样例输出
```
2
3
NO
```
#### 问题分析
用逆序的优先队列即可完成！
#### 完整代码
```cpp
#include <iostream>
#include <queue>
using namespace std;
void find() {
	priority_queue <int, vector<int>, greater<int>> pq;
	int m;
	cin >> m;
	for (int i = 0;i < m;i++) {
		int a;
		cin >> a;
		pq.push(a);
	}
	int min = pq.top();
	pq.pop();
	while (!pq.empty() && min == pq.top()) {
		pq.pop();
	}
	if (pq.empty()) {
		cout << "NO" << endl;
	}
	else cout << pq.top() << endl;
}
int main() {
	int n;
	cin >> n;
	for (int i = 0;i < n;i++) {
		find();
	}
	return 0;
}
```
## 问题 E: 按十进制各位和排序
#### 题目描述
对于给定的正整数序列，按照每个数的十进制形式各个位上的数之和从大到小排序，各个位上的数和相同的按照本身大小排序，大的在前，小的在后。
#### 输入
第一行 1 个整数 n,表示序列的大小。( 0 < n ≤ 1000) 第二行 n 个正整数，表示序列的每个数，每个数不大于 100000000。
#### 输出
输出按照题目要求排序后的序列。
#### 样例输入
```
6 
17 26 9 13 88 22
```
#### 样例输出
```
88 9 26 17 22 13
```
#### 问题分析
要将其十进制排序后的结果与原本大小匹配，这里使用了冒泡排序完成。
#### 完整代码
```cpp
#include <iostream>
#include <vector>
using namespace std;
int Sum(int a) {//十进制各个位数求和函数
	int sum = 0;
	while (a != 0) {
		sum += a % 10;
		a = a / 10;
	}
	return sum;
}
int main() {
	int n;
	cin >> n;
	vector <int> order(n);
	for (int i = 0;i < n;i++) {
		cin >> order[i];
	}
	for (int i = 0;i < n ; i++) {//进行冒泡排序
		for (int j = 0 ;j < n-i-1;j++) {
			if (Sum(order[j]) < Sum(order[j + 1]) ||
				(Sum(order[j]) == Sum(order[j + 1]) && order[j] < order[j + 1])) {
				int temp=order[j];
				order[j]=order[j+1];
				order[j+1]=temp;
			}
		}
	}
	for (int i = 0;i < n; i++) {
		cout<<order[i]<<" ";
	}
	return 0;
}
```
## 问题 F: 奇偶数的排序
#### 题目描述
给你10个正整数，其中5个奇数、5个偶数，先递减排奇数，然后再递增排偶数。请编程实现。
#### 输入
一行10个正整数（int类型范围）。
#### 输出
先递减排5个奇数，然后再递增排5个偶数，各个数之间有一个空格间隔。
#### 样例输入
```
1 2 3 4 5 6 7 8 10 9
```
#### 样例输出
```
9 7 5 3 1 2 4 6 8 10
```
#### 问题分析
分别使用优先队列和逆序优先队列对奇数偶数进行排序，再依次输出队首元素。
#### 完整代码
```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;
int main() {
	priority_queue<int> odd;//奇数的优先队列
	priority_queue <int, vector<int>, greater<int>>  even;//偶数逆序优先队列
	for (int i = 0;i < 10;i++) {
		int a;
		cin >> a;
		if (a % 2 == 0) {
			even.push(a);
		}
		else odd.push(a);
	}
	while(!odd.empty()) {
		cout << odd.top()<<" ";
		odd.pop();
	}
	while (!even.empty() ){
		cout << even.top()<<" ";
		even.pop();
	}
	return 0;
}
```
封面来源：: [Learn Hash Tables in 13 minutes](https://www.youtube.com/watch?v=FsfRsGFHuv4)

