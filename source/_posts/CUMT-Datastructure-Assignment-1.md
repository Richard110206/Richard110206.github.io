---
title: CUMT-Datastructure-Assignment 1
date: 2025-06-8 22:29:13
tags: [Linear List，stack]
index_img: /medias/CUMT-Datastructure-Assignment-1.png
---

数据结构作业1，涉及循环链表的约瑟夫问题和栈相关的问题，包含重要知识点的解析、解题思路和完整代码。

 <!-- more -->

## E题： 约瑟夫问题
#### 题目描述

编写一个程序求解约瑟夫（Joseph）问题。有n个小孩围成一圈，给他们从1开始依次编号，从编号为1的小孩开始报数，数到第m（0<m<n）个小孩出列，然后从出列的下一个小孩重新开始报数，数到第m个小孩又出列，…，如此反复直到所有的小孩全部出列为止，求整个出列序列。

#### 输入
占一行为n和m（n<100）。
#### 输出
整个出列序列。
#### 样例输入
```
6 5
```
#### 样例输出
```
5 4 6 2 3 1
```
#### 问题分析
  约瑟夫问题是一个经典的使用循环链表的问题，**解题思路**如下：  
1. 创建循环链表：通过结构体```struct```创建孩子结点，创建```head```头结点和指向当前结点可供遍历进行移动的指针```cur```，再通过```for```循环遍历，用```new```运算动态分配内存，给每个孩子结点的```num```域赋上编号的初值，最后将尾结点的指针域指向头结点，完成链表的闭合，实现循环链表的功能。
2. 模拟报数过程：通过```cur```指针，计数```m-1```个孩子后，```delete```删除下一个结点释放内存。
3. 重复过程：重复执行，直至有```cur==cur->next```时，即链表中只剩下一个孩子时终止遍历。

#### 完整代码
```cpp
#include <iostream>
using namespace std;
//定义孩子结点的结构体
	struct Child{
		int num;//孩子编号
		Child* next;//指向下一个孩子的指针
	};
	//约瑟夫问题解决主函数
	void Joseph(int n,int m) {
		Child* head = new Child;//创建第一个孩子的头结点
			head->num = 1;//头结点的编号
		   head->next = nullptr;//定义头结点的指针为空
			Child* cur = head;//当前指针指向头结点
			//构建循环链表
			for (int i = 2;i <= n;i++) {
				Child* node = new Child;//创建后续新的孩子结点
				node->num = i;//给后续孩子孩子结点编号
				cur->next = node;//指针指向新的结点
				cur = cur->next;//指针向后移动
			}//链表构建完成
			cur->next = head;//将链表首尾相接形成循环
			while (cur != cur->next) {//当剩下的孩子不止一个时
				for (int i = 1;i < m;i++) {//数m-1个孩子
					cur = cur->next;
				}
				Child* victim = cur->next;//用新的victim记录删除的节点
				cout << victim->num << " ";
				cur->next = victim->next;//跳过第m个结点，指针域进行连接
				delete victim;//释放内存
		}
			cout << cur->num << " ";
			delete cur;
	}
	int main() {//执行主函数
		int n, m;
		cin >> n >> m;
		Joseph(n, m);
		return 0;
}
```
##  F题：括号配对
#### 题目描述
设计一个算法利用顺序栈检查用户输入的表达式中括号是否配对（假设表达式中可能含有圆括号()、中括号[]和大括号{}）。
#### 输入
占一行为含有三种括号的表达式（最长100个符号）。
#### 输出
匹配时输出YES，小括号不匹配输出NO1，中括号不匹配时输出NO2，大括号不匹配时输出NO3。
#### 样例输入

```
{([a])}
```
#### 样例输出
```
YES
```
#### 问题分析
栈"后进先出"(LIFO)的特性正好匹配括号嵌套的顺序，**解题思路**如下：
1. 如果检测输入为左括号，压入栈中；如果检测输入为右括号：检测栈是否为空、如果相匹配则弹出栈，不匹配则输出错误。
#### 注意点
1. 执行有关栈的操作，如```.top()```、```.pop()```时，需要先检查栈非空，否则会导致未定义行为。输入遍历结束后也需要检查是否非空。
2. 需要考虑输入的特殊情况，如只有左、右括号，左括号多余，无输入的情况。
3. 根据左括号是否匹配进行输出NO，而不是根据右括号匹配判断。
4. 注意处理当输入字符为非括号时的情况。

