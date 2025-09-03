---
title: Simple Git Project Management Guide
date: 2025-07-13 11:16:13
tags: [Git]
index_img: https://raw.githubusercontent.com/Richard110206/blog-image/main/cover/Simple-Git-Project-Management-Guide.png
category: Tutorial
category_bar: true
description: A simple guide to manage your Git project.
---



`Git` 是一个**分布式**版本控制系统(`Vision Control Systems`)，它允许多个人在同一项目上工作。通过` Git`，我们可以记录项目的每一次更改，并且可以将代码推送到 `GitHub`，方便以后查看和分享（多人的协作开发）。以下是`Git`进行代码版本管理的基本操作：

## 基本操作
### 1、在本地初始化git仓库
打开命令行工具（`Windows` 用户可以使用 `PowerShell`，`Mac` 和 `Linux` 用户可以使用终端`terminal`），确保你进入了项目文件夹（命令：`cd path`），然后输入以下命令来初始化 `Git` 仓库：
```bash
git init
```

 **解释：**`git init`会在当前文件夹中创建一个新的**Git仓库**（`.git`目录），这个目录里存储着管理当前目录内容所需要的仓库数据。现在，你的项目文件夹已经开始被 Git 追踪，之后的每一次文件修改都可以通过 Git 来记录。
 
 - 使用`git status`命令可以查看当前**仓库的状态**，包括哪些文件被修改了，哪些文件被添加到暂存区了，哪些文件还没有被跟踪等。

```bash
$ git status
# on branch master
#
# Initial commit
#
nothing to commit (creat/copy files and use "git add" to track)
```

### 2、添加文件到暂存区
暂存区是一个**准备提交的区域**，只有在将文件添加到暂存区后，才能将它们提交到本地仓库中。
```bash
git add
```

**解释**:`git add.`会将当前文件夹下的所有文件添加到暂存区。这意味着 Git 现在知道这些文件已经被修改，并准备将它们保存到版本历史中。

### 3、提交文件到本地仓库
暂存区中的文件提交到本地仓库。输入以下命令：
```bash
git commit -m "feat: "
```
**解释：**`git commit -m` 命令用于提交更改到本地仓库，并且 `-m` 参数后面跟着的是你为此次提交**添加的描述信息**，也就是提交信息（`commit message`）。提交信息是对此次提交所做更改的简短描述，它帮助团队成员或其他查看项目历史的人理解每次提交的目的或内容。

- `git log`可以查看以往仓库中**提交的日志**，包括什么人在什么时候进行了提交或合并，以及操作前后有什么差别。

```bash
richard@RicharddeMacBook-Air leetcode % git log
commit 03748e1b5410364249cb10d78ae72f443682dc24 (HEAD -> main)
Author: Richard110206 <lqx3222482537@qq.com>
Date:   Sun Aug 17 23:28:22 2025 +0800

    First Commit
```

- `git diff`可以查看**当前工作目录和暂存区之间的差异**。“+”表示添加的行，"-"表示删除的行，"~"表示修改的行。

这些是特定的标签或标识符来说明提交的类型：
 - ```feat```: 表示**新功能的增加**（`feature`）
 - ```fix```: 表示**修复了一个`bug`**
 - ```docs```: 表示**文档更新**
 - ```refactor```: 表示**代码重构**（既不是修复bug也不是添加新功能）
 - ```style```: **改进代码格式但不影响逻辑**
 - ```test```: **添加或修正测试用例**

### 4. 创建远程仓库
前往 `GitHub`，创建一个**新的远程仓库**，并命名，注意：不要勾选“初始化仓库”，因为我们已经在本地初始化了仓库。

