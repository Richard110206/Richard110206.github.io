---
title: CUMT-Datastructure-Practice 3
date: 2025-06-24 12:57:21
tags: [binary tree,huffman treem,queue,Dijkstra,Kruskal]
index_img: /medias/CUMT-Datastructure-Practice-3.png
category: Data Structure
category_bar: true
---

**数据结构实验3，涉及链表存储二叉树、哈夫曼树、优先队列、Dijkstra算法、Kruskal算法，包含重要知识点的解析、解题思路和完整代码。**

 <!-- more -->
## 问题 A: 二叉链表存储的二叉树
#### 题目描述
树形结构是一类重要的非线性数据结构，其中以树和二叉树最为常用。对于每一个结点至多只有两棵子树的一类树，称其为二叉树。二叉树的链式存储结构是一类重要的数据结构.在本题中，将会给出一个按照先序遍历得出的字符串，空格代表空的子节点，大写字母代表节点内容。请通过这个字符串建立二叉树，并按照题目描述中的一种先序遍历和两种中序遍历的算法分别输出每一个非空节点。
#### 输入
输入只有一行，包含一个字符串S，用来建立二叉树。保证S为合法的二叉树先序遍历字符串，节点内容只有大写字母，且S的长度不超过100。
#### 输出
共有三行，每一行包含一串字符，表示分别按先序、中序、中序得出的节点内容，每个字母后输出一个空格。请注意行尾输出换行。
#### 样例输入
```
ABC  DE G  F 
  ```
#### 样例输出
```
A B C D E G F 
C B E G D F A 
C B E G D F A
 ```

#### 提示
遍历是二叉树各种操作的基础，可以在遍历的过程中对节点进行各种操作。通过二叉树的遍历，可以建立二叉树。而先序、中序和后序遍历分别具有各自的特点，是探索二叉树性质的绝佳“武器”。

#### 问题分析
本题依照题目使用二叉树链式存储结构进行二叉树的建立和遍历。
1. 设计二叉树的```struct```的节点结构，包含数据元素和左子叶节点和右子叶节点（使用构造函数给结点赋初值和初始化节点指针）。
2. 用递归的思想实现二叉树的构建，若索引值```index```非空，则构建节点，再依次递归构建左子树，递归构建右子树。
3. 递归遍历注意访问和递归的调用顺序即可。

遍历方式|访问顺序|递归调用顺序
--------- | -----------|----------|
先序遍历	|根 → 左 → 右	|访问 → 左递归 → 右递归
中序遍历	|左 → 根 → 右	|左递归 → 访问 → 右递归
后序遍历	|左 → 右 → 根	|左递归 → 右递归 → 访问

