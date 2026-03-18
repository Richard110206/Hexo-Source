---
title: Start With JOERN
date: 2026-01-16 23:02:22
tags:
archive: true
---


### Joern的安装

可以使用[官方文档](https://docs.joern.io/installation/)进行安装，也可以看这个教程：[Joern安装与使用](https://blog.csdn.net/wtmdnm_/article/details/134492512?ops_request_misc=elastic_search_misc&request_id=629aef199295f22d9476f49483435e76&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-2-134492512-null-null.142^v102^pc_search_result_base2&utm_term=joern&spm=1018.2226.3001.4187)，但是安装Joern的时候等待时间太长，总是超时，于是使用了在[Joern官网](https://github.com/joernio/joern/releases/)下载压缩包的方式进行安装！

#### 前置检查

Joern 是基于 JVM（Java 虚拟机）运行的工具，对 Java 版本有明确要求（推荐 JDK /17），如果 Java 版本不达标或未安装，执行`./joern`时会直接报错。打开WSL终端执行以下命令检查：
```bash
java -version
```
- 若输出`openjdk version 17.x.x`等符合要求的版本，可继续；
- 若提示`command not found`，先执行以下命令安装 JDK 17：

```bash
# 1. 添加Java 17软件源
sudo add-apt-repository ppa:openjdk-r/ppa -y
sudo apt update

# 2. 安装OpenJDK 17 JDK（编译工具）
sudo apt install openjdk-17-jdk -y

# 3. 安装OpenJDK 17 JRE（运行时环境）
sudo apt install openjdk-17-jre-headless -y

# 4. 手动创建java命令软链接（解决"command not found"）
sudo rm -f /usr/bin/java  # 删除旧链接（若有）
sudo ln -s /usr/lib/jvm/java-17-openjdk-amd64/bin/java /usr/bin/java
```
```bash
# 5. 配置环境变量
sudo nano ~/.bashrc
# 在文件末尾添加以下内容（保存：Ctrl+O→回车→Ctrl+X）
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
```

```bash
# 6. 生效环境变量并验证
source ~/.bashrc
java -version  # 应显示17.x.x
javac -version # 应显示17.x.x
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

### 快速开始

我们先提供一段可供检测的C++源代码：

```c++
#include <iostream>
#include <cstring>
#include <fstream>
#include <string>

// 全局变量
const int BUF_SIZE = 10;
char global_buf[BUF_SIZE];

// 简单类定义
class FileHandler {
private:
    std::string filename;
    std::fstream file;

public:
    // 构造函数
    FileHandler(const std::string& fname) : filename(fname) {}

    // 成员函数：打开文件（潜在空指针/权限风险）
    bool openFile(std::ios_base::openmode mode = std::ios_base::in) {
        file.open(filename.c_str(), mode);
        if (!file.is_open()) {
            std::cerr << "Failed to open file: " << filename << std::endl;
            return false;
        }
        return true;
    }

    // 成员函数：读取文件内容（潜在缓冲区风险）
    void readToBuffer(char* buf, int size) {
        if (buf == nullptr || size <= 0) {
            std::cerr << "Invalid buffer or size" << std::endl;
            return;
        }
        file.read(buf, size);
    }

    // 析构函数
    ~FileHandler() {
        if (file.is_open()) {
            file.close();
        }
    }
};

// 普通函数：字符串拷贝（典型缓冲区溢出风险）
void copyString(const char* input) {
    // 风险点：未检查input长度，可能超出global_buf容量
    strcpy(global_buf, input);
    std::cout << "Copied string: " << global_buf << std::endl;
}

// 主函数
int main(int argc, char* argv[]) {
    // 1. 简单条件判断+命令行参数处理
    if (argc < 2) {
        std::cerr << "Usage: " << argv[0] << " <input_string> [filename]" << std::endl;
        return 1;
    }

    // 2. 调用有风险的字符串拷贝函数
    copyString(argv[1]);

    // 3. 类实例化+文件操作
    if (argc >= 3) {
        FileHandler fh(argv[2]);
        if (fh.openFile()) {
            char local_buf[BUF_SIZE];
            fh.readToBuffer(local_buf, BUF_SIZE);
            std::cout << "Read from file: " << local_buf << std::endl;
        }
    }

    // 4. 空指针操作（潜在崩溃风险）
    char* null_ptr = nullptr;
    if (argc == 4) {
        // 风险点：解引用空指针
        *null_ptr = 'a';
    }

    return 0;
}
```

同样我们可以先使用`help`指令来查看系统级的一些指令：

```bash
joern> help
val res12: Helper = Welcome to the interactive help system. Below you find
a table of all available top-level commands. To get
more detailed help on a specific command, just type

`help.<command>`.

Try `help.importCode` to begin with.
┌────────────────┬────────────────────────────────────────────────┬─────────────────────────┐
│command         │description                                     │example                  │
├────────────────┼────────────────────────────────────────────────┼─────────────────────────┤
│close           │Close project by name                           │close(projectName)       │
│cpg             │CPG of the active project                       │cpg.method.l             │
│delete          │Close and remove project from disk              │delete(projectName)      │
│exit            │Exit the REPL                                   │                         │
│importCode      │Create new project from code                    │importCode("example.jar")│
│importCpg       │Create new project from existing CPG            │importCpg("cpg.bin.zip") │
│open            │Open project by name                            │open("projectName")      │
│openForInputPath│Open project for input path                     │                         │
│project         │Currently active project                        │project                  │
│run             │Run analyzer on active CPG                      │run.securityprofile      │
│save            │Write all changes to disk                       │save                     │
│switchWorkspace │Close current workspace and open a different one│                         │
│workspace       │Access to the workspace directory               │workspace                │
└────────────────┴────────────────────────────────────────────────┴─────────────────────────┘
```

使用`cpg.`+`Tab`来查看`cpg`相关 API 操作：

```bash
joern> cpg.
!=                          close                       hashCode                    ne                   ##                          closureBinding              help                        nn                   #>                          comment                     helpVerbose                 notify                #>>                         configFile                  id                          notifyAll             #|                          continue                    identifier                  parameter             #|^                         controlStructure            ids                         ret                    ->                          cpg                         ifBlock                     runtimeChecked       ==                          declaration                 imports                     switchBlock           all                         dependency                  isInstanceOf                synchronized         annotation                  doBlock                     jumpLabel                   tag                   annotationLiteral           dotCallGraph                jumpTarget                  tagNodePair           annotationParameter         dotTypeHierarchy            keyValuePair                templateDom           annotationParameterAssign   elseBlock                   literal                     throws               argument                    ensuring                    local                       toString             arithmetic                  eq                          member                      tryBlock             arrayAccess                 equals                      metaData                    typ                   arrayInitializer            expression                  method                      typeArgument         asInstanceOf                fieldAccess                 methodParameterIn           typeDecl             assignment                  fieldIdentifier             methodParameterOut          typeParameter         astNode                     file                        methodRef                   typeRef               binding                     finding                     methodRefWithName           unknown               block                       forBlock                    methodReturn                wait                 break                       formatted                   modifier                    whileBlock           call                        getClass                    moduleVariables             wrappedCpg           callRepr                    goto                        namespace                   →                     cfgNode                     graph                       namespaceBlock
```



这里我使用的是wsl2，我们可以将待检测的源代码放在windows中，在`joern`中导入代码进行检测：

```bash
joern> val cpg=importCode("/mnt/e/test-project/test_1/")
Using generator for language: NEWC: CCpgGenerator
Creating project `test_11` for code at `/mnt/e/test-project/test_1/`
=======================================================================================================
Invoking CPG generator in a separate process. Note that the new process will consume additional memory.
If you are importing a large codebase (and/or running into memory issues), please try the following:
1) exit joern
2) invoke the frontend: /home/richard_11_02_06/joern/joern-cli/c2cpg.sh -J-Xmx3970m /mnt/e/test-project/test_1/ --output /home/richard_11_02_06/joern/joern-cli/workspace/test_11/cpg.bin.zip
3) start joern, import the cpg: `importCpg("path/to/cpg")`
=======================================================================================================