### 5. 关联本地仓库
在命令行中，输入以下命令来将本地仓库和 GitHub 的远程仓库**关联**起来：
- 若是通过`SSH`方式连接远程仓库：
```bash
$ git remote add origin git@github.com:用户名/仓库名.git
```
- 若是通过`HTTPS`方式连接远程仓库：
```bash
git remote set-url origin https://令牌@github.com/用户名/远程仓库名`
```

### 6、推送代码到 GitHub
最后一步，我们将本地仓库中的更改**推送**到 `GitHub` 上的远程仓库：
```bash
git push -u origin main
```
**解释：**`git push` 命令会将本地仓库中的代码上传到 `GitHub` 的远程仓库。`-u` 参数会将本地的 `main` 分支与远程仓库的 `main` 分支关联起来，确保以后每次推送都不需要重复指定分支。

## 从远程仓库获取
`git clone`可以**获取远程仓库的代码到本地**
- 若是通过`SSH`方式连接远程仓库：
```bash
git clone git@github.com:用户名/仓库名.git
```
- 若是通过`HTTPS`方式连接远程仓库：
```bash
git clone https://github.com/用户名/仓库名.git
```
当前本地仓库的`main`分支和`Github`仓库的`main`分支在内容上是完全相同的。

`git pull`可以从远程仓库分支**获取最新的代码到本地**，这样只要在本地进行提交再`push`给远程仓库，就可以与其他开发者同时在同一个分支中进行作业，而如果两个人同时修改了同意部分的源代码，`push`时就很容易**发生冲突**，因此建议**更频繁的进行**`push`和`pull`操作！

|操作|`git clone`|`git pull`|
|----|----|----|
|**用途**|首次下载远程仓库到本地|更新本地已有代码|
|**执行前提**|本地没有仓库时使用|	必须在已有`Git`仓库的目录内执行|
|**操作内容**|下载完整仓库、自动创建远程跟踪分支|获取远程最新变更`git fetch`<br> 自动合并到当前分支`git merge`|

![关键操作图解](https://xflops.sjtu.edu.cn/hpc-start-guide/primer/attachments/20241106195404.png)

### 快照
`VCS` 通过一系列快照跟踪文件夹及其内容的更改，其中每个快照都封装了顶级目录中文件/文件夹的完整状态，其还维护元数据，例如每个快照的创建者、与每个快照相关的消息等等。

`Git` 将某个顶级目录中文件和文件夹集合的历史记录建模为一系列快照。文件/文件夹的树模型：
```bash
<root> (tree)
|
+- foo (tree)
|  |
|  + bar.txt (blob, contents = "hello world")
|
+- baz.txt (blob, contents = "git is wonderful")
```
### 关联快照
{% note warning %}不知你是否有过疑惑。如果我和我的伙伴并行共同完成一个项目的两个特性应该如何处理？是将他的那部分完成后让我在复制到我的版本上吗？显然那样做太过麻烦！{%endnote%}

在`Git`中，历史记录是快照的**有向无环图** (`DAG`)。这意味着`Git `中的每个快照都指向一组“父级”，即它之前的快照。它是一组父级，而不是单个父级（线性历史记录中的情况），因为快照可能源自多个父级，例如，由于合并（合并）两个并行的开发分支。`Git` 将这些快照称为“提交”。可视化提交历史记录可能如下所示：
```bash
o <-- o <-- o <-- o
            ^
             \
              --- o <-- o
 ```
` o`s代表单个提交（快照），理解为提交的文件（夹）
 ```bash
 o <-- o <-- o <-- o <---- o
            ^            /
             \          v
              --- o <-- o
  ```
这里意味着并行开发项目的两个功能，并进行合并，创建了一个包含两个功能的快照，`Git`会自行合并处理两个快照，若出现冲突无法解决时，则需要人工进行调整！
- `git branch`可以将分支名列表显示，同时可以确认当前所在的分支
```bash
$ git branch
* main
```
分支`main`左上角带有星号`*`表示这是我们当前所在的分支，也就是说我们正在`main`分支下进行开发，同时没有其他分支名，表示本地仓库只存在main一个分支。（以往`Git`默认分支是`master`，但现在改为了`main`）

- `git checkout -b`创建并切换到新的分支

```bash
$ git checkout -b feature-A
```
可以创建并切换到新的分支`feature-A`

```bash
$ git branch feature-A
$ git checkout feature-A
```
实际上，连续执行以上两条命令也能收到相同的的效果。

```bash
$ git branch
* feature-A
  main
```
这时再来查看我们的分支，就会发现我们正处在`feature-A`分支下，但我们在正常开发、执行`git add`命令并进行`commit`时，代码就会提交至`feature-A`分支，我们称为“培育分支”，这样我们就能在不相互影响的情况下同时进行多个功能的开发！

- `git merge`可以**将分支合并**，如上文`feature-A`是典型的**特性分支**，能集中实现单一特性，且除此之外不进行任何作业，在日常开发中往往会创建数个特性分支，同时在保留一个可以随时发布软件的稳定分支。通常使用`main`作为**主干分支**（也就是特性分支的原点和合并的终点）。
首先切换到`main`分支：
```bash
$ git checkout main
```
然后合并分支，为了在历史记录中明确记录下本次分支合并，我们需要创建合并并提交。
```bash
$ git merge --no-ff feature-A
```

- `git log --graph`**以图表形式查看分支合并历史**
- `git reset`**回溯历史版本**，我们只要提供目标时间点的哈希值，就可以完全恢复至稿时间点的状态。
```bash
git reset --hard <hash>
```
  
{% note warning %}如果我上线测试时突然发现了一个`bug`，而我不知道他是他是何时产生的应该怎么办呢？{%endnote%}

`Git`也会提供相应的二分历史记录查找！

## 总结
至此，你学会了如何通过 `Git` 管理项目的版本历史，最终将项目代码推送到 `GitHub`。整个过程包括了从初始化仓库、添加文件、提交文件到推送到远程仓库的完整步骤。通过这些操作，你便已经具备了基本的版本控制能力。

封面来源：[Git Explained in 100 Seconds](https://www.youtube.com/watch?v=hwP7WQkmECE)