#### 完整代码
```cpp
#include <iostream>
#include <string>
using namespace std;
struct BinaryTreeNode {
	char data;
	BinaryTreeNode* left;
	BinaryTreeNode* right;
	BinaryTreeNode(char x):data(x),left(nullptr),right(nullptr){}
};
//若不构造函数则创建节点时需要:
//BinaryTreeNode* node = new BinaryTreeNode{'A', nullptr, nullptr};
BinaryTreeNode* buildtree(string str, int& index) {
	if (index >= str.size() || str[index] == ' ') {
	//若索引值超出范围或者为空
		index++;
		return nullptr;
	}
	BinaryTreeNode* node = new BinaryTreeNode(str[index++]);
	//创建新节点，递归构建左右子树
	node->left = buildtree(str, index);
	node->right = buildtree(str, index);
	return node;
};
void preOrder(BinaryTreeNode* root) {//前序遍历
	if (root == nullptr) {
		return;
	}
	cout << root->data << " ";
	preOrder(root->left);
	preOrder(root->right);
};
void inOrder(BinaryTreeNode* root) {//中序遍历
	if (root == nullptr) {
		return;
	}
	inOrder(root->left);
	cout << root->data << " ";
	inOrder(root->right);
};
//void postOrder(BinaryTreeNode* root) {后序遍历
//	if (root == nullptr) {
//		return;
//	}
//	postOrder(root->left);
//	postOrder(root->right);
//	cout << root->data << " ";
//};
int main() {
	string str;
	getline(cin,str);
	//注意用getline需要读取空格
	int index=0;
	BinaryTreeNode* root=buildtree(str, index);
	preOrder(root);
	cout << endl;
	inOrder(root);
	cout << endl;
	inOrder(root);
	cout << endl;
	//postOrder(root);
	//cout << endl;
	return 0;
}
```
## B题：哈夫曼树
#### 题目描述
哈夫曼树，第一行输入一个数n，表示叶结点的个数。需要用这些叶结点生成哈夫曼树，根据哈夫曼树的概念，这些结点有权值，即weight，题目需要输出所有叶子结点的路径长度与权值的乘积之和。
#### 输入
输入有多组数据。
每组第一行输入一个数n，接着输入n个叶节点（叶节点权值不超过100，2<=n<=1000）。
#### 输出
输出权值。
#### 样例输入
```
2
2 8 
3
5 11 30 
```
#### 样例输出
```
10
62
```
#### 问题分析
1. 将所有权值作为单独的树（每个树只有一个节点）
2. 每次选择权值最小的两棵树合并，形成新的子树，新树的权值为两子树权值之和
3. 将新树根节点作为新的子树，重复上述过程，直到只剩下一棵树
4. 计算合并过程中所有中间结果的累加和
```cpp
#include <iostream>
#include <queue>
using namespace std;
void huffman(int n) {
	priority_queue <int, vector<int>, greater<int>> minqueue;
	//创建最小优先队列
	for (int i = 0;i < n;i++) {
		int a;
		cin >> a;
		minqueue.push(a);
	}   // 读取n个权值并存入最小堆
	int total = 0;//带权路径长度
	// 隐式构建哈夫曼树
	while (!minqueue.empty()) {
		int parent = minqueue.top();
		minqueue.pop();
		//取出当前最小值
		if (minqueue.empty()) {
			break;//当队列中只有一个元素时退出循环
		}
		parent += minqueue.top();
		minqueue.pop();
		//取出次小值
		total +=  parent;
		minqueue.push(parent);
	}
	cout << total << endl;
}
int main() {
	int n;
	while (cin >> n) {
		huffman(n);
	}
	return 0;
}
```
#### 注释
**优先队列：** 这是一种特殊的队列，每次```push```进去一个数，它就会自动按照大小排好队，```top```就能得到队首元素（最大值），```pop```就会弹出队首元素（队列中最大值）。定义方法如下：
```cpp
priority_queue<int> pq;//int型优先队列
```
若我们不满足其降序排列我们还可以将其调整为升序排列：
```cpp
priority_queue <int, vector<int>, greater<int>> pq;//int型逆序优先队列
```

## C题：树的遍历
#### 题目描述
&emsp;&emsp;假设二叉树中的所有键值都是不同的正整数。唯一的二元树可以通过给定的后序和顺序遍历序列，或前序和顺序遍历序列来确定。但是，如果仅给出后序和前序遍历序列，则相应的树可能不再是唯一的。
&emsp;&emsp;现在给出一对后序和前序遍历序列，您应该输出树的相应的中序遍历序列。如果树不是唯一的，只需输出其中任何一个。
#### 输入
每个输入文件包含一个测试用例。对于每种情况，第一行给出正整数N（≤30），即二叉树中的节点总数。第二行给出预订序列，第三行给出后序序列。一行中的所有数字都用空格分隔。

#### 输出
对于每个测试用例，如果树是唯一的，则首先是行中的Yes，否则是No。然后在下一行中打印相应二叉树的中序遍历序列。如果解决方案不是唯一的，那么任何答案都可以。保证至少存在一种解决方案。一行中的所有数字必须用一个空格分隔，并且行的末尾不能有额外的空格。
#### 样例输入
```
7
1 2 3 4 6 7 5
2 6 7 4 5 3 1
```
#### 样例输出
```
Yes
2 1 6 4 7 3 5
```
#### 问题分析
&emsp;&emsp;已知前序、中序二叉树唯一确定，已知中序、后序二叉树唯一确定，而一直前序和后序二叉树无法唯一确定。
&emsp;&emsp;前序序列的第一个节点必于后续序列最后一个节点相同，为根节点；前序序列的第二个节点为左子树的根节点（如果有左子树的话），在后续序列中寻找该节点，则划分了左右子树，其左边的所有节点属于左子树，右边的节点（直到根节点之前）属于右子树。
&emsp;&emsp;因而我们可以使用前序和后序序列递归地分割左右子树，并直接生成中序序列。
而如果前序的第二个节点在后序中位于根节点前的位置，说明该节点可以是左子树或右子树的根，此时树不唯一，可以任意选择左右子树。
#### 完整代码
```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<int> in, pre, post;
//定义全局变量，分别为前序、中序、后序序列
bool unique = true;//标记二叉树是否唯一
void build(int preL, int preR, int postL, int postR) {
//分别标记了前序和后序序列的左右边界
    if (preL == preR) {
        in.push_back(pre[preL]);
        return;
    }
    if (pre[preL] == post[postR]) {
        int i = preL + 1;
        while (i <= preR && pre[i] != post[postR - 1]) i++;
        //在后序序列中寻找左子树的根节点
        if (i - preL > 1)
            build(preL + 1, i - 1, postL, postL + (i - preL - 1) - 1);
        else
            unique = false;
        in.push_back(post[postR]);
        build(i, preR, postL + (i - preL - 1), postR - 1);
    }
}
int main() {
    int n;
    cin >> n;
    pre.resize(n);
    post.resize(n);
    //为全局变量重新分配空间
    for (int& val : pre) cin >> val;
    for (int& val : post) cin >> val;
    //读入数据（注意是&索引值）
    build(0, n - 1, 0, n - 1);
    cout << (unique ? "Yes" : "No") << endl;
    if (!in.empty()) {
        cout << in[0];
        for (int i = 1; i < in.size(); ++i)
            cout << " " << in[i];
    }
    cout << endl;

    return 0;
}
```
## D题：最短路径
#### 题目描述
一个迷宫地图中，多个房间由单向通道相连，房间号从1到N依次编号。你能编程求解任意房间间的最短路径长度吗？

