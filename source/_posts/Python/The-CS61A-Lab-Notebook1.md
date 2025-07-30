---
title: The CS61A Lab Notebook1
date: 2025-07-25 23:11:35
tags: [Python,function]
index_img: /medias/The-CS61A-Lab-Notebook.png
category: Python
category_bar: true
---

This blog will document my notes and insights as I work through this challenging yet rewarding curriculum!

<!--more-->

> 笔者在学校仅仅学习了C++，对其他语言知之甚少，然而在实际应用中，Python的应用场景之广泛令人惊叹：无论是数学建模中的数据可视化、数据分析，还是计算机视觉领域的机器学习与深度学习，Python都展现出无可替代的重要性。虽然高中时曾对Python有所涉猎，但不成体系。

> 我深知在计算机领域，自主学习能力至关重要。然而平日既要应对繁重的课业，又要准备各类竞赛，实在分身乏术。值此暑假时间充裕之际，我决定研读计算机领域的标杆课程——CS61A。希望能够有所收获！以下是我的学习笔记，大家共勉！
## What's the CS61A?
&emsp;&emsp;`CS61A`是加州大学伯克利分校（`UC Berkeley`）计算机科学专业的入门课程，全称为"计算机程序的结构与解释"(`Structure and Interpretation of Computer Programs`)。这门课程**采用`Python`作为主要教学语言**，同时涵盖`Scheme`和`SQL`。课程核心在于**培养计算思维**而非单纯编程技巧，**重点教授抽象思想、函数式编程和元语言抽象**，被誉为"真正教会学生如何思考的计算入门课"！

