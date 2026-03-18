---
title: QT Tutorial
date: 2026-02-10 14:43:57
tags:
category: 
- CPP
category_bar: true
archive: true
---

#### 一个简单 GUI 项目

这里手写代码完成一个简易版`win`+`R`的界面框：

```c++
#include <QApplication>
#include <QLabel>
#include <QLineEdit>
#include <QPushButton>
#include <QHBoxLayout> // 水平布局
#include <QVBoxLayout> // 垂直布局
#include <QWidget>

int main(int argc,char *argv[]){

    QApplication app(argc,argv);

    QLabel *infoLabel=new QLabel;
    QLabel *openLabel=new QLabel;
    QLineEdit *cmdLineEdit=new QLineEdit;
    QPushButton *commitButton=new QPushButton;
    QPushButton *cancelButton=new QPushButton;
    QPushButton *browseButton=new QPushButton;

    infoLabel->setText("input cmd");
    openLabel->setText("open");
    commitButton->setText("commit");
    cancelButton->setText("cancel");
    browseButton->setText("browse");

    QHBoxLayout *cmdLayout=new QHBoxLayout;
    cmdLayout->addWidget(openLabel);
    cmdLayout->addWidget(cmdLineEdit);

    QHBoxLayout *buttonLayout=new QHBoxLayout;
    buttonLayout->addWidget(commitButton);
    buttonLayout->addWidget(cancelButton);
    buttonLayout->addWidget(browseButton);

    QVBoxLayout *mainLayout=new QVBoxLayout;
    mainLayout->addWidget(infoLabel);
    mainLayout->addLayout(cmdLayout);
    mainLayout->addLayout(buttonLayout);

    QWidget window;
    window.setLayout(mainLayout);
    window.show();

    return app.exec();
}
```

#### 编译流程

 打开 Qt 安装后自带的`「Qt 6.9.2 (MinGW 64-bit)」`命令提示符，如果使用系统默认`CMD`编译需要配置环境变量较为麻烦。

1. 生成基础的`.pro`项目文件

```bash
qmake -project
```

2. 根据`.pro`文件生成 `Makefile`

- 如果修改了`.pro`（比如加 QT 模块`QT += core gui widget`），必须重新执行这一步。

```bash
qmake
```

3. 执行编译，生成可执行文件

```bash
mingw32-make -j4
```

- `mingw32-make`是 MinGW 的编译工具，会读取 `Makefile` 的规则调用 `g++` 编译；

- `-j4`是多核编译（4 核），加快编译速度，可根据自己 CPU 核心数调整（比如`-j8`）；

- 若仅修改源码（未改`.pro`），无需重新执行`qmake -project`和`qmake`，直接执行这一步即可（make 会自动检测文件修改）。

4. 切换到编译产物目录

```bash
cd release
```

5. 运行编译好的可执行文件

```bash
qmake.exe
```

## QtCreator

### 信号与槽

可以将UI控件与事件（函数）相关联起来，在“设计”界面右击，选项中选择“转到槽”：

```c++
void Widget::on_commitButton_clicked()
{
    // 获取lineEdit数据
    QString program=ui->cmdLineEdit->text();
    // 创建process对象
    QProcess *myProcess=new QProcess(this);
    myProcess->start(program);
}
```

以上`on_控件名_事件名` 是 Qt 的 **“自动关联（Auto-Connection）”** 机制，但是在实际开发中，为了代码可读性更高、命名更自由，我们通常会**手动绑定信号和槽**：

```c++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);
    
    // 手动绑定：按钮的clicked信号 关联到 自定义的槽函数executeCommand
    connect(ui->commitButton, &QPushButton::clicked, this, &Widget::executeCommand);
}

Widget::~Widget()
{
    delete ui;
}

// 自定义命名的槽函数，不再受on_控件名_事件名的限制
void Widget::executeCommand()
{
    QString program = ui->cmdLineEdit->text();
    QProcess *myProcess = new QProcess(this);
    myProcess->start(program);
}
```

通过手动绑定的方式添加`回车`确认的功能：  

```c++
Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);

    // 连接信号与槽 发出信号、发出什么信号、谁处理信号、如何处理
    connect(ui->cmdLineEdit,SIGNAL(returnPressed()),this,SLOT(on_commitButton_clicked()));
}
```

当我们的事件不是很多的时候，连接信号与槽的时候可以是用`lambda`表达式：