#### 完整代码
```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    char a;
    stack<char> match;//用于存储左括号的栈
    bool hasInput = false;//标记是否有输入

    while (cin >> a) {
        hasInput = true;
        if (a == '(' || a == '[' || a == '{') {
            match.push(a);
        }//处理左括号，输入后压进栈内
        else if (a == ')' || a == ']' || a == '}') {
            if (match.empty()) {
                if (a == ')') cout << "NO1" << endl;
                else if (a == ']') cout << "NO2" << endl;
                else if (a == '}') cout << "NO3" << endl;
                return 0;
            }
            char top = match.top();//查看栈顶元素
            if ((a == ')' && top == '(') ||
                (a == ']' && top == '[') ||
                (a == '}' && top == '{')) {
                match.pop();//匹配则弹出栈顶
            }
            else {
                if (top == '(') cout << "NO1" << endl;
                else if (top == '[') cout << "NO2" << endl;
                else if (top == '{') cout << "NO3" << endl;
                return 0;
            }
        }
    }
    if (!hasInput) {
        cout << "YES" << endl;
        return 0;
    }
    if (match.empty()) {
        cout << "YES" << endl;
    }
    else {
        char top = match.top();
        if (top == '(') cout << "NO1" << endl;
        else if (top == '[') cout << "NO2" << endl;
        else if (top == '{') cout << "NO3" << endl;
    }
    return 0;
}
```
##  G题：后缀表达式
#### 题目描述
给出一个中缀表达式，输出该表达式的后缀表达式。
#### 输入
占一行，一个中缀表达式（运算符只有+ - * /，最多1000个字符），输出后缀表达式。
#### 输出
输出后缀表达式。
#### 样例输入

```
(56-20)/(4+2)
```
#### 样例输出
```
56 20 - 4 2 + /
```
#### 问题分析
本题也是栈的经典问题，**解题思路**如下：
1. 根据先乘除后加减的原则，数字划分优先级
2. 当读入的输入时数字时，直接将其输出，若读入的是运算符，则先检查栈内：栈空，则压入栈内；栈非空，则与栈顶元素比较：将栈中优先级不低于当前运算符的运算符弹出并输出，然后将当前运算符入栈
   
#### 注意点
1. 遇到左括号时直接入栈，遇到右括号时，不断弹出栈顶运算符并输出，直到遇到左括号（左括号弹出但不输出）
2. 表达式扫描完毕后，将栈中剩余运算符全部弹出并输出

#### 完整代码
```cpp
#include <iostream>
#include <stack>
#include <cctype>
using namespace std;
//定义运算符优先级函数
int pre(char ch) {
    if (ch == '+' || ch == '-') return 1;
    if (ch == '*' || ch == '/') return 2;
    return 0;
}
int main() {
    stack<char> cal;
    char ch;
    bool read = false;
    while (cin >> noskipws >> ch) { //不跳过空格读入
    if (ch == ' ' || ch == '\n') continue;
            if (isdigit(ch)) {//如果是数字
            cout << ch;
            read = true;//标记读入的是数字
        }
        else {
            if (read) {
                cout << " ";
                read = false;
            }
            if (ch == '(') {//左括号直接入栈
                cal.push(ch);
            }
            else if (ch == ')') {//处理读入的右括号
            //弹出栈顶元素直至栈顶为左括号
                while (!cal.empty() && cal.top() != '(') {
                    cout << cal.top() << " ";
                    cal.pop();
                }
                if (!cal.empty()) cal.pop();//弹出左括号但不输出
            }
            else if (ch == '+' || ch == '-' || ch == '*' || ch == '/') {
                while (!cal.empty() && cal.top() != '(' && pre(cal.top()) >= pre(ch)) {
                    cout << cal.top() << " ";
                    cal.pop();
                }
                cal.push(ch);
            }
        }
    }
    //弹出栈内所有运算符
    while (!cal.empty()) {
        cout << cal.top() << " ";
        cal.pop();
    }

    return 0;
}
```
#### 注释

