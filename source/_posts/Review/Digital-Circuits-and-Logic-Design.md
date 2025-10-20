---
title: Digital Circuits and Logic Design
date: 2025-09-04 12:58:09
tags:
archive: true
math: true
category: Review
category_bar: true
---

## 一、数制与编码

#### 整数十进制转换：除X倒取余法
十进制整数 N 可表示为 X 进制的幂次和形式：
$$N = a_{n} \times X^n + a_{n-1} \times X^{n-1} + ... + a_{1} \times X^1 + a_{0} \times X^0$$通过 “除 X 取余”，可依次求出最低位，再 “倒取余” 得到最终结果。
#### 小数十进制转换：乘X顺取整法
十进制小数 M（0<M<1）可表示为 X 进制的负幂次和形式：
$$M = b_{-1} \times X^{-1} + b_{-2} \times X^{-2} + ... + b_{-k} \times X^{-k} + ...$$通过 “乘 X 取整”，可依次求出小数点后第一位，再 “顺取整” 得到 X 进制小数。

#### 原码
在最高位补0或1表示正负，0为正，1为负。

#### 反码
- 对于正数，反码与原码相同
- 对于负数，反码为原码**除符号位外按位取反**
{%note info%}
在反码表示法中，0的表示方法不是唯一的
{%endnote%}
反码表示法的优点是在进行加减运算时**不需要判断两数的符号是否相同**，只要先求出两数的反码然后相加即可（**符号位也参与运算**，当符号位产生进位时，需要**循环进位**，即把符号位的进位加到和的最低位上去），比原码运算要简单的多。

#### 补码
- 对于正数，补码与原码相同
- 对于负数，补码为原码**除符号位外按位取反**，再在**最低位加一**（**补码+1**）
{%note info%}
补码的补码是原码
{%endnote%}
补码进行运算时，符号位参与运算，而且**符号位的进位会丢弃**（区别于反码）

## 二、逻辑代数基础