```c++
    connect(ui->browseButton,&QPushButton::clicked,[this](){
                QMessageBox::information(this,"信息","点击浏览");
    });
```

- `ui->`用于设置控件
- `this->`用于设置窗口

### Caculator
```cpp
│ - calculator.pro
│ - calculator.pro.user
│ - main.cpp
│ - widget.cpp
│ - widget.h
│ - widget.ui
```
前面没什么可说的，就是设计ui界面，然后绑定按键信号与槽，点击一个按键对字符串进行操作，一个难点是计算这里字符串的结果：
- 方案一：使用`QSJEngine`
`QJSEngine`是Qt提供的`Javascript`解释器,可以将字符串当成`Javascript`来执行。使用时注意在`calculator.pro`文件（`.pro` 是 `qmake` 项目文件，用于描述如何编译你的项目）中加入`qml`模块：
```
QT       += core gui qml
```
| 模块 | 作用 |
  |---------|---------------------------------------|
  | `core`| Qt 核心功能（字符串、容器、文件等）|
  | `gui`| GUI 相关（`QFont`, `QColor` 等） |
  | `widgets` | 窗口控件（`QPushButton`, `QLineEdit` 等）|
  | `qml`| `JavaScript` 引擎和 `QML` 支持  |
  | `network`|网络功能|
  | `sql`| 数据库功能|
注意要先引入头文件：
```cpp
 #include <QJSEngine>
 ```
 ```cpp
 void Widget::on_eqlButton_clicked()
{
    QJSEngine engine;
    QJSValue result=engine.evaluate(expression);
    if(result.isError()){
        ui->mainLineEdit->setText("Error");
        expression.clear();
    }
    else {
        ui->mainLineEdit->setText(result.toString());
        expression=result.toString();
    }
}
```
- 方案二：使用双堆算法实现
```cpp
void Widget::on_eqlButton_clicked()
{
    if (expression.isEmpty()) return;  // 空表达式直接返回

    QStack<double> nums;
    QStack<QChar> ops;

    auto priority = [](QChar op) {
        if (op == '+' || op == '-') return 1;
        if (op == '*' || op == '/') return 2;
        return 0;
    };

    auto calc = [&]() {
        double b = nums.pop();
        double a = nums.pop();
        QChar op = ops.pop();
        switch (op.toLatin1()) {
        case '+': nums.push(a + b); break;
        case '-': nums.push(a - b); break;
        case '*': nums.push(a * b); break;
        case '/': nums.push(a / b); break;
        }
    };

    QString num;
    for (int i = 0; i < expression.size(); ++i) {
        QChar c = expression[i];
        if (c.isDigit() || c == '.') {
            num += c;
        } else {
            if (!num.isEmpty()) {
                nums.push(num.toDouble());
                num.clear();
            }
            if (c == '(') {
                ops.push(c);
            } else if (c == ')') {
                while (!ops.isEmpty() && ops.top() != '(') calc();
                if (!ops.isEmpty() && ops.top() == '(') ops.pop();
            } else {
                while (!ops.isEmpty() && ops.top() != '(' &&
                       priority(ops.top()) >= priority(c)) calc();
                ops.push(c);
            }
        }
    }
    if (!num.isEmpty()) nums.push(num.toDouble());

    while (!ops.isEmpty() && nums.size() >= 2) calc();

    if (nums.isEmpty()) {
        ui->mainLineEdit->setText("Error");
        expression.clear();
    } else {
        double result = nums.pop();
        ui->mainLineEdit->setText(QString::number(result));
        expression = QString::number(result);
    }
}
```

### Timer
 在 C++ 字符串中，\ 是转义字符，用来表示特殊含义：
| 写法 | 实际含义          |
|------|-------------------|
| `\n`   | 换行              |
| `\t`   | 制表符            |
| `\"`   | 双引号字符本身    |
| `\\`   | 一个反斜杠 `\` 本身 |

因此图片路径不可直接使用文件地址：
- 写法1：双反斜杠（转义）：`"C:\\Users\\Legion\\Desktop\\avatar.png"`
- 写法2：正斜杠，Qt会自动处理：`"C:/Users/Legion/Desktop/ avatar.png"`
- 写法3：`R"(...)" `中内容不转义，直接当字面量：`R"(C:\Users\Legion\Desktop\avatar.png)"`
  
注意勾选`QLabel`里的`scaleContent`使得图片随窗口大小等比例缩放
