---
title: CMake Tutorial
date: 2025-09-21 09:37:34
tags: [CMake]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/CMake-Tutorial.png?raw=true
category: Tutorial
category_bar: true
description: CMake is a cross-platform build system that simplifies compiling code across different operating systems and compilers. This post offers a concise introduction to its core concepts and basic usage.
---

## 前言
&emsp;&emsp;当我们只需要编译一个cpp文件时，在macOS系统中可以直接**使用g++进行编译**，**运行生成的可执行文件**即可。（对应c语言的是gcc）
```bash
richard@RicharddeMacBook-Air 1 % touch hello.cpp
richard@RicharddeMacBook-Air 1 % vim hello.cpp
richard@RicharddeMacBook-Air 1 % g++ hello.cpp
richard@RicharddeMacBook-Air 1 % ls    
a.out		hello.cpp
richard@RicharddeMacBook-Air 1 % ./a.out 
hello world
```

&emsp;&emsp;我们平时在编写程序时**通常只有一个源文件**，所有的代码都写在这个文件中，直接编译即可；但是在实际的项目中，通常会有**多个模块**，而每个模块又由多个源文件、头文件、第三方库组成，这时候再去一个个的编译效率就会低很多，而构建所做的就是自动化完成编译、链接、打包的过程，我们平时使用的ide都内置了构建系统。

## 一、CMake
&emsp;&emsp; CMake 全称为 Cross Paltform Make，从名字中就能看出来这是一款**跨平台**的构建工具，它是**不自带编译工具**的，是比 Makefile 更高层次的构建工具， 相当于一个 CMake 配置文件就可以自动生成 Makefile文件（或是对应操作系统的配置文件）

| 操作系统 | 常用编译器（C/C++） |
| ---- | ---- |
| Linux | `GCC（gcc/g++）`：开源且默认；</br>`Clang（clang/clang++）`：兼容性好。 |
| macOS | `Clang：Xcode` 内置；</br>`GCC`：可通过包管理器安装（非默认）。 |
| Windows | `MSVC（Microsoft Visual C++）`：VS内置；</br>`MinGW/GCC`：跨平台兼容。 |


### （一）、只有单个源文件的情况
我们在根目录添加文件`CMakeLists.txt`，内容如下：
```txt
cmake_minimum_required(VERSION 3.31)
project(untitled)

set(CMAKE_CXX_STANDARD 20)

add_executable(untitled main.cpp)
```
让我们来逐行解析一下这段文本
{%note info%}
-  `cmake_minimum_required(VERSION 3.31)`

用于指定 CMake 的**最低版本要求**

-  `project(untitled)` 

用于定义项目的名称

- `set(CMAKE_CXX_STANDARD 20)`

用于设置 **C++ 标准**

- `add_executable(untitled main.cpp)`

用于添加一个**可执行文件**，并**指定它的源文件**
{%endnote%}

&emsp;&emsp;随后我们即可通过这个 `CMakeLists.txt` 文件生成目标平台下的原生工程，如Windows下的Visual Stdio工程，macOS下的`Xcode`工程，Linux下的`Makefile`工程，这个过程在`cmake`中叫做配置（`config`）,这个过程本质上**解决了跨平台编译问题**。

***

&emsp;&emsp;我们运行`cmake`，它会读取 `CMakeLists.txt` 配置文件，根据项目结构、编译选项等信息，自动生成适用于当前系统的`Makefile`（或其他构建系统文件）
```bash
richard@RicharddeMacBook-Air project % cmake .
-- The C compiler identification is AppleClang 17.0.0.17000013
-- The CXX compiler identification is AppleClang 17.0.0.17000013
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.4s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/richard/project
```

这时我们发现文件夹下多了很多文件：
```bash
richard@RicharddeMacBook-Air project % ls
cmake_install.cmake	CMakeFiles		hello.cpp
CMakeCache.txt		CMakeLists.txt		Makefile
```

再运行`make`，它会读取 `CMake` 生成的 `Makefile`，并按照规则：
- **编译**所有源代码文件（如 .cpp）为目标文件（.o）
- 将目标文件**链接成最终的可执行程序**（如你看到的 main）
- **处理依赖关系**（比如某个文件修改后，只重新编译受影响的部分）

```bash
richard@RicharddeMacBook-Air project % make
[ 50%] Building CXX object CMakeFiles/main.dir/hello.cpp.o
[100%] Linking CXX executable main
[100%] Built target main
```
我们再查看文件夹，发现多出了一个名为 `main`的可执行程序
```bash
richard@RicharddeMacBook-Air project % ls
cmake_install.cmake	CMakeLists.txt		Makefile
CMakeCache.txt		hello.cpp
CMakeFiles		main
```
这时我们再运行这个可执行文件就成功输出了：
```bash
richard@RicharddeMacBook-Air project % ./main
hello world
```


### （二）、工程中的应用(多文件)

我们先来学习一下头文件的使用方法。

#### 1.头文件保护
用于防止头文件被重复包含导致的编译错误
```cpp
#ifndef _UTILS_H_    // 唯一标识符，通常是"_文件名_H_"格式
#define _UTILS_H_    // 定义该标识符

// 头文件内容（声明、宏等）

#endif  // _UTILS_H_  // 结束条件编译
```

#### 2. 只放声明，不放定义
头文件的核心作用是声明接口，而非实现代码，否则可能导致 "多重定义" 错误。

✅ 允许的内容（声明）：
- 函数声明（int add(int a, int b);）
- 类 / 结构体声明（class MyClass; 或完整类定义，类内可包含函数声明）
- 变量声明（需加extern，如 extern int global_var;）
- 宏定义（#define MAX 100）
- 模板声明或定义（模板必须在头文件中实现，特殊情况）

❌ 不允许的内容（定义，除非有特殊处理）：
- 普通函数定义（除非用inline修饰）写在相应源文件中
- 非extern变量定义（如 int global_var = 0; 会导致重复定义）
- 可执行代码（如独立的for循环、cout语句等）

#### CMake 应用

编写主函数：
```main.cpp
#include <iostream>
#include "test1.h"
#include "test2.h"

int main(){
std::cout<<"hello world"<<std::endl;
testfunc1();
testfunc2();
return 0;
}
```
头文件：
```test1.h
#ifndef _TEST1_H
#define _TEST1_H

#include <iostream>

void testfunc1();

#endif /*_TEST1_H*/
```
对应的实现文件：
```test1.cpp
#include <iostream>
#include "test1.h"

void testfunc1(){
std::cout<<"this is testfunc1"<<std::endl;
}
```
文件夹下文件目录：
```bash
richard@RicharddeMacBook-Air project % ls
main.cpp	test1.cpp	test1.h		test2.cpp	test2.h
```
编写`CMakeLists.txt`，将源文件都包含在里面：
```txt
cmake_minimum_required(VERSION 3.10)

project(project)

add_executable(main main.cpp test1.cpp test2.cpp)
```
其余步骤与上文相同，运行结果如下：
```bash
richard@RicharddeMacBook-Air project % ./main
hello world
this is testfunc1
this is testfunc2
```

但是如果文件很多，我们在写`CMakeLists.txt`的时候，如果将每个源文件都写上去，效率显然很低，CMake 提供一种**更简便**的方法：
```txt
cmake_minimum_required(VERSION 3.10)

project(project)

aux_source_directory(. SRC_LIST)
# 将当前目录下的所有源文件添加到SRC_LIST中
add_executable(main ${SRC_LIST})
# 将SRC_LIST中的源文件编译成可执行文件main
```