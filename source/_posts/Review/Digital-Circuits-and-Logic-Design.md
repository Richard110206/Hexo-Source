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

## 四、组合逻辑电路
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

### 数据选择器
多输入多输出 有效（高低电平均可）

<div style="width: 100%; overflow: auto;">
    <table style="width: 100%;">
    <tr>
      <th style="text-align: center;">A</th>
      <th style="text-align: center;">D₁</th>
      <th style="text-align: center;">D₀</th>
      <th style="text-align: center;">Y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;">0</td>
      <td style="text-align: center;">0</td>
      <td style="text-align: center;">0</td>
      <td style="text-align: center;">0</td>
    </tr>
    <tr>
      <td style="text-align: center;">0</td>
      <td style="text-align: center;">0</td>
      <td style="text-align: center;">1</td>
      <td style="text-align: center;">1</td>
    </tr>
    <tr>
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
    </tr>
    <tr>
      <td style="text-align: center;">1</td>
      <td style="text-align: center;">0</td>
      <td style="text-align: center;">0</td>
      <td style="text-align: center;">0</td>
    </tr>
    <tr>
      <td style="text-align: center;">1</td>
      <td style="text-align: center;">0</td>
      <td style="text-align: center;">1</td>
      <td style="text-align: center;">0</td>
    </tr>
    <tr>
      <td style="text-align: center;">1</td>
      <td style="text-align: center;">1</td>
      <td style="text-align: center;">0</td>
      <td style="text-align: center;">1</td>
    </tr>
    <tr>
      <td style="text-align: center;">1</td>
      <td style="text-align: center;">1</td>
      <td style="text-align: center;">1</td>
      <td style="text-align: center;">1</td>
        </tr>
    </table>
</div>

二选一：
$$ Y=D_0 A'+D_1A$$
四选一：
$$ Y = S\left( D_0 (\overline{A_1} \overline{A_0}) + D_1 (\overline{A_1} A_0) + D_2 (A_1 \overline{A_0}) + D_3 (A_1 A_0) \right)$$
$ S $: 控制输入端（若低电平有效可直接接地）

#### 三人表决器
- 一般问题：多数同意为通过
- 重要问题：全部同意为通过

- 输入变量：P(1号)、 Q（2号）、R（3号）、T（问题类型）
- 输出变量：Z（表决结果）

- 数据选择器：多输入，单输出
- 数据分配器：单输入，多输出
二者功能相反，互为可逆过程

### 加法器
- 半加器：两个一位二进制数相加，不考虑进位
- 全加器：两个一位二进制数相加，考虑进位
  - 输入变量：A（加数）,B（加数）,CI（从低位的进位输入）
  - 输出变量：S（和）, CO（向高位的进位输出）

