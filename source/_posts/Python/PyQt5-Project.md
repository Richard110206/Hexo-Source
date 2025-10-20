---
title: PyQt5 Project
date: 2025-10-15 15:37:27
tags:
archive: true
---

&emsp;&emsp;经过过去一年的学习，踩过的无数的坑告诉我单单学习语法是远远不够的，看似知识点面面俱到，实则之间功能的使用难以串联起来，且一段时间后便会大幅遗忘，所以现在我打算以项目驱动的方式进行学习，在完成简单项目的过程中对代码进行梳理和学习！

##### This is my first trial,it's time for pyqt5,let's go!

## PyQt5
后面会先附上项目完整代码，再对代码进行**逐行的梳理和解析**！

### Labels
[Python PyQt5 LABELS are easy! 🏷️](https://www.youtube.com/watch?v=nFLADhwXjW4)
```python
import sys
from PyQt5.QtWidgets import QApplication,QMainWindow,QLabel
from PyQt5.QtGui import QFont
from PyQt5.QtCore import Qt

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("My cool first GUI")
        self.setGeometry(300, 300, 400, 300)

        label=QLabel("Hello",self)
        label.setFont(QFont("Arial",40))
        label.setGeometry(50,50,200,50)
        label.setStyleSheet("color:blue;"
                            "background-color:gray;"
                            "font-weight:bold;"
                            "font-style:italic;"
                            "text-decoration:underline")

        # label.setAlignment(Qt.AlignTop)
        # label.setAlignment(Qt.AlignBottom)
        # label.setAlignment(Qt.AlignHCenter)

        # label.setAlignment(Qt.AlignRight)
        # label.setAlignment(Qt.AlignHCenter)
        # label.setAlignment(Qt.AlignLeft)

        label.setAlignment(Qt.AlignHCenter | Qt.AlignTop)
        label.setAlignment(Qt.AlignHCenter | Qt.AlignBottom)
        label.setAlignment(Qt.AlignHCenter | Qt.AlignVCenter)


def main():
    app=QApplication(sys.argv)
    window=MainWindow()
    window.show()
    sys.exit(app.exec_())

if __name__=="__main__":
    main()
```


```python
import sys
```
 导入`sys`模块，可以理解为这是 Python 与它自身运行环境以及操作系统之间的一个桥梁。

{%note info%}
#### 1.与命令行交互
- `sys.argv` 是一个列表，包含了命令行参数。
  - `sys.argv[0]` 是脚本的名称。
  - `sys.argv[1:]` 是用户在命令行中输入的参数列表。
```python
import sys

print("脚本名:", sys.argv[0])
print("参数个数:", len(sys.argv))

for i, arg in enumerate(sys.argv):
    print(f"参数 {i}: {arg}")
```
当你从终端或命令提示符运行一个 Python 脚本时，在脚本名后面输入的参数都会被存储在这里：
```text
richard@RicharddeMacBook-Air pyqt5 % python3 test1.py hello world          
脚本名: test1.py
参数个数: 3
参数 0: test1.py
参数 1: hello
参数 2: world
```
#### 2.控制程序退出
- `sys.exit([arg])`：用于退出 Python 程序。通常用于在某种条件满足时提前结束程序。
  - `arg` 是可选的退出状态码。默认值为 0，表示正常退出。非零值通常用于表示异常退出。

#### 3.查看系统路径和模块
- `sys.path` 是一个列表，包含了 Python 解释器搜索模块的路径。
  - 当你导入一个模块时，Python 解释器会在 `sys.path` 中列出的路径中搜索该模块。
- `sys.modules` 是一个字典，包含了当前已导入的模块。
  - 键是模块的名称，值是模块对象。
  - 你可以使用 `sys.modules` 来检查某个模块是否已经导入，或者获取已导入模块的引用。
```python
import sys

# 打印当前所有的模块搜索路径
for path in sys.path:
    print(path)
"""
/Users/richard/PyCharmMiscProject
/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python39.zip
/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9
/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/lib-dynload
/Users/richard/Library/Python/3.9/lib/python/site-packages
/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/site-packages
"""
```

#### 4.获取系统/解释器信息
- `sys.version` 获取当前 Python 解释器的版本信息。
- `sys.platform` 获取当前操作系统的名称，常用于编写跨平台代码。
```python
import sys
str=sys.platform

if str=='win32': # windows
    print(f"这是在{str}上运行的程序")
elif str=='darwin': # macos
    print(f"这是在{str}上运行的程序")
elif str=='linux':
    print(f"这是在{str}上运行的程序")
else:
    print(f"这是在{str}上运行的程序")
```
{%endnote%}
```python
from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel
```
- `PyQt5.QtWidgets`：Qt 的窗口组件模块，包含构建 GUI 的核心类。
  - `QApplication`：应用程序主类，管理应用程序的生命周期、事件循环等，每个 PyQt 程序必须有且仅有一个QApplication实例。
  - `QMainWindow`：主窗口类，提供标准的窗口结构（标题栏、菜单栏、工具栏等），适合作为程序的主窗口。
  - `QLabel`：标签组件，用于显示文本或图像。

```python
from PyQt5.QtCore import Qt
```
- `PyQt5.QtCore`：核心模块，包含 Qt 框架的基础类和功能。
  - `Qt`：枚举类，包含了 Qt 框架中常用的常量和枚举值。

```python
from PyQt5.QtGui import QFont
```
- `PyQt5.QtGui`：图形相关模块，包含字体、颜色、图像等视觉元素的类。
  - `QFont`：字体类，用于设置文本的字体、大小、样式等。
```python
class MainWindow(QMainWindow):
```
定义`MainWindow`类，继承自`QMainWindow`，表示程序的主窗口。通过继承，我们可以复用`QMainWindow`的所有功能（如窗口大小调整、标题设置等），并添加自定义逻辑。
```python
    def __init__(self):
        super().__init__()
```
类的初始化方法（构造函数），用于初始化窗口属性。
`super().__init__()`：调用父类`QMainWindow`的初始化方法，确保父类的功能正常工作（必须调用，否则窗口可能无法正常显示）。

```python
self.setWindowTitle("My cool first GUI")
```
`setWindowTitle()`：`QMainWindow`的方法，设置窗口标题为字符串参数内容。

```python
self.setGeometry(300, 300, 400, 300)
```
`setGeometry(x, y, width, height)`：设置窗口的位置和大小。

**参数含义：**
- `x` 和 `y` 分别表示窗口左上角相对于屏幕的坐标(坐标原点在屏幕左上角，向右为 x 轴正方向，向下为 y 轴正方向)
- `width` 和 `height` 分别表示窗口的宽度和高度

```python
label = QLabel("Hello", self)
```
创建`QLabel`实例，显示文本，并指定父组件为`self`（即当前`MainWindow`窗口）。
```python
label.setFont(QFont("Arial", 40))
```
设置标签的大小和字体
```python
label.setGeometry(50, 50, 200, 50)
```
设置标签在父组件中的位置和大小（相对于父窗口左上角）
```python
label.setStyleSheet("color:blue;"
                    "background-color:gray;" # 背景颜色
                    "font-weight:bold;" # 字体加粗
                    "font-style:italic;" # 字体斜体
                    "text-decoration:underline") # 下划线
```
`setStyleSheet()`：通过 CSS 样式表设置组件的外观（PyQt 支持部分 CSS 语法）。

```python
label.setAlignment(Qt.AlignTop) # 垂直顶部对齐
label.setAlignment(Qt.AlignBottom) # 垂直底部对齐
label.setAlignment(Qt.AlignHCenter) # 水平居中对齐
label.setAlignment(Qt.AlignRight) # 水平右对齐
label.setAlignment(Qt.AlignLeft) # 水平左对齐
```
`|`（位或运算符）实现组合对齐方式：
```python
label.setAlignment(Qt.AlignHCenter | Qt.AlignTop)
label.setAlignment(Qt.AlignHCenter | Qt.AlignBottom)
label.setAlignment(Qt.AlignHCenter | Qt.AlignVCenter)
```
```python
def main():
    app = QApplication(sys.argv) # 创建QApplication实例
    window = MainWindow() # 创建窗口
    window.show() # 显示窗口
```
PyQt 组件默认隐藏，必须调用`show()`才会显示



```python
import sys
from PyQt5.QtWidgets import QApplication,QMainWindow,QLabel
from PyQt5.QtGui import QPixmap

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("My cool first GUI")
        self.setGeometry(300, 300, 400, 300)

        label=QLabel(self)
        label.setGeometry(0,0,250,250)

        pixmap=QPixmap("../lqx.png")
        label.setPixmap(pixmap)

        label.setScaledContents(True)
        # label.setGeometry(self.width()-label.width(),self.width()-label.width(),label.width(),label.height())
        label.setGeometry((self.width()-label.width())//2,(self.height()-label.height())//2,label.width(),label.height())

def main():
    app=QApplication(sys.argv)
    window=MainWindow()
    window.show()
    sys.exit(app.exec_())

if __name__=="__main__":
    main()
```

- `QPixmap`：Qt 中用于处理图像的类，支持加载多种格式图片（如 PNG、JPG 等）
```python
label.setPixmap(pixmap)
```
- `setPixmap()`：`QLabel`的方法，将`QPixmap`对象设置为标签的显示内容，使标签从显示文本转为显示图像。
```python
label.setScaledContents(True)
```
- `setScaledContents(bool)`：设置图像是否自适应标签大小。
  - `True`：图像会自动缩放以填满整个标签（可能改变宽高比）。
  - `False`（默认）：图像按原始大小显示，若超过标签尺寸则被截断。

### Layout
[Python PyQt5 LAYOUT MANAGERS are easy! 🧲](https://www.youtube.com/watch?v=ml-mBl77h6Q)
```python
import sys
from PyQt5.QtWidgets import (QApplication,QMainWindow,QLabel,QWidget,QVBoxLayout,QHBoxLayout,QGridLayout)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setGeometry(300, 300, 400, 300)
        self.initUI()

    def initUI(self):
        central_widget=QWidget()
        self.setCentralWidget(central_widget)

        label1 = QLabel("#1",self)
        label2 = QLabel("#2", self)
        label3 = QLabel("#3", self)
        label4 = QLabel("#4", self)
        label5 = QLabel("#5", self)

        label1.setStyleSheet("background-color:red")
        label2.setStyleSheet("background-color:yellow")
        label3.setStyleSheet("background-color:green")
        label4.setStyleSheet("background-color:blue")
        label5.setStyleSheet("background-color:purple")
        """
        vbox = QVBoxLayout()

        vbox.addWidget(label1)
        vbox.addWidget(label2)
        vbox.addWidget(label3)
        vbox.addWidget(label4)
        vbox.addWidget(label5)

        central_widget.setLayout(vbox)
        """
        # 5 个标签垂直排列，宽度默认填满窗口，高度平均分配
        """
        hbox=QHBoxLayout()

        hbox.addWidget(label1)
        hbox.addWidget(label2)
        hbox.addWidget(label3)
        hbox.addWidget(label4)
        hbox.addWidget(label5)

        central_widget.setLayout(hbox)
        """
        # 5 个标签水平排列，高度默认填满窗口，宽度平均分配。

        grid=QGridLayout()

        grid.addWidget(label1,0,0)
        grid.addWidget(label2,0,1)
        grid.addWidget(label3,1,0)
        grid.addWidget(label4,1,1)
        grid.addWidget(label5,1,2)
        # 组件按行列坐标排列，形成类似表格的结构：
        central_widget.setLayout(grid)

def main():
    app=QApplication(sys.argv)
    window=MainWindow()
    window.show()
    sys.exit(app.exec_())

if __name__=="__main__":
    main()
```

```python
from PyQt5.QtWidgets import QVBoxLayout, QHBoxLayout, QGridLayout
```

- `QVBoxLayout`：**垂直**布局，组件按从上到下的顺序排列。
- `QHBoxLayout`：**水平**布局，组件按从左到右的顺序排列。
- `QGridLayout`：**网格**布局，组件按行列坐标定位，类似表格。

### Button
[Python PyQt5 BUTTONS are easy! 🛎️](https://www.youtube.com/watch?v=9pl55MxZlG4)
```python
import sys
from PyQt5.QtWidgets import (QApplication,QMainWindow,QLabel,QPushButton)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setGeometry(700, 300, 500, 500)
        self.button=QPushButton("Click me!",self)
        # 创建QPushButton实例，父组件为self（即主窗口）
        self.label=QLabel("Hello",self)
        self.initUI()


    def initUI(self):
        self.button.setGeometry(150,200,200,100)
        self.button.setStyleSheet("font-size:30px")
        self.button.clicked.connect(self.on_click)

        self.label.setGeometry(210,300,200,100)
        self.label.setStyleSheet("font-size:30px")

    def on_click(self):
        """
        print("Button clicked")
        self.button.setText("Clicked")  # 修改按钮文本，反馈点击状态
        self.button.setDisabled(True)  # 禁用按钮（变为灰色，无法再次点击）
        """
        self.label.setText("Goodbye")

def main():
    app=QApplication(sys.argv)
    window=MainWindow()
    window.show()
    sys.exit(app.exec_())

if __name__=="__main__":
    main()
```

```python
self.button.clicked.connect(self.on_click)
```
- `clicked`：`QPushButton`的核心信号，当按钮被点击时触发（无需传递状态参数，只需响应点击动作）。
- `connect`：将信号绑定到自定义槽函数`on_click`，实现点击后的逻辑处理




### Checkbox
[Python PyQt5 CHECKBOXES are easy! ✅](https://www.youtube.com/watch?v=VgnUB_vzR9I)
```python
import sys
from PyQt5.QtWidgets import QApplication,QMainWindow,QCheckBox
from PyQt5.QtCore import Qt

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setGeometry(700, 300, 500, 500)
        self.checkbox = QCheckBox("Do you like food",self)
        self.initUI()

    def initUI(self):
        self.checkbox.setGeometry(10,0,500,100)
        self.checkbox.setStyleSheet("font-size: 30x;"
                                    "font-family: Arial;")
        self.checkbox.setChecked(True)
        # 确定默认是否为选中状态
        self.checkbox.stateChanged.connect(self.checkbox_changed)

    def checkbox_changed(self,state):
        # print(state)
        # if state==2:
        if state == Qt.Checked:
            print("You like food")
        else:
            print("You don't like food")

def main():
    app=QApplication(sys.argv)
    window=MainWindow()
    window.show()
    sys.exit(app.exec_())

if __name__=="__main__":
    main()
```
### Radio Button
[Python PyQt5 RADIO BUTTONS are easy! 🔘](https://www.youtube.com/watch?v=DZ3-ij_JHE0)
```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QRadioButton,QButtonGroup

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setGeometry(700,300,500,400)
        self.radio1=QRadioButton("Visa",self)
        self.radio2 = QRadioButton("Mastercard", self)
        self.radio3 = QRadioButton("Gift Card", self)
        self.radio4 = QRadioButton("In-Store", self)
        self.radio5 = QRadioButton("Online", self)
        self.button_group1=QButtonGroup(self)
        self.button_group2=QButtonGroup(self)
        self.initUI()

    def initUI(self):
        self.radio1.setGeometry(0,0,300,50)
        self.radio2.setGeometry(0,50,300,50)
        self.radio3.setGeometry(0,100,300,50)
        self.radio4.setGeometry(0, 150, 300, 50)
        self.radio5.setGeometry(0, 200, 300, 50)

        self.setStyleSheet("QRadioButton{"
                           "font-size:20px;"
                           "font-family:Arial;"
                           "padding:10px;"
                           "}")

        self.button_group1.addButton(self.radio1)
        self.button_group1.addButton(self.radio2)
        self.button_group1.addButton(self.radio3)
        self.button_group2.addButton(self.radio4)
        self.button_group2.addButton(self.radio5)

        self.radio1.toggled.connect(self.radio_button_changed)
        self.radio2.toggled.connect(self.radio_button_changed)
        self.radio3.toggled.connect(self.radio_button_changed)
        self.radio4.toggled.connect(self.radio_button_changed)
        self.radio5.toggled.connect(self.radio_button_changed)

    def radio_button_changed(self, checked):
        radio_button = self.sender()
        if radio_button.isChecked():
            print(f"{radio_button.text()} is selected")
        # print("You select something!")



def main():
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    sys.exit(app.exec_())

if __name__ == '__main__':
    main()
```

- `QRadioButton`：单选按钮组件，用于从多个选项中选择唯一一项（与复选框的多选项不同），常见于互斥选项场景（如支付方式、性别选择）。
- `QButtonGroup`：按钮组组件，用于管理一组单选按钮，实现 "互斥选择"（同一组内只能有一个按钮被选中），简化分组逻辑。

### Signal and Slot
- **信号（`Signal`）**：组件在特定事件发生时发出的 “通知”，是事件的 “触发器”。比如按钮被点击（`clicked`）、复选框状态变化（`stateChanged`）、单选按钮选中切换（`toggled`）等，都是组件自带的信号。
- **槽（`Slot`）**：用于接收信号并执行具体逻辑的函数，是事件的 “处理器”，如自定义函数（如`on_click`）。

#### 1. 准备组件（创建信号源和处理对象）
先创建产生信号的组件（如按钮QPushButton）和需要被处理的对象（如标签QLabel），确保组件被正确初始化。
```python
# 1. 创建信号源（按钮，点击时发信号）
self.button = QPushButton("点击修改文本", self)
# 2. 创建处理对象（标签，文本需被修改）
self.label = QLabel("初始文本", self)
```

#### 2. 定义槽函数（编写信号触发后的逻辑）
自定义一个**函数**，用于**处理信号触发后的具体操作**（如修改标签文本、打印日志等）。槽函数的参数需与信号传递的参数匹配（可选，部分信号无参数）。
```python
# 自定义槽函数：接收信号后修改标签文本
def on_button_click(self):
    self.label.setText("按钮被点击啦！")
```

#### 3. 绑定信号与槽（建立关联）
通过组件的`connect()`方法，将 “信号” 与 “槽函数” 绑定，完成 “事件→响应” 的关联。**`语法：信号源.信号.connect(槽函数)`**
```python
# 将按钮的"clicked"信号，绑定到自定义槽函数on_button_click
self.button.clicked.connect(self.on_button_click)
```