这里是[CS61A官方网站](https://cs61a.org/)，里面包含了相关的`videos`、`slides`和`homework`帮助你学习！
英语和我一样不很出彩的可以观看b站双语翻译版本：
[【完结】【CS61A精翻双语·英文原声】伯克利大学《计算机程序的结构与解释》(2024)](https://www.bilibili.com/video/BV1sy411z7nA?spm_id_from=333.788.videopod.sections&vd_source=54c2981c1a7a8e0433b7d23096150b7a)


## Functions
```python
def <name>(<formal parameters>):
    return <return expression>
 ```
 
函数与变量的区别：变量是**一次性赋值**的，函数在**每次调用时会重新计算**。
```python
>>> radius=10
>>> area=mul(radius,radius)*pi
>>> area
314.1592653589793
>>> radius=20
>>> area
314.1592653589793
```
这里我们发现变量`area`在第一次进行赋值后，尽管`radius`进行改变，但是其值不再发生变化，可见赋值是一次性的，不是动态的！
```python
>>> def area():
...     return mul(radius,radius)*pi
...
>>> area()
1256.6370614359173
>>> radius=10
>>> area()
314.1592653589793
>>> radius=1
>>> area()
3.141592653589793
```
于是我们考虑将`area`变成函数，发现每次调用`area()`时，`area()`都会根据表达式的值重新计算，做到了动态变化！

```python
>>> print(print(1),print(2))
1
2
None None
```
| 步骤 | 代码        | 行为     | 输出      |
|-------------|----------------|-----------------------------------|----------|
| 1    | `print(1)`     | 调用 `print(1)`，打印 `1`，返回 `None` | `1`      |
| 2    | `print(2)`     | 调用 `print(2)`，打印 `2`，返回 `None`| `2`      |
| 3    | `print(None, None)` | 调用 `print(None, None)`，打印 `None None` | `None None` |

`Python`会先计算所有参数，再执行外层函数，`print()`的**返回值永远是 `None`**，但它会先执行打印。
```python
from operator import floordiv,mod
def divide_exact(N,D):
    """Return the quotient and remainder of dividing N by D.

    >>>q,r=divide_exact(2013,10)
    >>>q
    201
    >>>r
    3
    """
    return floordiv(N,D),mod(N,D)
```
项目中函数的**形式参数用大写字母表示**，提供**文档字符串**：在函数定义`def`的第一行添加注释，**表明这个函数的作用**，并**添加一个实例说明**（可以在`python`**交互式界面**进行演示）。
```python
def fib(n):
    """Compute the nth Fibonacci number?"""

    pred,curr=1,0
    k=0
    while k < n:
        pred,curr=curr,curr+pred
        k=k+1
    return curr
```
通过`while`控制语句实现求值斐波那契数列索引的元素值！
## Higher-Order Functions


```python
"""Generalization."""

from math import pi,sqrt

def area(r,shape_constant):
    assert r>0,"A length must be positive"
    return r*r*shape_constant

def area_square(r):
    return area(r,1)

def area_circle(r):
    return area(r,pi)

def area_hexagon(r):
    return area(r,3*sqrt(3)/2)
```
1. `assert` 的防御性编程作用：**防止非法的负值或零值输入**导致数学错误！
```pythono
assert r > 0, "A length must be positive"
```
2. 将不同图形的面积计算**抽象为统一公式**：`r² × 形状系数`：

 - **避免为每个图形重复编写**` r*r `的计算逻辑

- 新增图形时**只需提供对应的形状常数**，**无需修改核心算法**，**增强代码整体的泛化能力**

- **集中维护输入验证逻辑**（如` assert `检查）

```python
"""Generalization."""

def identity(k):
    return k

def cube(k):
    return pow(k,3)

def summation(n,term):
    """Sum the first N terms of a sequence.

    >>> summation(5,cube)
    225
    """
    total,k=0,1
    while k<=n:
        total,k=total+term(k),k+1
    return total

def sum_naturals(n):
    """Sum the first N natural numbers.

    <<< sum_naturals(5)
    15
    """
    return summation(n,identity)

def sum_cubes(n):
    """ Sum the first N cubes of natural numbers.

    >>> sum_cubes(5)
    225
    """
    return summation(n,cube)
```
这里传入的参数`term`是一个已定义的函数名，我们通过参数`term`对求和类型进行修改和新定义，而**整体框架无需改动**。
```python 
"""Generalization."""

def make_adder(n):
    """Return a function that takes one argument
    K and return K+N.

    >>> add_three=make_adder(3)
    >>> add_three(4)
    7
    >>> make_adder(3)(4)
    7
    """
    def adder(k):
        return k+n
    return adder
```
这是一个返回值为函数的函数（函数的嵌套）叫作：
- 函数工厂（`Factory Pattern`）:
`make_adder` 是一个 生成函数的函数（工厂）
根据参数 n 动态生成不同功能的加法函数

以上表明函数与编程语言中的其他值一样，可以**作为参数传递**也可以**作为返回值返回**，这就是**高阶函数**(**Higher-Order Function**)。

## Environments
### lambda表达式
`def`和`lambda`表达式的区别：
| 特性               | Lambda 表达式                          | 普通函数 (`def`)                     |
|--------------------|---------------------------------------|--------------------------------------|
| **语法**           | 单行匿名表达式：`lambda x: x + 1`     | 多行命名定义：`def func(x): return x + 1` |
| **名称**           | 匿名（无函数名）                       | 有函数名（可通过 `func.__name__` 获取） |
| **代码复杂度**      | 仅限单个表达式（不能包含语句）         | 可包含多行语句、循环、条件等复杂逻辑    |
| **返回值**         | 自动返回表达式结果                     | 需显式使用 `return`                   |
| **作用域**         | 只能访问全局变量和参数                 | 可访问全局、局部变量，支持闭包          |
| **适用场景**       | 简单逻辑、临时函数                     | 复杂逻辑、需复用的功能                 |

### Function Currying(函数柯里化)
&emsp;&emsp;**柯里化**（`Currying`）是一种将接受多个参数的函数转换为一系列只接受单个参数的函数链式调用的技术，其核心特点是分步传递参数和延迟计算。

```python
def curry(f):
    def g(x):
        def h(y):
            return f(x,y)
        return h
    return g
```
&emsp;&emsp;在这个例子中我们发现柯里化进行了**参数分解**：原始函数` f(x,y) `需要同时接收两个参数，而柯里化后通过`curry(f)` 生成的新函数链 `g(x)(y)` 允许先传` x `再传 `y`。

```python
a=1
def f(g):
    a=2
    return lambda y:a*g(y)
f(lambda y:a+y)(a)
```
&emsp;&emsp;在这个例子中，先定义了`a=1`全局变量（`local frame`），在函数`f`内部定义局部变量`a=2`（`global frame`）所以结果是（`2*(1+1)`）！

## Abstraction
&emsp;&emsp;函数抽象是给某个计算过程起个名字，然后整个项目过程都引用这个名字，而不用担心具体的实现细节。

- 需要知道函数需要**传递几个参数**
- 需要知道**函数的功能**
- 不需要知道函数的**实现过程**

比如平方函数：

```python
def square(x):
    return x*x
```
```python
from operator import mul
def square(x):
    return mul(x,x-1)+x
```

```python
def square(x):
    return pow(x,2)
```
封面来源于`CS61A`中`lecture1`的`slide`，是其标志性的表达式树！