#### 多位串行加法器
依次将进位加法器的进位输出端CO接到高位全加器的进位输出端CI就可以构成多位串行加法器。
![多位串行加法器示意图](https://github.com/Richard110206/Blog-image/blob/main/article/Review/DigitalCircuits/adder.png?raw=true)
- 缺点：低位运算结束产生进位后，高位才能开始全加运算，运算速度较慢。 
- 优点：计算结构较为简单

#### 超前进位加法器
通过逻辑电路事先知道每一位的进位输出信号，而无需从最低位开始向高位逐位传递进位信号。
- 优点：每一位的进位输入基本上同时知道，运算速度快。
- 缺点：计算结构较为复杂，随着数位增加，电路复杂程度急剧上升。
一位数值比较器

## 五、集成触发器
[时序逻辑电路总结【一】触发器](https://blog.csdn.net/m0_63028174/article/details/128995698?ops_request_misc=%257B%2522request%255Fid%2522%253A%252241ccdfc61df19f77f26d4cdb84276985%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=41ccdfc61df19f77f26d4cdb84276985&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-128995698-null-null.142^v102^pc_search_result_base2&utm_term=RS%E8%A7%A6%E5%8F%91%E5%99%A8&spm=1018.2226.3001.4187)
触发器：具有记忆功能（引入了**反馈机制**）的基本逻辑单元，输出状态**不止与现时的的输入**有关，还**与原来的输入**有关。
- 有外触发器：状态改变
- 触发信号撤除：维持状态不变
### (I) RS触发器
- 输入信号：$\overline{R_D}$（复位端，低电平有效）、$\overline{S_D}$（置位端，低电平有效）
- 输出信号：$Qⁿ$（现态，触发器当前状态）、$Q^{n+1}$（次态，触发后新状态）
<table>
  <thead>
    <tr>
      <th>$\overline{R_D}$</th>
      <th>$\overline{S_D}$</th>
      <th>$Q^{n+1}$</th>
      <th>$\overline{Q^{n+1}}$</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>$1$</td>
      <td>$1$</td>
      <td colspan="2" style="text-align: center;">不变</td>
    </tr>
    <tr>
      <td>$0$</td>
      <td>$1$</td>
      <td>$0$</td>
      <td>$1$</td>
    </tr>
    <tr>
      <td>$1$</td>
      <td>$0$</td>
      <td>$1$</td>
      <td>$0$</td>
    </tr>
    <tr>
      <td>$0$</td>
      <td>$0$</td>
      <td colspan="2" style="text-align: center;">不定</td>
    </tr>
  </tbody>
</table>

约束条件：$\overline{R_D}+\overline{S_D}=1$（此时两个输出端不再满足互补关系，是禁止出现的输入项）

绘制卡诺图（自变量：$\overline{R_D}$、$\overline{S_D}$、$Qⁿ$；因变量：$Q^{n+1}$）（不定项可当做1处理）。
| $\overline{R_D}\overline{S_D}$\$Qⁿ$ | 0（$Qⁿ=0$） | 1（$Qⁿ=1$） |
| :----------------------------------: | :----------: | :----------: |
| 00（$\overline{R_D}=0,\overline{S_D}=0$） | 1（不定按1） | 1（不定按1） |
| 01（$\overline{R_D}=0,\overline{S_D}=1$） | 0           | 0           |
| 11（$\overline{R_D}=1,\overline{S_D}=1$） | 0           | 1           |
| 10（$\overline{R_D}=1,\overline{S_D}=0$） | 1           | 1           |

写出表达式：$Q^{n+1} = \overline{R_D}Q^{n}+S_{D}$

同步触发器：在基本RS触发器基础上增加**CP时钟控制端**，实现“时钟同步触发”
- 当$CP=0$时：时钟无效，无论$\overline{R_D}$、$\overline{S_D}$状态如何，触发器保持原有状态（$Q^{n+1}=Qⁿ$）。
- 当$CP=1$时：时钟有效，触发器功能与基本RS触发器完全相同，即次态由$\overline{R_D}$、$\overline{S_D}$和$Qⁿ$共同决定，需满足约束条件$\overline{R_D} + \overline{S_D} = 1$。


异步置位、同步复位的RS触发器：异步端（$\overline{R_D}$、$\overline{S_D}$）的优先级高于CP时钟，常用于电路上电初始化或紧急置位/复位场景。
- **异步置位（$\overline{S_D}$）**：只要$\overline{S_D}$加低电平，无需等待CP时钟，触发器立刻置1（$Q=1$），不受CP和其他输入信号控制。
- **异步复位（$\overline{R_D}$）**：只要$\overline{R_D}$加低电平，无需等待CP时钟，触发器立刻置0（$Q=0$），不受CP和其他输入信号控制。

### (II) 钟控JK触发器
钟控JK触发器是在同步RS触发器基础上改进而来，通过将输出端$Q$和$\overline{Q}$反馈到输入端，**彻底解决了RS触发器的“不定态”问题**，是应用更广泛的时钟控制触发器。

- 输入信号：$J$（置位控制端）、$K$（复位控制端）、$CP$（时钟控制端，高电平有效）
- 输出信号：$Qⁿ$（现态）、$Q^{n+1}$（次态）
- 反馈机制：$J$端与$\overline{Q}$相连，$K$端与$Q$相连，确保输入组合始终合法。

| $CP$ | $J$ | $K$ | $Q^{n+1}$ | 功能说明               |
| :---: | :---: | :---: | :--------: | :---------------------: |
| 0    | ×  | ×  | $Qⁿ$      | 保持（时钟无效，记忆原状态） |
| 1    | 0  | 0  | $Qⁿ$      | 保持（输入无置位/复位指令） |
| 1    | 0  | 1  | 0         | 复位（置0，$K$端有效）    |
| 1    | 1  | 0  | 1         | 置位（置1，$J$端有效）    |
| 1    | 1  | 1  | $\overline{Qⁿ}$ | 翻转（输出状态与原状态相反） |

根据特性表推导，钟控JK触发器（高电平有效）的次态表达式为：  
$Q^{n+1} = J\overline{Qⁿ} + \overline{K}Qⁿ$（$CP=1$时有效，$CP=0$时$Q^{n+1}=Qⁿ$）  
该表达式无约束条件，因反馈机制已排除不定态。

### (III) 边沿触发器
边沿触发器是对钟控触发器的进一步优化，**仅在时钟信号（CP）的上升沿（↑）或下降沿（↓）瞬间触发**，CP电平稳定期间（高/低电平）输入信号变化不影响输出，彻底解决了“空翻”问题。

- **边沿JK触发器**：
  - 输入输出：与钟控JK触发器一致（$J$、$K$、$CP$、$Qⁿ$、$Q^{n+1}$）。
  - 特性表：仅触发时机变为“CP边沿”，功能逻辑与钟控JK触发器相同（保持、置0、置1、翻转）。
  - 次态表达式：$Q^{n+1} = J\overline{Qⁿ} + \overline{K}Qⁿ$（仅CP↑或CP↓时有效）。
  - 符号标识：CP端旁标注“↑”（上升沿）或“↓”（下降沿），区分触发类型。
  
- **边沿D触发器**：
  - 输入输出：$D$（数据输入端）、$CP$（边沿触发）、$Qⁿ$（现态）、$Q^{n+1}$（次态）。
  - 特性表：

| $CP$ | $D$ | $Q^{n+1}$ | 功能说明               |
| :--- | :--- | :-------- | :--------------------- |
| ×（非边沿） | ×  | $Qⁿ$      | 保持（非触发时刻）     |
| ↑/↓（边沿） | 0  | 0         | 置0（$D=0$时边沿触发） |
| ↑/↓（边沿） | 1  | 1         | 置1（$D=1$时边沿触发） |

- 次态表达式：$Q^{n+1} = D$（仅CP边沿时有效），逻辑简单，常用于数据锁存、移位寄存器。
- 无空翻：仅边沿触发，CP电平稳定期输入变化不影响输出，适合高频数字电路。
- 抗干扰强：对CP电平波动和输入噪声不敏感，电路稳定性高。
- 典型应用：计数器、定时器、数据存储器、数字通信中的信号同步电路。


## 六、时序逻辑电路
- 同步：
- 异步：
TTL标准电源电压为5V
自启动能力：电路在通电后，即便初始状态处于无效状态（非设计预期的工作状态），也能在时钟或输入信号的作用下，自动进入有效工作状态循环的能力。

- 160/162:偶数，十进制计数器
- 161/163:奇数，二/十六进制计数器

两片3/8译码器组合为4/16译码器
一个芯片工作，通过使能端禁止另一个芯片工作，一个连接高电平有效，一个低电平有效


数据选择器/多路开关
数据输入端 相当于电视频道 D表示
数据选择（输入）端 相当于遥控器 A表示
数据输出端

74LS85芯片
比较器
