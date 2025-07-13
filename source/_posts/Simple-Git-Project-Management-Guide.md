---
title: Simple Git Project Management Guide
date: 2025-07-13 11:16:13
tags: [Git]
index_img:  /medias/Simple-Git-Project-Management-Guide.png
---

本文简单介绍了使用Git进行项目管理的操作。

 <!-- more -->

&emsp;&emsp;Git 是一个分布式版本控制系统，它允许多个人在同一项目上工作。通过 Git，我们可以记录项目的每一次更改，并且可以将代码推送到 GitHub，方便以后查看和分享（便于多人的协作开发）。以下是Git进行代码版本管理的基本操作：
### 1、在本地初始化git仓库
打开命令行工具（Windows 用户可以使用 PowerShell，Mac 和 Linux 用户可以使用终端），确保你进入了项目文件夹（命令：`cd path`），然后输入以下命令来初始化 Git 仓库：
```bash
git init
```
 **解释：**`git init`会在当前文件夹中创建一个新的 Git 仓库。现在，你的项目文件夹已经开始被 Git 追踪，之后的每一次文件修改都可以通过 Git 来记录。
 
### 2、添加文件到暂存区
暂存区是一个准备提交的区域，只有在将文件添加到暂存区后，才能将它们提交到本地仓库中。
```bash
git add
```
**解释**:`git add.`会将当前文件夹下的所有文件添加到暂存区。这意味着 Git 现在知道这些文件已经被修改，并准备将它们保存到版本历史中。
### 3、提交文件到本地仓库
暂存区中的文件提交到本地仓库。输入以下命令：
```bash
git commit -m "feat: "
```
**解释：**`git commit -m` 命令用于提交更改到本地仓库，并且 -m 参数后面跟着的是你为此次提交添加的描述信息，也就是提交信息（commit message）。提交信息是对此次提交所做更改的简短描述，它帮助团队成员或其他查看项目历史的人理解每次提交的目的或内容。

这些是特定的标签或标识符来说明提交的类型：
 - `feat`: 表示新功能的增加（feature）
 - `fix`: 表示修复了一个bug
 - `docs`: 表示文档更新
 - `refactor`: 表示代码重构（既不是修复bug也不是添加新功能）
 - `style`: 改进代码格式但不影响逻辑
 - `test`: 添加或修正测试用例
### 4. 创建远程仓库
前往 GitHub，创建一个新的远程仓库，并命名，注意：不要勾选“初始化仓库”，因为我们已经在本地初始化了仓库。
### 5. 关联本地仓库
在命令行中，输入以下命令来将本地仓库和 GitHub 的远程仓库关联起来：
```bash
git remote set-url origin https://令牌@github.com/用户名/远程仓库名`
```
### 6、推送代码到 GitHub
最后一步，我们将本地仓库中的更改推送到 GitHub 上的远程仓库：
```bash
git push -u origin main
```
**解释：**`git push` 命令会将本地仓库中的代码上传到 GitHub 的远程仓库。`-u` 参数会将本地的 main 分支与远程仓库的 main 分支关联起来，确保以后每次推送都不需要重复指定分支。

## 总结
&emsp;&emsp;至此，你学会了如何通过 Git 管理项目的版本历史，最终将项目代码推送到 GitHub。整个过程包括了从初始化仓库、添加文件、提交文件到推送到远程仓库的完整步骤。通过这些操作，你便已经具备了基本的版本控制能力。
封面来源：[Git Explained in 100 Seconds](https://www.youtube.com/watch?v=hwP7WQkmECE)

