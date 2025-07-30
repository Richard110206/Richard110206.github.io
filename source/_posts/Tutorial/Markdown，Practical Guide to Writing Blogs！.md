---
title: Practical Guide to Writing Blogs！
date: 2025-05-29 22:32:19
tags: [markdown, blogger]  # 建议至少添加1-2个标签
index_img:  /medias/Markdown，Practical Guide to Writing Blogs！.png
category: Tutorial
category_bar: true
---

## 个人博客搭建指南：
**介绍markdown从入门到进阶的语法进行博客撰写**

<!-- more -->

&emsp;&emsp;正所谓“工欲善其事必先利其器”，博主之前在csdn上写过一些简单，了解也使用过一些简单的markdown语法，但是最近新部署了github博客网站，发现别人的博客网站颜色华丽，排版精美，而我的博客却只有简单的标题、正文、斜体、加粗等简单的界面，煞是寡淡，于是上网学习，发现在markdown中可以添加HTML标签和CSS样式，使得界面变得更加丰富，更加美观，下面是一些markdown的基础语法和进阶玩法，希望对大家有所帮助。

### markdown语法

&emsp;&emsp;markdown语法具有简单易学的特性，但这也带来了一定的局限性，或许对于初学者来说是个不错的选择，但对于一些需要更加丰富的界面和功能、有更高追求的用户来说，可能需要使用HTML标签和CSS样式来实现更加复杂的界面和功能。

### HTML标签
&emsp;&emsp;HTML标签是用来描述网页内容的标签，通过HTML标签可以实现更加丰富的界面和功能，例如添加图片、视频、表格、链接等。

### CSS样式
&emsp;&emsp;CSS样式是用来描述网页样式的样式，通过CSS样式可以实现更加丰富的界面和功能，例如添加背景色、字体颜色、边框、圆角等。

### markdown语法和HTML标签
&emsp;&emsp;Markdown 自带的标记和 HTML 标签之间具有一定的互换性。事实上，Github 在展示 Markdown 格式的文件的时候，就是将 Markdown 的标记替换为 HTML 标签，再通过一定的方式渲染得到最终效果的。
&emsp;&emsp;尽管 Markdown 脱胎于 HTML，但是它们之间依然存在显著的区别。HTML 侧重于渲染的效果，具有复杂且多样的标签和繁复的框架结构，而 Markdown 则更加关注文本内容，标记少而简单，内容以文本为主。显然，HTML 学习和编辑的难度更大，但是能获得更统一，更加标准化的渲染效果，适合表达复杂的多媒体内容；Markdown 学习和编辑的难度更小，源文件更简洁直观，但是能实现的功能也更加单一，难以处理复杂的层次结构。因此 Markdown 更加适合比较简单的富文本内容。


 {% note primary%}**前言**： 不知大家是否和博主有着相同的疑惑，为什么别的博主写出来的博客排版工整、优雅美观，而自己的博客却毫无出彩之处。原先博主更多的关注点在技术博客的内容上，markdown语法仅仅靠csdn上的“语法说明”自己瞎琢磨的，现在放暑假正好有时间，再温习一下基础语法和学习一下进阶语法，来丰富自己的博客排版！这篇是我的学习笔记，博采众长，希望也能帮到大家！{% endnote %}


&emsp;&emsp;下面分为两个板块进行阐述：**基础语法**和**进阶语法**，板块一可以帮助你简单入门，版块二则是语法的升级！下面我都在代码块中展示markdown的写法，下面是对应的博客展现形式，大家可以根据自己的需求进行选择和学习！
## 一、基础语法
#### 1.标题
输入```#```+```space（空格）```就是不同等级的标题（最多只有六个等级）
```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```
># 一级标题
>## 二级标题
>### 三级标题
>#### 四级标题
>##### 五级标题
>###### 六级标题
#### 2.文本样式

**斜体**：```*```+```文本内容```+```*```
```mardown
*这是一个斜体*
```
>*这是一个斜体*

**加粗**：```**```+```文本内容```+```**```
```mardown
**这里进行了加粗**
```
>**这里进行了加粗**

**斜体加粗**：```***```+```文本内容```+```***```
```markdown
***这里是斜体加粗***
```
>***这里是斜体加粗***

**删除线**：```~~```+```文本内容```+```~~```
```markdown
~~这里是删除线~~
```
>~~这里是删除线~~

