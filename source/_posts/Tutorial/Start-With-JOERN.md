---
title: Start With JOERN
date: 2026-01-16 23:02:22
tags:
archive: true
---


### Joern的安装

一开始使用的是这个教程：[Joern安装与使用](https://blog.csdn.net/wtmdnm_/article/details/134492512?ops_request_misc=elastic_search_misc&request_id=629aef199295f22d9476f49483435e76&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-2-134492512-null-null.142^v102^pc_search_result_base2&utm_term=joern&spm=1018.2226.3001.4187)，但是安装Joern的时候等待时间太长，总是超时，于是使用了在[Joern发行版官网](https://github.com/joernio/joern/releases/)下载压缩包的方式进行安装！

#### 前置检查

Joern 是基于 JVM（Java 虚拟机）运行的工具，对 Java 版本有明确要求（官方要求 JDK 11+，推荐 JDK 11/17），如果 Java 版本不达标或未安装，执行`./joern`时会直接报错。打开WSL终端执行以下命令检查：
```bash
java -version
```
- 若输出`openjdk version 11.x.x/17.x.x`等符合要求的版本，可继续；
- 若提示`command not found`，先执行以下命令安装 JDK 11：

```bash
sudo apt update && sudo apt install openjdk-11-jdk -y
```
#### 正式安装
先进入[Joern发行版官网](https://github.com/joernio/joern/releases/)，下载`joern-cli.zip`，
```bash
# 1. 创建joern目录（已存在则无影响）
mkdir -p ~/joern
# 2. 复制Windows下载的压缩包到WSL的joern目录（请确认压缩包文件名与下载的一致）
cp /mnt/c/Downloads/joern-cli.zip ~/joern/
# 3. 进入joern目录
cd ~/joern
# 4. 解压压缩包（文件名需与下载的joern-cli.zip完全一致）
unzip joern-cli.zip
# 5. 进入解压后的核心目录
cd ~/joern/joern-cli
# 6. 启动Joern
./joern
```

{%note info%}
- 开头的`/`：表示「绝对路径」（从根目录开始）
- 结尾的`/`：表示「这是一个目录，不是文件」
- 既无开头也无结尾的`/`：表示「相对路径」（相对于当前目录）（既可以是文件也可以是目录）
{%endnote%}

若成功启动，会进入 Joern 的交互终端（出现`joern>` 提示符）看到这样的界面就代表安装完成了！

```bash
richard_11_02_06@LAPTOP-THUPDMQ0:~$ joern

     ██╗ ██████╗ ███████╗██████╗ ███╗   ██╗
     ██║██╔═══██╗██╔════╝██╔══██╗████╗  ██║
     ██║██║   ██║█████╗  ██████╔╝██╔██╗ ██║
██   ██║██║   ██║██╔══╝  ██╔══██╗██║╚██╗██║
╚█████╔╝╚██████╔╝███████╗██║  ██║██║ ╚████║
 ╚════╝  ╚═════╝ ╚══════╝╚═╝  ╚═╝╚═╝  ╚═══╝
Version: 4.0.464
Type `help` to begin


joern>
```

如果想在任意目录直接输入joern就能启动可以将 joern 加入系统环境变量：
```bash
运行
# 1. 编辑bash配置文件
nano ~/.bashrc

# 2. 在文件末尾添加这一行（指定joern脚本的绝对路径）
export PATH="$HOME/joern/joern-cli:$PATH"

# 3. 按Ctrl+O保存，按Ctrl+X退出nano编辑器

# 4. 使配置立即生效
source ~/.bashrc

# 5. 验证：任意目录输入joern即可启动
joern
```