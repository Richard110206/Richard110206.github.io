---
title: Powerful Editor:Vim
date: 2025-07-28 16:33:00
tags: [vim]
index_img:  /medias/Powerful Editor Vim.jpg
category: Tutorial
category_bar: true
---

A brief tutorial of Editor Vim,including the Unique Philosophy and Basic Commands!
<!--more-->

{% note primary %}"Writing English words and writing code are very different activities. When programming, you spend more time switching files, reading, navigating, and editing code compared to writing a long stream."—— < The Missing Semester of Your CS Education >{%endnote%}


`Vim` 是一个**模态编辑器**（modal editor），它的设计哲学是：编辑操作应当通过**键盘组合**完成，而非依赖鼠标或菜单。

{% note primary %}"Vim avoids the use of the mouse, because it’s too slow; Vim even avoids using the arrow keys because it requires too much movement."—— < The Missing Semester of Your CS Education >{%endnote%}

`Vim`认为使用鼠标浪费时间，会降低效率，因为手从鼠标移动到键盘需要一定时间，对程序员来说反复来回的切换是很`annoying`的。因此，所有的`vim`功能都可以通过键盘操作，或许一开始你并不习惯，但等你使用久了，便能发现它得到程序员青睐的原因！:blush:

课程相关视频与讲义：[Editors (Vim)](https://missing.csail.mit.edu/2020/editors/)
## 核心特性
 
   - `普通模式`：用于导航和操作文本（默认模式）
   - `插入模式`：像常规编辑器一样输入文本（按 `i` 进入）
   - `可视模式`：选择文本块（按 `v` 进入）
   - `命令行模式`：执行保存/退出等命令（按 `:` 进入）
## 基础操作
使用 `Vim` 时会经常使用`<ESC>`键，而它不在主键盘区，显然不那么方便，于是很多程序员考虑将 `Caps Lock` 重新映射到 `Escape`或使用简单的键序列创建替代映射！
### 模式切换
| 操作 | 功能 |
|------|------|
| `vim` | 进入vim编辑器 |
| `vim 文件名` | 打开特定文件（不存在时会新建） |
| `i`(`insert`) | 进入插入模式 |
| `Esc` | 返回普通模式 |
| `:` | 进入命令行模式 |

### 光标移动
| 操作 | 功能 |
|------|------|
| `h` `j` `k` `l` | 左/下/上/右移动 |
| `0` | 移动到行首 |
| `$` | 移动到行尾 |
| `^` | 移动到行首非空字符 |
| `G` | 移动到文件底部 |
| `gg` | 移动到文件顶部 |
| `H` | 移动到窗口顶部 |
| `L` | 移动到窗口底部 |
| `Ctrl+u` | 上翻半页 |
| `Ctrl+d` | 下翻半页 |
| `Ctrl+b` | 上翻整页 | 
| `Ctrl+f` | 下翻整页 | 


## 编辑功能
### 文本操作
| 操作 | 功能 |
|------|------|
| `o` | 下方新建行并插入 |
| `O` | 上方新建行并插入 |
| `u` | 撤销 |
| `Ctrl+r` | 重做 |
| `x` | 删除字符 |
| `dw` | 删除单词 |
| `dd` | 删除整行 |
| `cc` | 删除并进入插入模式 |
### 复制粘贴
| 操作 | 功能 |
|------|------|
| `y` (`yank`)| 复制 |
| `yy` | 复制当前行 |
| `yw` | 复制单词 |
| `p`(`paste`) | 粘贴 |
### 可视化模式
| 操作 | 功能 | 说明 |
|------|------|------|
| `v` | 字符可视化 | 按字符选择 |
| `V` | 行可视化 | 按行选择 |
| `Ctrl+v` | 块可视化 | 矩形选择 |

## 高级功能

### 搜索与替换
| 操作 | 功能 |
|------|------|
| `f+字符` | 向前查找字符 |
| `F+字符` | 向后查找字符 |
| `~` | 大小写转换 |

### 批量操作
`数字+指令`可以进行批量化操作
```bash
 4j #向下移动4行 
 3ee #选择3个单词 
 7dw #删除7个单词
```

## 文件操作

### 保存与退出
| 操作 | 功能 |
|------|------|
| `:w`(`write`) | 保存文件 |
| `:q`(`quit`) | 退出 |
| `:qa`(`all`) | 退出所有窗口 |
| `:wq` | 保存并退出 |

### 实战演示
学习了上面那么多的指令不妨自己创建一个`python`文件，结合之前学习的`shell`命令，在实践中感受`Vim`的魅力吧！:smile:
```python
import sys #导入sys来接受shell中的参数

def fizz_buzz(limit):
    for i in range(1, limit + 1):
        if i % 3 != 0 and i % 5 != 0:
            print(i)
        elif i % 3 == 0 and i % 5 != 0:
            print('fizz')
        elif i % 5 == 0 and i % 3 != 0:
            print('buzz')
        else:
            print('fizzbuzz')

def main():
    fizz_buzz(int(sys.argv[1]))

if __name__=='__main__':
    main()
   ```
   ```bash
   $ python3 fizzbuzz.py 30
1
2
fizz
4
buzz
fizz
7
8
fizz
buzz
11
fizz
13
14
fizzbuzz
16
17
fizz
19
buzz
fizz
22
23
fizz
buzz
26
fizz
28
29
fizzbuzz
```