**分割线**：在一行中用三个以上的星号建立一个分隔线，行内不能有其他内容，也可以在星号中间插入空格。
```markdown
* * *
```
>* * *
#### 3.列表
**无序列表**：```-```或```*```或```+```+```space（空格）```+```文本内容```
```markdown
- 无序列表1
- 无序列表2
- 无序列表3
```
- 无序列表1
- 无序列表2
- 无序列表3

**有序列表**：```数字序号```+```.```+```space（空格）```+```文本内容```
```markdown
1. 有序列表1
2. 有序列表2
3. 有序列表3
```
>1. 有序列表1
>2. 有序列表2
>3. 有序列表3

**嵌套列表**：
```-```+```space```+```第一级无序列表1```
```space*2```+```-```+```第二级无序列表2```
```markdown
- 嵌套列表1
  - 嵌套列表2
    - 嵌套列表3
```
>- 嵌套列表1
>   - 嵌套列表2
>     - 嵌套列表3

#### 4.板块
**表格**：
```|```+ ```每列的标题```+```|```+```每列的标题```+```|```
```|```+ ```----| ----```+```|```
```|```+ ```每列内容```+```|```+```每列内容```+```|```
```|```+ ```每列内容```+```|```+```每列内容```+```|```
```markdown
|语言类型 |输出函数|
| --- | ----------- |
|c语言|printf|
|c++|cout|
|python|print|
```
|语言类型 |输出函数|
| --- | ----------- |
|c语言|printf|
|c++|cout|
|python|print|

可以通过在标题行中的连字符的左侧，右侧或两侧添加冒号（:），将列中的文本对齐到左侧，右侧或中心。
>:— 设置内容和标题栏居左对齐。
>:----: 设置内容和标题栏居中对齐。
>—: 设置内容和标题栏居右对齐。