[相关电路符号图](https://www.cnblogs.com/icmaxwell/p/17374702.html)

### 三种基本逻辑运算：
1. 与：串联`·`
```
0·0=0
0·1=0
1·0=0
1·1=1
```
2. 或：并联`+`
```
0+0=0
0+1=0
1+0=0
1=1=1
```
3. 非：取反`-`

```
A·A=A
A+A=A
```
### 复合逻辑运算
1. 与非：$F=\overline{A·B·C}$
2. 或非: $F=\overline{A+B+C}$
3. 与或非：$F=\overline{AB+CD}$
4. 异或：$ F=A\oplus B =\overline{A}B+A\overline{B}$ **相异为 1，相同为 0**
5. 同或：$ F=A\odot B=AB+\overline{A}\overline{B}$  **相同为 1，相异为 0**

![基本逻辑门电路图形符号1](https://github.com/Richard110206/Blog-image/blob/main/article/Review/DigitalCircuits/circuitsymboltable1.png?raw=true)
![基本逻辑门电路图形符号2](https://github.com/Richard110206/Blog-image/blob/main/article/Review/DigitalCircuits/circuitsymboltable2.png?raw=true)


### 逻辑函数表示方法的相互转换

1. 真值表法
2. 表达式法
3. 逻辑图法
4. 波形图


#### 真值表:arrow_forward::arrow_forward:逻辑式
{%note info %}
1. 从真值表中**找出使得F=1的那些变量取值**
2. 把每一组变量取值写成对应的乘积项, **0 --> 1，1不变**
3. 将**乘积相加**得到逻辑式

{%endnote%}

#### 波形图
1. 输入波形要**穷举所用可能的输入**
2. 输出波形与输入波形**一一对应**
{%fold into@根据波形图求解表达式%}
根据**输出波形为真值**的输入组合，写出逻辑式（类似于 Y=AB+A'B)
{%endfold%}
#### 对偶规则（求对偶函数）
{%note info %}
- 将其中的所有 “**与**”运算（·） 和 “**或**”运算（+） **互换**
- 将所有的常量 **“0” 和 “1” 互换**
- **变量保持不变**
{%endnote%}

#### 反演规则（求反函数）
{%note info %}
- 将其中的所有 “**与**”运算（·） 和 “**或**”运算（+） **互换**
- 将所有的常量 **“0” 和 “1” 互换**
- **变量取反**
{%endnote%}

{%note danger%}
- **长非号不变**
- 运算的**优先顺序不变**（**要加括号**）
{%endnote%}

{%note danger%}
注意对偶规则和反演规则的**区别是变量是否取反**
{%endnote%}

### 定律
#### 摩根定律
$$\overline{A + B} = \overline{A} \cdot \overline{B}$$

$$\overline{A \cdot B} = \overline{A} + \overline{B}$$

#### 吸收律
$$A + A \cdot B = A$$


$$A \cdot (A + B) = A$$


$$A + \overline{A} \cdot B = A + B$$


$$A \cdot (\overline{A} + B) = A \cdot B$$

#### 包含律
$$A \cdot B + \overline{A} \cdot C + B \cdot C = A \cdot B + \overline{A} \cdot C$$

$$(A + B) \cdot (\overline{A} + C) \cdot (B + C) = (A + B) \cdot (\overline{A} + C)$$

#### 尾部变换
$$A \cdot \overline{B} = A \cdot \overline{A \cdot B}$$

$$A \cdot \overline{A \cdot B} = A \cdot (\overline{A} + \overline{B}) = A \cdot \overline{A} + A \cdot \overline{B} = A \cdot \overline{B}$$

### 常用的化简方法

#### 并项法
$$AB+A\overline{B}=A$$
#### 吸收法
$$A+AB=A$$
#### 消去法
$$A+\overline{A}B=A+B$$

$m_0$下标编号规则：原变量取1，反变量取0

n变量的最小项有n个相邻项：
相邻项：只有一个变量不同，一对相邻项可以消去一个变量

### 卡诺图
&emsp;&emsp;卡诺图表示按照格雷码（任意两个相邻的编码，仅有 1 位二进制数不同）进行标识，使得相邻变量的组合中只有一个变量不同。卡诺图**排列顺序**：00->01->11->10

|AB/CD|00|01|11|10|
|----|----|----|----|----|
|00|$m_0$|$m_1$|$m_3$|$m_2$|
|01|$m_4$|$m_5$|$m_7$|$m_6$|
|11|$m_{12}$|$m_{13}$|$m_{15}$|$m_{14}$|
|10|$m_8$|$m_9$|$m_{11}$|$m_{10}$|

{%note danger%}
需要特殊考虑：

- **左右相邻** 

- **上下相邻** 

- **四角相邻**

{%endnote%}

#### 用卡诺图表示逻辑函数
1. 先将卡诺图补充为最小项的形式

{%note info%}

$Y(A,B,C)=A'B'C'+AB'$

$=A'B'C'+AB'(C+C')$

$=A'B'C'+AB'C'+AB'C$

$=m_0+m_4+m_5$

{%endnote%}

2. 在卡诺图中进行填充
- **最小项**在方格中填1
- **最大项**在方格中填0

#### 卡诺图的化简
根据相邻两个小方格中只有一个变量不同，可以合并为一项，消去一个互非的变量。
1.	卡诺圈**越大越好**（必须是$2^n$个方格）
2.	卡诺圈**越少越好**
3.	每一个卡诺圈都**要有新的成分**（所有的 1 都必须要用到）
4.	**先圈大圈，后圈小圈**（ 1 可以重复使用）

#### 具有无关项的逻辑函数及化简
- 约束项：由于输入变量限制而不可能出现的最小项
- 任意项：取值为0或1都不影响正常逻辑功能的最小项
- 无关项：约束项 + 任意项

无关项在卡诺图中用 :x: 表示，可以表示 0 或 1 ，具体选择什么依据能绘制的**矩形圈最大、圈数最小**为准！

## 三、基本逻辑门电路
{%note info%}
刚开始学习**基本逻辑门电路**的时候会有点疑惑：为什么这里的电路与高中的电路如此不同，**不是闭合的电路**，**没有了电源正负极符号**，取而代之的是一个个冰冷的出入线头？
{%endnote%}

事实上，这与我们学习的目标有关！

- 高中物理电路更关注的是**能量的流动和转换**。通过**欧姆定律**研究电压、电流、电阻之间的关系。因此需要画出完整的回路，包括电源、负载和导线。
- 而数字逻辑门电路在高中的基础上更进一步，关注的是**信息的处理和传递**。为了简洁只画核心的 “**信号输入 / 输出部分**”，不关心电流具体多少，只关心 “输入 / 输出电压**对应逻辑 0 还是 1**”（高电平还是低电平）

{%note danger%}
高电平和低电平都是一种状态，一种状态**对应一个范围值**，而不是一个固定值
{%endnote%}

1.	高低电平会偏移
2.	负载会影响输出信号

### 三极管
- 集电极
- 发射极
- 基极

相当于一个开关，当**给基极接入电源**（支路）$>0.7V$，晶体管接通（相当于闭合的开关），电路处于**导通**状态，先是遵循  $i_C=\beta{i_B}$ 的线性变化，增加到一定值不在增加，由**放大状态进入饱和状态**，进入饱和状态的**主路压降**很低（硅管$\approx0.3V$）。

{%fold into@为何要多此一举？制作一个控制开关的开关？%}
相当于一个放大器，可以使用**小电流控制大电流**
{%endfold%}

### MOS管
- 栅极（G极）
- 源极（S极）
- 漏极（D极）

与三极管类似，相当于一个开关，给**栅极加上电压**，D极和S极会**形成通路**。

{%fold into@都是用通过第三方控制主电路的开关，三极管和MOS管有什么区别呢？%}
- 显然，三极管通过电流控制，而MOS管通过电压控制。由于MOS管的**绝缘层**导致电阻极大（上亿欧姆），支路电流很小几乎为零，耗电很少；而三极管基极要**持续提供电流**，耗电相对较大。
{%endfold%}


[数字电子技术基础（四）：门电路（二极管）](https://blog.csdn.net/qq_40483920/article/details/107966986)
上拉电阻：接在输出端和正电源 (Vcc) 之间的电阻，将不确定的信号 “拉” 到高电平，提供一个稳定的高电平输出。二极管与门。
下拉电阻：接在输出端和地 (GND) 之间的电阻，将不确定的信号 “拉” 到低电平，提供一个稳定的低电平输出。二极管或门。

## 四、逻辑电路
|组合逻辑电路|	时序逻辑电路|
|:---:|:---:|
|输出只与当前的输入有关，与电路过去的工作状态无关|输出与电路过去的工作状态有关|
|不包含存储单元|包含存储单元|

根据逻辑电路写出逻辑函数表达式用卡诺图进行化简绘制真值表得到逻辑功能

流程也可以是反过来：绘制逻辑电路图示先画输出变量的原变量和反变量共同输出的结构

### 编码器
编码器是用二进制码来表示每个给定的信息符号。
#### 抢答器


**普通编码器**：任何时刻只有一个输入信号
<div style="width: 100%; overflow: auto;">
    <table style="width: 100%;">
        <tr>
            <td style="text-align: center;" colspan="8">输入</td>
            <td style="text-align: center;" colspan="3">输出</td>
        </tr>
        <tr>
            <td style="text-align: center;">$I_0$</td>
            <td style="text-align: center;">$I_1$</td>
            <td style="text-align: center;">$I_2$</td>
            <td style="text-align: center;">$I_3$</td>
            <td style="text-align: center;">$I_4$</td>
            <td style="text-align: center;">$I_5$</td>
            <td style="text-align: center;">$I_6$</td>
            <td style="text-align: center;">$I_7$</td>
            <td style="text-align: center;">$Y_2$</td>
            <td style="text-align: center;">$Y_1$</td>
            <td style="text-align: center;">$Y_0$</td>
        </tr>
        <tr>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
        </tr>
        <tr>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
        </tr>
        <tr>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
        </tr>
        <tr>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">1</td>
        </tr>
        <tr>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
        </tr>
        <tr>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
        </tr>
        <tr>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
        </tr>
        <tr>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">1</td>
        </tr>
    </table>
</div>
$$Y_2=I_4+I_5+I_6+I_7$$
$$Y_1=I_2+I_3+I_6+I_7$$
$$Y_0=I_1+I_3+I_5+I_7$$

**优先编码器**：允许同时输入两个以上的信号，但只对其中优先权更高的进行编码

{%note danger%}
注意优先编码器均**使用反变量**作为输入和输出，并且输入**0为有效输入**，**1为无效输入**
{%endnote%}

<div style="width: 100%; overflow: auto;">
    <table style="width: 100%;">
        <thead>
            <tr>
                <th style="text-align: center;" colspan="8">输 入</th>
                <th style="text-align: center;" colspan="3">输 出</th>
            </tr>
            <tr>
                <th style="text-align: center;">$I_0'$</th>
                <th style="text-align: center;">$I_1'$</th>
                <th style="text-align: center;">$I_2'$</th>
                <th style="text-align: center;">$I_3'$</th>
                <th style="text-align: center;">$I_4'$</th>
                <th style="text-align: center;">$I_5'$</th>
                <th style="text-align: center;">$I_6'$</th>
                <th style="text-align: center;">$I_7'$</th>
                <th style="text-align: center;">$Y_2'$</th>
                <th style="text-align: center;">$Y_1'$</th>
                <th style="text-align: center;">$Y_0'$</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center; color: red;">0</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
            </tr>
            <tr>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center; color: red;">0</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">0</td>
            </tr>
            <tr>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center; color: red;">0</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">0</td>
                <td style="text-align: center;">1</td>
            </tr>
            <tr>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center; color: red;">0</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">0</td>
                <td style="text-align: center;">0</td>
            </tr>
            <tr>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center; color: red;">0</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">0</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
            </tr>
            <tr>
                <td style="text-align: center;">×</td>
                <td style="text-align: center;">×</td>
                <td style="text-align: center; color: red;">0</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">0</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">0</td>
            </tr>
            <tr>
                <td style="text-align: center;">×</td>
                <td style="text-align: center; color: red;">0</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">0</td>
                <td style="text-align: center;">0</td>
                <td style="text-align: center;">1</td>
            </tr>
            <tr>
                <td style="text-align: center; color: red;">0</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">1</td>
                <td style="text-align: center;">0</td>
                <td style="text-align: center;">0</td>
                <td style="text-align: center;">0</td>
            </tr>
        </tbody>
    </table>
</div>

{%note danger%}
输入数越高**优先级越大**
{%endnote%}

{%note info%}
实例：74HC148，电路原件中圆圈表示低电平有效
{%endnote%}

{%fold into@如何寻找芯片的 1 引脚？%}
将芯片**缺口放在左边**，左下角第一个为1号引脚，**逆时针方向旋转**计数引脚依次增大
{%endfold%}

有选通输入端 $S'$、选通输出端$Y_{s}'$，只有当选通输入端为低电平（接地），芯片才正常导通，否则输出端都是高电平（无效）；选通输出端**控制下一级电路是否工作**（一般当前有输入时选通输出端 $Y_{s}'$ 输出 1，禁止下一级电路工作）

{%fold into@案例%}
用两个8线-3线优先编码器接成16线-4线优先编码器
{%endfold%}

<div style="width: 100%; overflow: auto;">
    <table style="width: 100%;">
        <tr>
            <td style="text-align: center;" colspan="3">输入</td>
            <td style="text-align: center;" colspan="8">输出</td>
        </tr>
        <tr>
            <td style="text-align: center;">$A_2$</td>
            <td style="text-align: center;">$A_1$</td>
            <td style="text-align: center;">$A_0$</td>
            <td style="text-align: center;">$Y_7$</td>
            <td style="text-align: center;">$Y_6$</td>
            <td style="text-align: center;">$Y_5$</td>
            <td style="text-align: center;">$Y_4$</td>
            <td style="text-align: center;">$Y_3$</td>
            <td style="text-align: center;">$Y_2$</td>
            <td style="text-align: center;">$Y_1$</td>
            <td style="text-align: center;">$Y_0$</td>
        </tr>
        <tr>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
        </tr>
        <tr>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
        </tr>
        <tr>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
        </tr>
        <tr>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
        </tr>
        <tr>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
        </tr>
        <tr>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
        </tr>
        <tr>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
        </tr>
        <tr>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">1</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
            <td style="text-align: center;">0</td>
        </tr>
    </table>
</div>

{%fold into@案例%}
用两个3线-8线译码器组成4线-16线译码器
{%endfold%}

### 显示译码器 
#### 七段字符显示器
四位二进制数（0000-1001）
多输入多输出 有效（高低电平均可）

一位数值比较器


两片3/8译码器组合为4/16译码器
一个芯片工作，通过使能端禁止另一个芯片工作，一个连接高电平有效，一个低电平有效


数据选择器/多路开关
数据输入端 相当于电视频道 D表示
数据选择（输入）端 相当于遥控器 A表示
数据输出端

74LS85芯片
比较器