moving cpg.bin.zip to cpg.bin because it is already a database file
Creating working copy of CPG to be safe
Loading base CPG from: /home/richard_11_02_06/joern/joern-cli/workspace/test_11/cpg.bin.tmp
Code successfully imported. You can now query it using `cpg`.
For an overview of all imported code, type `workspace`.
Adding default overlays to base CPG
The graph has been modified. You may want to use the `save` command to persist changes to disk.  All changes will also be saved collectively on exit
The graph has been modified. You may want to use the `save` command to persist changes to disk.  All changes will also be saved collectively on exit
val cpg: io.shiftleft.codepropertygraph.generated.Cpg = Cpg[Graph[463 nodes]]
```

事实上这里使用的都是Scala语法，我们可以将Scala代码直接放在这里运行！ 

- `cpg.method.l.size`：获取函数数量

```bash
joern> println(s"代码中函数总数：${cpg.method.l.size}")
代码中函数总数：31
```

- `cpg.method.name.l.foreach`：查看所有函数名

```bash
joern> cpg.method.name.l.foreach(name => println(s"函数名：$name"))
函数名：FileHandler
函数名：openFile
函数名：readToBuffer
函数名：~FileHandler
函数名：<global>
函数名：copyString
函数名：main
函数名：<global>
函数名：close
函数名：<operator>.shiftLeft
函数名：<operator>.addressOf
函数名：<operator>.logicalNot
函数名：<operator>.indirectFieldAccess
函数名：<operator>.lessThan
函数名：strcpy
函数名：is_open
函数名：<operator>.indirectIndexAccess
函数名：<operator>.fieldAccess
函数名：<operator>.indirection
函数名：c_str
函数名：<operator>.lessEqualsThan
函数名：read
函数名：<operator>.alloc
函数名：FileHandler
函数名：open
函数名：<operator>.logicalOr
函数名：<operator>.equals
函数名：<operator>.greaterEqualsThan
函数名：openFile
函数名：<operator>.arrayInitializer
函数名：<operator>.assignment
```

- `cpg.file.l`：查询并列出代码属性图（CPG）中所有 File 类型节点

```bash
joern> cpg.file.l
val res7: List[io.shiftleft.codepropertygraph.generated.nodes.File] = List(
  File(
    code = "<empty>",
    columnNumber = None,
    content = "<empty>",
    hash = None,
    lineNumber = None,
    name = "test_1/test_1.cpp",
    offset = None,
    offsetEnd = None,
    order = 0
  ),
  File(
    code = "<empty>",
    columnNumber = None,
    content = "<empty>",
    hash = None,
    lineNumber = None,
    name = "<includes>",
    offset = None,
    offsetEnd = None,
    order = 1
  ),
  File(
    code = "<empty>",
    columnNumber = None,
    content = "<empty>",
    hash = None,
    lineNumber = None,
    name = "<unknown>",
    offset = None,
    offsetEnd = None,
    order = 0
  )
)
```