**代码块**：前后三个```即可，可自定义选择编程语言
```python
print("这是一个代码块")
print("这是使用的python语言")
```
```cpp
#include <iostream>
using namespace std;
int main(){
cout<<"这是一个c++的代码块！"<<endl;
return 0;}
```
**链接**：```[```+```超链接显示名```+```]```+```(```+```超链接地址```+```)```
```markdown
不在了情绪的博客主页：[不在了情绪](https://blog.csdn.net/2401_86849688?type=blog)
```
>不在了情绪的博客主页：[不在了情绪](https://blog.csdn.net/2401_86849688?type=blog)

我们还可以给这个链接添加title，当鼠标悬停在链接上会出现提示。
```[```+```超链接显示名```+```]```+```(```+```超链接地址```+```space```+```"链接title"```+```)```
```markdown
不在了情绪的博客主页：[不在了情绪](https://blog.csdn.net/2401_86849688?type=blog "欢迎来到我的博客！")
```
> 不在了情绪的博客主页：[不在了情绪](https://blog.csdn.net/2401_86849688?type=blog "欢迎来到我的博客！")

使用尖括号可以很方便地把URL或者email地址变成可点击的链接：
```markdown
<https://blog.csdn.net/2401_86849688?type=blog>
```
><https://blog.csdn.net/2401_86849688?type=blog>

要显示原本用于格式化 Markdown 文档的字符，需要在字符前面添加反斜杠字符 \ 
```markdown
\*我想要输出的是两个星号*
```
>\*我想要输出的是两个星号*

## 二、进阶用法
#### 1.字体与背景
```<font face="字体">```+```文本内容```+```</font>```
```markdown
<font face="黑体">这是黑体</font>
<font face="微软雅黑">这是微软雅黑</font>
<font face="STCAIYUN">这是华文彩云</font>
```
><font face="黑体">这是黑体</font>
 <font face="微软雅黑">这是微软雅黑</font>
 <font face="STCAIYUN">这是华文彩云</font>

```<font size=字体大小>```+```文本内容```+```</font>```
```markdown
<font size=4>字体大小为4的文字</font>
<font size=3>字体大小为3的文字</font>
<font size=2>字体大小为2的文字</font>
<font size=1>字体大小为1的文字</font>
```
><font size=4>字体大小为4的文字</font>
<font size=3>字体大小为3的文字</font>
<font size=2>字体大小为2的文字</font>
<font size=1>字体大小为1的文字</font>

```<font color=“color”>```+```文本内容```+```</font>```
```markdown
<font color="red">红色的文本内容</font>
<font color="green">绿色的文本内容</font>
<font color="blue">蓝色的文本内容</font>
```
><font color="red">红色的文本内容</font>
<font color="green">绿色的文本内容</font>
<font color="blue">蓝色的文本内容</font>

```<mark>```+```文本内容```+```</mark>```或者```==```+```文本内容```+```==```
```markdown
<mark>高亮显示的文本内容</mark>
==高亮显示的文本内容==
```
><mark>高亮显示的文本内容</mark>
==高亮显示的文本内容==

```<table><tr><td bgcolor=“color”><mark>```+```文本内容```+```</mark></td></tr></table>```
```markdown
<table><tr><td bgcolor="green"><mark>有背景颜色的高亮文本内容</mark></td></tr></table>
<table><tr><td bgcolor="red"><mark>有背景颜色的高亮文本内容</mark></td></tr></table>
```
><table><tr><td bgcolor="green"><mark>有背景颜色的高亮文本内容</mark></td></tr></table>
><table><tr><td bgcolor="red"><mark>有背景颜色的高亮文本内容</td></tr></table>

```<u>```+```文本内容```+```</u>```
```markdown
<u>下划线的文本内容</u>
```
><u>下划线的文本内容</u>

```>```+```文本内容```
```markdown
>引用的文本内容
```
>引用的文本内容
#### 2.段落缩进
**首行缩进**：
- 全角：```&emsp;```或```&#8195;```
- 半角：```&ensp;```或```&#8194;```
- 半角之半角：```&nbsp;```或```&#160;```
```markdown
&emsp;&emsp;磨刀不误砍柴工，好好学习、反复练习markdown语法，才能在日后的博客撰写中手到擒来，手拿把掐！
```
>&emsp;&emsp;磨刀不误砍柴工，好好学习、反复练习markdown语法，才能在日后的博客撰写中手到擒来，手拿把掐！
#### 3.公式
&emsp;&emsp;在markdown中可以使用```$$```来作为公式块，在其中进行Latex类型公式的输入！

```$``` + ```公式``` + ```$```

```markdown
$x^2-y^2=(x+y)(x-y)$
$$(x-y)^2=x^2-2xy+y^2$$
```

>$x^2-y^2=(x+y)(x-y)$

>$$(x-y)^2=x^2-2xy+y^2$$
#### 4.表情
我们可以通过键入```emoji shortcodes```来输出表情包：
```man```:
| 😄 `:smile:` | 😊 `:blush:` | 😍 `:heart_eyes:` |
|--------------|--------------|------------------|
| 😘 `:kissing_heart:` | 😳 `:flushed:` | 😁 `:grin:` |
| 😉 `:wink:` | 😜 `:tongue_wink:` | 😀 `:grinning:` |
| 😴 `:sleeping:` | 😟 `:worried:` | 😮 `:open_mouth:` |
| 😕 `:confused:` | 😑 `:expressionless:` | 😅 `:sweat_smile:` |
| 😥 `:sad_relieved:` | 😢 `:cry:` | 😭 `:sob:` |
| 😂 `:joy:` | 😱 `:scream:` | 😠 `:angry:` |
| 😡 `:rage:` | 😷 `:mask:` | 😎 `:sunglasses:` |
| 😇 `:innocent:` | ❤️ `:heart:` | 💔 `:broken_heart:` |
| ✨ `:sparkles:` | 👍 `:thumbsup:` | 👎 `:thumbsdown:` |
| 👌 `:ok_hand:` | ✊ `:fist:` | ✌️ `:v:` |
| 👋 `:wave:` | 🙌 `:raised_hands:` | 🙏 `:pray:` |
| 👏 `:clap:` | 💪 `:muscle:` | 🏃 `:running:` |
| 👫 `:couple:` | 👪 `:family:` | 💃 `:dancer:` |
| 🙅 `:no_good:` | 💁 `:info_desk:` | 👶 `:baby:` |
| 👩 `:woman:` | 👨 `:man:` | 👵 `:grandma:` |
| 👴 `:grandpa:` | 👮 `:police:` | 😺 `:smile_cat:` |
| 🙈 `:see_no_evil:` | 💀 `:skull:` | 💋 `:kiss:` |
| 👀 `:eyes:` | 👄 `:mouth:` | 💬 `:speech_bubble:` |

```nature```:
| ☀️ `:sunny:` | ☔ `:umbrella:` | ☁️ `:cloud:` |
|--------------|----------------|-------------|
| ❄️ `:snowflake:` | ⛄ `:snowman:` | ⚡ `:zap:` |
| 🌊 `:ocean:` | 🐱 `:cat:` | 🐶 `:dog:` |
| 🐭 `:mouse:` | 🐰 `:rabbit:` | 🐯 `:tiger:` |
| 🐨 `:koala:` | 🐻 `:bear:` | 🐷 `:pig:` |
| 🐮 `:cow:` | 🐵 `:monkey:` | 🐴 `:horse:` |
| 🐘 `:elephant:` | 🐼 `:panda:` | 🐍 `:snake:` |
| 🐦 `:bird:` | 🐤 `:chick:` | 🐧 `:penguin:` |
| 🐢 `:turtle:` | 🐝 `:bee:` | 🐙 `:octopus:` |
| 🐠 `:fish:` | 🐳 `:whale:` | 🐬 `:dolphin:` |
| 🌸 `:cherry_blossom:` | 🌹 `:rose:` | 🌻 `:sunflower:` |
| 🍁 `:maple_leaf:` | 🍄 `:mushroom:` | 🌵 `:cactus:` |
| 🌴 `:palm_tree:` | 🌲 `:tree:` | 🌞 `:sun_with_face:` |
| 🌙 `:moon:` | 🌎 `:earth:` | 🌋 `:volcano:` |

```objects```:
| 🎍 `:bamboo:` | 💝 `:gift_heart:` | 🎒 `:school_satchel:` |
|--------------|--------------------------|--------------|
| 🎓 `:mortar_board:` | 🎏 `:flags:` | 🎆 `:fireworks:` |
| 🎇 `:sparkler:` | 🎃 `:jack_o_lantern:` | 👻 `:ghost:` |
| 🎅 `:santa:` | 🎄 `:christmas_tree:` | 🎁 `:gift:` |
| 🔔 `:bell:` | 🎉 `:tada:` | 🎊 `:confetti_ball:` |
| 🎈 `:balloon:` | 📷 `:camera:` | 🎥 `:movie_camera:` |
| 💻 `:computer:` | 📺 `:tv:` | 📱 `:iphone:` |
| ☎️ `:phone:` | 📞 `:telephone_receiver:` | 💡 `:bulb:` |
| 🔋 `:battery:` | 📧 `:email:` | ✉️ `:envelope:` |
| 🛀 `:bath:` | 🚿 `:shower:` | 🚽 `:toilet:` |
| 🔧 `:wrench:` | 🔨 `:hammer:` | 💰 `:moneybag:` |
| 💵 `:dollar:` | 💳 `:credit_card:` | ✂️ `:scissors:` |
| 📌 `:pushpin:` | 📎 `:paperclip:` | ✏️ `:pencil2:` |
| 📕 `:closed_book:` | 📚 `:books:` | 🔖 `:bookmark:` |
| ⚽ `:soccer:` | 🏀 `:basketball:` | 🎾 `:tennis:` |
| 🏊 `:swimmer:` | 🎮 `:video_game:` | 🎬 `:clapper:` |
| 📝 `:memo:` | 🎤 `:microphone:` | 🎧 `:headphones:` |
| 👞 `:shoe:` | 👠 `:high_heel:` | 💄 `:lipstick:` |
| 👕 `:tshirt:` | 👖 `:jeans:` | 👙 `:bikini:` |
| 👑 `:crown:` | 👓 `:eyeglasses:` | ☕ `:coffee:` |
| 🍵 `:tea:` | 🍺 `:beer:` | 🍕 `:pizza:` |
| 🍔 `:hamburger:` | 🍟 `:fries:` | 🍣 `:sushi:` |
| 🍚 `:rice:` | 🍰 `:cake:` | 🍫 `:chocolate_bar:` |
| 🍎 `:apple:` | 🍌 `:banana:` | 🍅 `:tomato:` |

```place```:
| 🏠 `:house:` | 🏡 `:house_with_garden:` | 🏫 `:school:` |
|--------------|--------------------------|--------------|
| 🏢 `:office:` | 🏣 `:post_office:` | 🏥 `:hospital:` |
| 🏦 `:bank:` | 🏪 `:convenience_store:` | 🏨 `:hotel:` |
| 💒 `:wedding:` | ⛪ `:church:` | 🌇 `:city_sunrise:` |
| 🏯 `:japanese_castle:` | 🏰 `:european_castle:` | ⛺ `:tent:` |
| 🏭 `:factory:` | 🗼 `:tokyo_tower:` | 🗻 `:mount_fuji:` |
| 🌄 `:sunrise_over_mountains:` | 🌅 `:sunrise:` | 🌈 `:rainbow:` |
| 🎡 `:ferris_wheel:` | ⛲ `:fountain:` | 🎢 `:roller_coaster:` |
| 🚢 `:ship:` | 🚤 `:speedboat:` | ⛵ `:sailboat:` |
| 🚀 `:rocket:` | ✈️ `:airplane:` | 🚁 `:helicopter:` |
| 🚂 `:train:` | 🚊 `:tram:` | 🚲 `:bike:` |
| 🚗 `:car:` | 🚕 `:taxi:` | 🚌 `:bus:` |
| 🚓 `:police_car:` | 🚑 `:ambulance:` | 🚚 `:truck:` |
| 🚦 `:traffic_light:` | ⚠️ `:warning:` | 🚧 `:construction:` |
| 🏧 `:atm:` | 🎫 `:ticket:` | ♨️ `:hotsprings:` |

```number```&& ```directions```:
| 1️⃣ `:one:` | 2️⃣ `:two:` | 3️⃣ `:three:` |
|------------|------------|-------------|
| 4️⃣ `:four:` | 5️⃣ `:five:` | 6️⃣ `:six:` |
| 7️⃣ `:seven:` | 8️⃣ `:eight:` | 9️⃣ `:nine:` |
| 🔟 `:keycap_ten:` | 0️⃣ `:zero:` | #️⃣ `:hash:` |
| ◀️ `:arrow_backward:` | ⬇️ `:arrow_down:` | ▶️ `:arrow_forward:` |
| ⬅️ `:arrow_left:` | ➡️ `:arrow_right:` | ⬆️ `:arrow_up:` |
| 🔄 `:arrows_counterclockwise:` | ℹ️ `:information_source:` | 🆗 `:ok:` |
| 🆕 `:new:` | 🆙 `:up:` | 🆒 `:cool:` |
| 🚻 `:restroom:` | 🚹 `:mens:` | 🚺 `:womens:` |
| ♿ `:wheelchair:` | 🚇 `:metro:` | 🚫 `:no_entry_sign:` |
| ⛔ `:no_entry:` | ♻️ `:recycle:` | 🕐 `:clock1:` |
| ❌ `:x:` | ❗ `:exclamation:` | ⭕ `:o:` |
| ➕ `:plus:` | ➖ `:minus:` | ✔️ `:check_mark:` |
| 🔘 `:radio_button:` | 🔗 `:link:` | ✅ `:white_check_mark:` |
| ⚫ `:black_circle:` | ⚪ `:white_circle:` | 🔴 `:red_circle:` |
| 🔵 `:blue_circle:` | ©️ `:copyright:` | ®️ `:registered:` |
| ™️ `:tm:` | 🔶 `:orange_diamond:` | 🔷 `:blue_diamond:` |

***
 恭喜！ 看到这里你已经掌握了 Markdown 的核心语法和进阶技巧！

✨ 小建议：

精致排版 + 优质内容 = 王炸组合 💥 清晰的结构和美观的格式会让你的博客更专业、更吸睛！

立刻动手写一篇吧！ 从今天开始，用 Markdown 打造你的技术分享博客，下一个顶尖技术博主就是你！ 🚀

参考文献：
[Markdown 语法详解大全(超级版)（一）——标题、字体文本式样、颜色、列表、版块区块、缩进、列表项](https://blog.csdn.net/weixin_69553582/article/details/142665344)
[Markdown 语法详解大全(超级版)（二）——图片、表格、段落、转义字符、内嵌、注释、缩进、公式](https://blog.csdn.net/weixin_69553582/article/details/142711165)
[Markdown 语法详解大全(超级版)（三）——甘特图语法详解](https://blog.csdn.net/weixin_69553582/article/details/142719257)
[Markdown 语法详解大全(超级版)（四）——Markdown 使用 Emoji 表情 （附：表情符号简码列表）](https://blog.csdn.net/weixin_69553582/article/details/140277283)
