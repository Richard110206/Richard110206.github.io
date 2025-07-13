---
title: 'Hexo Hacks: Unleash Your Blogging Magic'
date: 2025-07-12 22:15:10
tags: [Hexo, Blogging]
index_img: /medias/Hexo-Hacks-Unleash-Your-Blogging-Magic.png
---

本文简单介绍了Hexo搭建个人博客的丰富玩法和不同平台撰写博客上的适配差异。

 <!-- more -->

## 一、玩转`Hexo`
不同主题的Hexo主题博客，大致特点如下：
| 主题       | 特点                          | 
|------------|-------------------------------|
| **Fluid**  | 加载速度快，有简单的动态效果，移动端适配佳          | 
| **Butterfly** | 功能全面，社区活跃，高度可定制 |  |
| **Landscape** | 默认主题，轻量级，简洁易上手（有较多限制）              | 

博主尝试了这几种后对`fluid`追一见钟情，遂使用此主题。
### （一）更换主题
**1. 安装 `Fluid` 主题**

在 Hexo 博客根目录（`/HexoData`）运行：
```bash
npm install --save hexo-theme-fluid
```
**2. 修改 `Hexo` 主配置**

打开`_config.yml`（位于博客根目录），找到`theme`字段，修改为：

```yaml
theme: fluid
```
如需要可以将 `language` 设置为中文：

```yaml
language: zh-CN
```
**3. 创建 Fluid 的配置文件**

`Fluid`主题的配置需要额外文件：
在博客根目录下创建` _config.fluid.yml `文件：

```bash
cp node_modules/hexo-theme-fluid/_config.yml _config.fluid.yml
```
编辑` _config.fluid.yml `来自定义主题（如菜单、颜色、字体等）。

**4. 迁移 Landscape 的内容（可选）**

自定义样式/脚本：将 `Landscape` 的 `source/css` 或 `source/js` 文件复制到 `Fluid` 的 `source` 目录。
文章/页面：`_posts` 和` _pages` 内容无需迁移，`Hexo` 会自动读取。

**5. 清理并重新生成**
```bash
hexo clean && hexo g && hexo s
```
访问 <http://localhost:4000> 查看效果。
### （二）添加评论系统
[Hexo快速构建个人小站-Fulid主题下添加Valine评论系统(三)](https://blog.csdn.net/Neter_Leon/article/details/107064603?ops_request_misc=&request_id=&biz_id=102&utm_term=hexo%E7%9A%84fulid%E4%B8%8D%E5%90%8C%E7%8E%A9%E6%B3%95&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-107064603.142^v102^pc_search_result_base2&spm=1018.2226.3001.4187)
注意`comments`在(`\themes\你的主题名\_config.yml`)文件中，Hexo 主配置文件(`_config.yml`)通常用于全局配置（如博客标题、作者等），添加评论系统的配置不在这个文件中。将`appid`和`appkey`填入，同时记得根据选择的插件修改博客评论系统的`type`！
### （三）添加背景音乐
[Hexo-Fluid主题添加音乐页面](https://blog.csdn.net/weixin_43471926/article/details/109798928?ops_request_misc=%257B%2522request%255Fid%2522%253A%25224db07d60e8ad014b04fcc3b6dc2a5cdf%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=4db07d60e8ad014b04fcc3b6dc2a5cdf&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-109798928-null-null.142^v102^pc_search_result_base2&utm_term=hexo%20fluid%E5%8D%9A%E5%AE%A2%E8%87%AA%E5%8A%A8%E6%92%AD%E6%94%BE%E9%9F%B3%E4%B9%90&spm=1018.2226.3001.4187)
### （四）给每篇博客添加封面/背景
在`source`文件夹下新建`medias`文件夹用于存放图片，在每篇博客文章` front-matter `的区域添加：
**背景图片：**
```markdown
banner_img: 图片路径（/medias/图片文件名）
```
**封面图片预览：**
```markdown
index_img: 图片路径（/medias/图片文件名）
```



## 二、CSDN与Hexo撰写差异

**1. 插入图片**
插入图片的语法如下：`![插入图片的注释](插入图片的连接或本地路径)`
&emsp;&emsp;CSDN 不支持本地图片链接：如果你在 CSDN 写博客时插入本地图片路径，其他用户无法访问你的本地文件。
&emsp;&emsp;使用Hexo进行博客撰写时，图片可以放在`source/images/ 文件夹（可以自行创建）`使用相对路径，避免文件迁移后路径找不到而导致问题，建议一篇博客单独创建一个文件夹进行图片的存储便于管理。
&emsp;&emsp;CSDN中的图片注释无法显示，必须通过在图片下添加`<center>添加图片注释</font</center>`的方式进行注释。
><center>添加图片注释</font</center>

&emsp;&emsp;网上其他博主也有推荐使用[七牛云存储](https://portal.qiniu.com/signin?redirect=%2Fhome)的，将图片从本地上传到七牛云存储仓库。得到一个外链地址，将外链地址作为图片的URL地址写进文章。

**2.数学公式**
CSDN中默认`latex`的格式是可以进行渲染的，而在`Hexo`中需要在全局配置文件中进行修改：
```markdown
  # 数学公式，开启之前需要更换 Markdown 渲染器，否则复杂公式会有兼容问题，具体请见：https://hexo.fluid-dev.com/docs/guide/##latex-数学公式
  # Mathematical formula. If enable, you need to change the Markdown renderer, see: https://hexo.fluid-dev.com/docs/en/guide/#math
  math:
    # 开启后文章默认可用，自定义页面如需使用，需在 Front-matter 中指定 `math: true`
    # If you want to use math on the custom page, you need to set `math: true` in Front-matter
    enable: true

    # 开启后，只有在文章 Front-matter 里指定 `math: true` 才会在文章页启动公式转换，以便在页面不包含公式时提高加载速度
    # If true, only set `math: true` in Front-matter will enable math, to load faster when the page does not contain math
    specific: false
```

封面来源：[如何使用Hexo+Github Pages 搭建个人博客，手把手最新教程](https://www.youtube.com/watch?v=XiMVwkxu3hU)