#### 输入
第一行是迷宫中的房间数N和单项通道数M（0<N,M<100），接下来M行，每行三个数x,y,z，表示一个通道是从x到y,且通道长度是z(z<1000）。
最后一行是start和end，分别是起点房间号和终点房间号。

#### 输出
输出起点房间号和终点房间号间的最短路径长度。如果没有通路，输出STOP。
#### 样例输入
```
7 9
1 2 3
1 3 2
3 4 2
6 3 1
2 6 3
6 7 6
2 5 4
5 4 2
5 7 5
1 7
```
#### 样例输出
```
12
```
#### 问题分析
最短路径路径问题，我们这里采用**Dijkstra算法**进行求解。
1. 构造邻接矩阵```graph```存储带权有向图。
1. 将起始顶点的距离设为0，其他所有顶点的距离```distance```设为无穷大（即从```start```到`i`的最短路径长度）。
2. 将起始顶点加入已访问集合```visited```。
3. 遍历未访问顶点，找出距离起始点最短的顶点，将其加入已访问集合。
4. 更新```distance```的距离，如果新计算的距离小于当前距离，则更新距离。
5. 重复步骤3和4，直到所有顶点都被访问过。
#### 完整代码
```cpp
#include <iostream>
#include <vector>
#include <climits>
using namespace std;
int main() {
	int M, N;
	cin >> N >> M;
	vector<int>distance(N, INT_MAX);//存放start到i点的最短路径长度
	vector<bool> visited(N, false);//记录当前顶点是否已被访问过
	vector<vector<int>> graph(N, vector<int>(N, 0));
	//有向图的邻接矩阵（二维数组）
	int p, q, length;
	for(int i = 0;i < M;i++){
		cin >> p >> q >> length;
		graph[p-1][q-1] = length;
		//均采用 0-based索引
	}
	int start, end;
	cin >> start >> end;
	visited[start - 1] = true;
	distance[start - 1] = 0;
	for (int i = 0;i < N;i++) {
		if (graph[start - 1][i] != 0) {
			distance[i] = graph[start - 1][i];
			//用start的邻接矩阵对distance数组进行初始化
		}
	}
    for(int j=0;j <N;j++){
		int small = INT_MAX;
		int tag = -1;//标记最小顶点的索引
		for (int i = 1;i <= N;i++) {
			if (distance[i - 1] < small && !visited[i - 1]) {
				small = distance[i - 1];
				tag = i;
			}
		}
		if (tag == -1) {
			break;
			//若没有找到最小索引
			//邻接矩阵已经全部遍历完成
			//退出循环
		}
		visited[tag - 1] = true;//标记已访问
		for (int i = 1;i <= N;i++) {
			if (graph[tag - 1][i - 1]!=0 && 
			distance[tag - 1] + graph[tag - 1][i - 1] < distance[i - 1]) {
			distance[i - 1] = distance[tag - 1] + graph[tag - 1][i - 1];
			//如果当前路径为最短路径则进行更新
			}
		}
	}
		if (distance[end - 1] != INT_MAX) {
			cout << distance[end-1] << endl;
		}
		//若distance==INT_MAX，则没有通路
		else cout << "STOP" << endl;
     return 0;
}
```
## E题：最小生成树
#### 题目描述
最小生成树问题是实际生产生活中十分重要的一类问题。假设需要在n个城市之间建立通信联络网，则连通n个城市只需要n-1条线路。这时，自然需要考虑这样一个问题，即如何在最节省经费的前提下建立这个通信网。
可以用连通网来表示n个城市以及n个城市之间可能设置的通信线路，其中网的顶点表示城市，边表示两个城市之间的线路，赋于边的权值表示相应的代价。对于n个顶点的连通网可以建立许多不同的生成树，每一棵生成树都可以是一个通信网。现在，需要选择一棵生成树，使总的耗费最小。这个问题就是构造连通网的最小代价生成树，简称最小生成树。一棵生成树的代价就是树上各边的代价之和。
而在常用的最小生成树构造算法中，普里姆（Prim）算法是一种非常常用的算法。
在本题中，读入一个无向图的邻接矩阵（即数组表示），建立无向图并按照以上描述中的算法建立最小生成树，并输出最小生成树的代价。
#### 输入
输入的第一行包含一个正整数n，表示图中共有n个顶点。其中n不超过50。
以后的n行中每行有n个用空格隔开的整数，对于第i行的第j个整数，如果不为0，则表示第i个顶点和第j个顶点有直接连接且代价为相应的值，0表示没有直接连接。当i和j相等的时候，保证对应的整数为0。
输入保证邻接矩阵为对称矩阵，即输入的图一定是无向图，且保证图中只有一个连通分量。
#### 输出
只有一个整数，即最小生成树的总代价。请注意行尾输出换行。
#### 样例输入
```
4
0 2 4 0
2 0 3 5
4 3 0 1
0 5 1 0
```
#### 样例输出
```
6
```

#### 提示
在本题中，需要掌握图的深度优先遍历的方法，并需要掌握无向图的连通性问题的本质。通过求出无向图的连通分量和对应的生成树，应该能够对图的连通性建立更加直观和清晰的概念。

#### 问题分析
完成本题首先要掌握要掌握两种计算最小生成树的方法：普利姆（Prim）算法、克鲁斯卡尔（Kruskal）算法。（当然本题只要求使用普利姆算法）
+ 普利姆算法（+点）
1. 选择任意一个顶点作为起始点，将其加入最小生成树中
2. 从未选择的顶点中选择与现有生成树连线权重最小的顶点，将其加入到现有生成树中
3. 重复上述步骤，直到最小生成树包含了图中的所有顶点。

+ 克鲁斯卡尔算法（+边）
 1. 从不属于最小生成树的边中找到权值最小的边，判断最小边及其连接的两个顶点加入到最小生成树是否会形成环路。
 2. 若不形成环路，则将此最小边及其连接的顶点并入最小生成树。
 3. 若形成环路，则永远不再看此边，然后从剩下的且不属于最小生成树的边中，寻找权值最小的边。
 4. 重复上述步骤，直至所有顶点均连接在一起，并没有形成环路时，最小生成树就找到了。
 
根据题意这里使用普利姆算法进行编程求解，用```key```记录当前生成树与各个顶点的最小值，```visited```记录当前顶点是否被访问，外循环```n```次，每次将一个顶点加入最小生成树中。
#### 完整代码
```cpp
#include <iostream>
#include <vector>
#include <climits>//用于INT_MAX的数组初始化
using namespace std;
int main() {
	int n;
	cin >> n;
	vector<vector<int>> matrix(n, vector<int>(n));
	for (int i = 0;i < n;i++) {
		for (int j = 0;j < n;j++) {
			cin >> matrix[i][j];//读入邻接矩阵
		}
	}
	vector <int> key(n,INT_MAX);
	//记录当前生成树与各顶点的最小值并不断更新
	vector <bool> visited(n, false);
	//记录当前顶点是否已经被访问过
	key[0] = 0;
	int total = 0;//记录最小生成树的总权值
	for (int i = 0;i < n;i++) {
		int u = -1;
		for (int j = 0;j < n;j++) {
			if (!visited[j] && (u == -1 || key[j] < key[u])) {
				u = j;//找到当前与生成树的最小权值点顶点
			}
		}
		visited[u] = true;
		total += key[u];
		for (int v = 0;v < n;v++) {
			if (!visited[v] && matrix[u][v]!=0 && matrix[u][v] < key[v]) {
				key[v] = matrix[u][v];
				//对key生成树到顶点的最小值进行更新
			}
		}

	}
	cout << total << endl;
	return 0;
}
```
#### 注释
```<climits>```头文件，定义了与整数类型的大小和范围相关的宏常量，常用```INT_MAX```和```INT_MIN```对数组变量进行初始化。

封面来源: [Shortest Path Algorithms Explained (Dijkstra's & Bellman-Ford)](https://www.youtube.com/watch?v=j0OUwduDOS0)
