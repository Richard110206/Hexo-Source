---
title: Debugging and Profiling
date: 2026-04-01 22:10:12
tags:
category: Tutorial
category_bar: true
archive: true
---
> A golden rule in programming is that code does not do what you expect it to do, but what you tell it to do. 

当我们遇到问题进行调试时，我们通常会采用两种方法:

- 在适当的地方添加打印语句
- 使用程序中的日志记录
  - 能定向到其他输出位置
  - 能设置不同的严重程度级别（如INFO、DEBUG、WARN、ERROR）并允许你根据这些级别过滤输出
  - 支持对日志的数据进行结构化记录，方便日后轻松的提取这些数据

调试器（debugger）是允许你在程序执行时与之相互的程序：

- 在到达某一行时停止执行
- 逐条执行单个指令
- 检查变量的值
- 满足条件时停止执行

如果我们修改了源程序，我们必须退出RR/LLDB/GDB，重新编译一次程序在进行调试，因为原窗口运行的是原二进制文件，无法直接用于新版本的程序

```bash
vim debug.cpp
g++ -g -o account_debug debug.cpp
// 编译源文件，生成可执行文件（命名为account_debug）并增加调试信息（-g）
```

- `-g`：**核心调试参数**，向编译后的可执行文件中嵌入调试信息（如行号、变量名、函数名），没有这个参数，lldb/gdb 无法定位代码位置；

- `-o account_debug`：指定编译后生成的可执行文件名称为 `account_debug`（如果省略 `-o`，默认生成 `a.out`）；

- `debug.cpp`：待编译的源文件名称（你的代码文件）。

```bash
lldb ./account_debug
```

由于我这里使用的是macOS系统，因此用其系统自带的调试器代替 GDB（大部分命令相差不大）

- `run`（简写`r`）：从`main`函数开始重新启动程序
  - 没有断点：完整运行到底
  - 有断点：运行到断点停止

```bash
(lldb) target create "./account_debug"
Current executable set to '/Users/richard/test/account_debug' (arm64).
(lldb) r
Process 8236 launched: '/Users/richard/test/account_debug' (arm64)
ID: 1, Name: Alice, Balance: 1000.5
ID: 2, Name: Bob, Balance: 500.2
Process 8236 exited with status = 0 (0x00000000) 
```

- `breakpoint set --file account.cpp --line 25`（简写 `b account.cpp:25`）
- `continue`（简写`c`）：从暂停处 “一直运行”，直到遇到下一个暂停条件（**断点** / **程序结束** / **崩溃**）

```bash
(lldb) b debug.cpp:25
Breakpoint 1: where = account_debug`main + 444 at debug.cpp:27:5, address = 0x00000001000006a4
(lldb) r
Process 42021 launched: '/Users/richard/test/account_debug' (arm64)
ID: 1, Name: Alice, Balance: 1000.5
ID: 2, Name: Bob, Balance: 500.2
Process 42021 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = breakpoint 1.1
    frame #0: 0x00000001000006a4 account_debug`main at debug.cpp:27:5
Target 0: (account_debug) stopped.
(lldb) p accountList.size()
(std::vector<Account>::size_type) 2
(lldb) c
Process 42021 resuming
Process 42021 exited with status = 0 (0x00000000) 
```

- `frame variable`（简写`fr v`）：打印当前栈帧中**所有可见变量**的完整值

```bash
(lldb) frame variable
(std::vector<Account>) accountList = size=2 {
  [0] = (accountId = 1, userName = "Alice", accountBalance = 1000.5)
  [1] = (accountId = 2, userName = "Bob", accountBalance = 500.19999999999999)
}
```

- `breakpoint list`（简写`br l`）：查看所有已设置的断点

```bash
(lldb) br l
Current breakpoints:
1: file = 'debug.cpp', line = 15, exact_match = 0, locations = 1
  1.1: where = account_debug`main + 24 at debug.cpp:16:26, address = account_debug[0x0000000100000500], unresolved, hit count = 0 

2: file = 'debug.cpp', line = 25, exact_match = 0, locations = 1
  2.1: where = account_debug`main + 444 at debug.cpp:27:5, address = account_debug[0x00000001000006a4], unresolved, hit count = 0 
```

- `breakpoint delete 1`（简写`br del 1`）：移除编号为1的断点

```bash
(lldb) br del 1
1 breakpoints deleted; 0 breakpoint locations disabled.
```
https://zhuanlan.zhihu.com/p/368904941
https://zhuanlan.zhihu.com/p/662950903