`<cctype>` 是 C++ 标准库中的头文件，提供了一系列用于字符分类和处理的函数（通常传入的参数类型为 `char`）。

| 字符分类函数 | 作用                              |
|--------------|-----------------------------------|
| `isalpha()`  | 检查是否是字母（a-z, A-Z）        |
| `isdigit()`  | 检查是否是数字（0-9）             |
| `isalnum()`  | 检查是否是字母或数字              |
| `isspace()`  | 检查是否是空白字符（空格、制表符、换行等）|
| `islower()`  | 检查是否是小写字母                |
| `isupper()`  | 检查是否是大写字母                |

字符转换函数     | 作用
-------- | -----
`tolower()`| 将字符转换为小写
`toupper()`| 将字符转换为大写

`noskipws`是定义在 `<iomanip>`头文件中的一个 I/O 操纵符（manipulator），全称是 **"no skip whitespace"**。

```cpp
char ch;
cin >> ch;       // 自动跳过空白，读取第一个非空白字符
cin >> noskipws >> ch;  // 不跳过空白，读取下一个字符（可能是空白）
```

如果需要恢复默认的跳过空白行为（作用相当于一个开关激活函数）：

```cpp
cin >> noskipws;  // 关闭跳过空白
// ... 读取包含空格的输入 ...
cin >> skipws;    // 恢复默认行为
```
与`cin.get`相比更加灵活可控：
```cpp
cin.get(ch);  // 总是读取下一个字符（包括空白），不需要 noskipws
```

##  H题：字符串反转
#### 题目描述
小C很喜欢倒着写单词，现在给你一行小C写的文本，你能把每个单词都反转并输出它们吗？
#### 输入
输入包含多组测试样例。第一行为一个整数T，代表测试样例的数量，后面跟着T个测试样例。每个测试样例占一行，包含多个单词。一行最多有1000个字符。
#### 输出
对于每一个测试样例，你应该输出转换后的文本。
#### 样例输入
```
3
olleh !dlrow
I ekil .bulcmca
I evol .mca
```
#### 样例输出
```
hello world!
I like acmclub.
I love acm.
```
### 问题分析
本题适合用栈“先进后出”的特性来进行字符串的反转，**解题思路**如下：
1. 逐个字符处理进行读入，压入栈内
2. 当遇到空格时，将栈内的字符全部弹出
3. 循环结束后单独处理栈内剩余元素


#### 注意点
1. 是逐个单词进行逆序，不是整段文本所有字母的逆序，因此需要根据输入的空格和回车来确定分段点。

#### 完整代码
```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;
void reverseWords() {
    string line;
    getline(cin, line);//整行读入输入
    stack<char> st;//用于反转字符的栈
    for (char c : line) {//遍历每一个字符
    //line可以是字符串也可以是字符数组
        if (c == ' ') {//遇到空格，开始输出字符
            while (!st.empty()) {
                cout << st.top();
                st.pop();
            }
            cout << ' ';
        }
        else {
            st.push(c);
        }
    }
    while (!st.empty()) {//循环结束，处理最后一个没有空格的单词
        cout << st.top();
        st.pop();
    }
    cout << endl;
}

int main() {
    int n;
    cin >> n;
    cin.ignore(); //忽略换行符
    for (int i = 0; i < n; i++) {
        reverseWords();
    }
    return 0;
}
```
#### 注释

特性 | getline(cin, line) |cin.get()
-------- | --------- |------------
读取方式  | 读取整行直到'\n'|可读取单个字符或一行
存储类型|`string`|`char`或`char[]`
是否跳过空白字符  | 不会跳过（读取所有字符）|不会跳过（读取所有字符）
换行符处理 | 丢弃'\n'（不存储） | 可以读取'\n'

封面来源：[Data Structures: Crash Course Computer Science #14](https://www.youtube.com/watch?v=DuDz6B4cqVc&t=92s)
