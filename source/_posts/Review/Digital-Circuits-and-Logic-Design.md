---
title: Digital Circuits and Logic Design
date: 2025-09-04 12:58:09
tags:
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/Digital%20Circuits%20and%20Logic%20Design.png?raw=true
math: true
category: Review
category_bar: true
description: A comprehensive guide covering fundamental concepts from number systems and Boolean algebra to sequential logic circuits and AHDL progRAMming. 
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
补码进行运算时，符号位参与运算，但是**符号位的进位会丢弃**（区别于反码）

#### 校验码
- 奇校验码：“1”的个数为奇数
- 偶校验码：“1”的个数为偶数
- 
奇偶校验码：在信息最后加一个校验码。如果信息有奇数个1，则奇校验码为0，偶校验码为1

#### BCD 码
二进制编码十进制的总称，涵盖所有用二进制表示十进制（0-9）的编码方案。

- **8421**：按 “8、4、2、1” 的位权分配编码，直接对应十进制数的二进制转换（0000-1001）
- **2421**：按 “2、4、2、1” 的位权分配编码（0000-0100、1011-1111）
- **5421**：按“5、4、2、1” 的位权分配编码（0000-0100、1000-1100）
- **余三码**：基于 8421 码衍生，每个编码比对应 8421 码多 3（二进制 0011），编码范围是 0011-1100 

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
根据**输出波形为真值**的输入组合，写出逻辑式
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

{%note info%}
- n变量的最小项有n个相邻项
- 相邻项：只有一个变量不同，**一对相邻项可以消去一个变量**
{%endnote%}

### 卡诺图
&emsp;&emsp;卡诺图表示按照**格雷码**（任意两个相邻的编码，仅有 1 位二进制数不同）进行标识，使得相邻变量的组合中只有一个变量不同。卡诺图**排列顺序**：00->01->11->10

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

{%note info%}
多变量的时候使用**降维法**
{%endnote%}

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

### CMOS集成
- 传输门
- 三态门
  - $\overline{EN}=1$时，$Y=Z$，输出为高阻态
  - $\overline{EN}=0$时，$Y=\overline{A}$
- 漏极开路门（OD门）
  - 可以**实现线与**的功能
  - 工作时必须**外接电源和电阻**

### TTL
- OC门
- 三态门

## 四、组合逻辑电路

### 编码器
编码器是用二进制码来表示每个给定的信息符号。（用二进制表示文字符号的过程叫做编码）

#### 抢答器
**普通编码器**：任何时刻只有一个输入信号（八线-三线编码器）
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

{%fold into@如何寻找芯片的 1 引脚？%}
将芯片**缺口放在左边**，左下角第一个为 1 号引脚，**逆时针方向旋转**计数引脚依次增大
{%endfold%}

有选通输入端 $S'$、选通输出端$Y_{s}'$，只有当选通输入端为低电平（接地），芯片才正常导通，否则输出端都是高电平（无效）；选通输出端**控制下一级电路是否工作**（一般当前有输入时选通输出端 $Y_{s}'$ 输出 1，禁止下一级电路工作）

{%fold into@案例%}
用两个8线-3线优先编码器接成16线-4线优先编码器
{%endfold%}


### 译码器 
输入$n$，输出$2^n$
#### 二 - 十译码器
输入4位，输出10位（0000-1001），其余输出为无效（1010-1111）

#### 三 - 八译码器
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


{%note info%}
用 72HC138译码器实现逻辑函数：$L=AB+BC+AC$
1. 使能端$S1=1$，$S2=S3=0$
2. 输入端$A,B,C$对应$A_2$、$A_1$、$A_0$
3. 将函数转化为最小项的形式
4. 两次取非，最小项下标对应输出端序号
{%endnote%}

{%note info%}
用3-8线译码器74HC138可以构成6-64线译码器，需要（9）片74HC138。
- 高位需要 1 片 74HC138：专门处理高 3 位地址，输出 8 个信号控制 8 片低位芯片的使能端；
- 低位需要 8 片 74HC138：每片对应高 3 位的 1 种组合，每片提供 8 个输出，8×8=64 个总输出；
{%endnote%}


#### 七段字符显示器
输出到7位数码管上，四位二进制数（0000-1001）
| 输入（$Q_3Q_2Q_1Q_0$） | 输出（a b c d e f g） | 显示数码 |
|:-----------------------:|:-----------------------:|:----------:|
| 0 0 0 0               | 1 1 1 1 1 1 0         | 0        |
| 0 0 0 1               | 0 1 1 0 0 0 0         | 1        |
| 0 0 1 0               | 1 1 0 1 1 0 1         | 2        |
| 0 0 1 1               | 1 1 1 1 0 0 1         | 3        |
| 0 1 0 0               | 0 1 1 0 0 1 1         | 4        |
| 0 1 0 1               | 1 0 1 1 0 1 1         | 5        |
| 0 1 1 0               | 1 0 1 1 1 1 1         | 6        |
| 0 1 1 1               | 1 1 1 0 0 0 0         | 7        |
| 1 0 0 0               | 1 1 1 1 1 1 1         | 8        |
| 1 0 0 1               | 1 1 1 1 0 1 1         | 9        |
#### 数据分配器
#### 数据选择器
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

{%note info%}
74LS151（8选1数据选择器）实现4变量逻辑函数$L = \sum m(0,3,5,8,13,15)$（4变量：A、B、C、D）
1. 地址端匹配
   - 74LS151有3个地址端（$A_2$、$A_1$、$A_0$），从4个变量中选3个作为地址端（通常选高位A、B、C），对应关系：$A \rightarrow A_2$、$B \rightarrow A_1$、$C \rightarrow A_0$，剩余变量D作为数据输入控制变量。
2. 按地址变量展开逻辑函数
   按A、B、C的8种组合（$m_0 \sim m_7$）拆分原函数，确定每个地址对应的数据端$D_0 \sim D_7$取值：
   - $m_0$（$A'B'C'$）：对应原函数$m_0$（$A'B'C'D'$）→ $D_0 = \overline{D}$
   - $m_1$（$A'B'C$）：原函数无此最小项 → $D_1 = 0$
   - $m_2$（$A'BC'$）：原函数无此最小项 → $D_2 = 0$
   - $m_3$（$A'BC$）：对应原函数$m_3$（$A'BCD$）→ $D_3 = D$
   - $m_4$（$AB'C'$）：对应原函数$m_8$（$AB'C'D'$）→ $D_4 = \overline{D}$
   - $m_5$（$AB'C$）：对应原函数$m_5$（$AB'CD$）→ $D_5 = D$
   - $m_6$（$ABC'$）：对应原函数$m_{13}$（$ABC'D$）→ $D_6 = D$
   - $m_7$（$ABC$）：对应原函数$m_{15}$（$ABCD$）→ $D_7 = D$
3. 电路连接
   - 使能端$\overline{S}$$接低电平（有效）；
   - 地址端：$A \rightarrow A_2$、$B \rightarrow A_1$、$C \rightarrow A_0$；
   - 数据端：按上述结果接$D/\overline{D}/0$；
   - 输出端$Y$即为逻辑函数$L$。
{%endnote%}
选3个变量作为地址端→按地址拆分4变量函数→确定各数据端取值→完成电路连接。

{%note info%}
- 译码器是**数据分配器**的 “直接实现方案”，无需额外器件。
- 译码器是**数据选择器**的 “部分组成器件”，需搭配组合门电路才能完成功能。
{%endnote%}

#### 三人表决器
- 分类：
  - 一般问题：多数同意为通过
  - 重要问题：全部同意为通过
- 变量：
  - 输入变量：P(1号)、 Q（2号）、R（3号）、T（问题类型）
  - 输出变量：Z（表决结果）
- 电路：
  - 数据选择器：多输入，单输出
  - 数据分配器：单输入，多输出

二者功能相反，互为可逆过程

### 数值比较器
- 一位比较器
  - 输入变量：$A$,$B$
  - 输出变量：$Y_{A>B}$、$Y_{A=B}$、$Y_{A<B}$
- 多位比较器：从高位比起，相等时向后进行比较

![用两片74HC85组成一个八位数值比较器](https://github.com/Richard110206/Blog-image/blob/main/article/Review/DigitalCircuits/compare.png?raw=true)

{%note info%}
用数值比较器 74LS85 设计一个**余3码有效监测电路**（当输入为余3码时，输出为1，否则为0）：用两个74LS85，分别控制上下限范围`0011~1100`
{%endnote%}


### 加法器
- 半加器：两个一位二进制数相加，不考虑进位
- 全加器：两个一位二进制数相加，**考虑进位**
  - 输入变量：A（加数）,B（加数）,CI（从低位的进位输入）
  - 输出变量：S（和）, CO（向高位的进位输出）

{%note info%}
**两个半加器和一个或门**可以构成**全加器**
{%endnote%}
#### 多位串行加法器
依次将进位加法器的进位输出端CO接到高位全加器的进位输出端CI就可以构成多位**串行**加法器。
![多位串行加法器示意图](https://github.com/Richard110206/Blog-image/blob/main/article/Review/DigitalCircuits/adder.png?raw=true)
- 优点：计算结构较为简单
- 缺点：低位运算结束产生进位后，高位才能开始全加运算，运算速度较慢。 


#### 超前进位加法器
通过逻辑电路事先知道每一位的进位输出信号，而无需从最低位开始向高位逐位传递进位信号。
- 优点：每一位的进位输入基本上同时知道，运算速度快。
- 缺点：计算结构较为复杂，随着数位增加，电路复杂程度急剧上升。

{%note danger%}
虽然是加法器，但是可以通过 “**补码**运算” **实现减法**的功能
{%endnote%}

### 竞争与险象
- 竞争：信号经不同路径到达同一点的时间差
- 险象：因信号竞争导致逻辑门输出端出现短暂、非预期的错误电平
#### 检查竞争—冒险
只要输出端的逻辑函数在一定条件下能简化成$ Y = A + A'$  或  $Y = A \cdot A' $，则可出现竞争—冒险现象。（卡诺图中，若两个相邻最小项分属不同包围圈，是否相切）

#### 消除竞争-冒险
**“正负相对，余全完”**：对于上式逻辑函数，通过添加冗余项 $ BC $，将函数修正为：$ Y = AB + A'C + BC $，当 $ B = C = 1 $ 时，$ BC = 1 $，因此 $ Y = 1 + 1 = 1 $（稳态值），消除了 $ A + A' $ 形式的竞争-冒险。

## 五、集成触发器
[时序逻辑电路总结【一】触发器](https://blog.csdn.net/m0_63028174/article/details/128995698?ops_request_misc=%257B%2522request%255Fid%2522%253A%252241ccdfc61df19f77f26d4cdb84276985%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=41ccdfc61df19f77f26d4cdb84276985&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-128995698-null-null.142^v102^pc_search_result_base2&utm_term=RS%E8%A7%A6%E5%8F%91%E5%99%A8&spm=1018.2226.3001.4187)

触发器：具有记忆功能（引入了**反馈机制**）的基本逻辑单元，输出状态**不止与现时的的输入**有关，还**与原来的输入**有关。
- 有外触发器：状态改变
- 触发信号撤除：维持状态不变

### (I) 电平触发器
#### RS触发器
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

{%note info%}
$R$为置0端，$S$为置1端，无论是或非还是与非触发器，都是谁有效，就实现谁的功能！
分析波形图时可以理解为相同就保持，不同就取反！优先级最高！参考资料：[13RS锁存器](https://www.bilibili.com/video/BV1qt4y1J7L6?spm_id_from=333.788.videopod.episodes&vd_source=54c2981c1a7a8e0433b7d23096150b7a&p=13)
{%endnote%}

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

#### D触发器
- 当$CP=0$时：时钟无效，触发器保持原有状态
- 当$CP=1$时：时钟有效，有$Q^{n+1} = D$
#### JK触发器
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

{%note info%}
- **00保持，11翻转**
- **相异跟随$J$**
{%endnote%}  

根据特性表推导，钟控JK触发器（高电平有效）的次态表达式为：  $Q^{n+1} = J\overline{Qⁿ} + \overline{K}Qⁿ$（$CP=1$时有效，$CP=0$时$Q^{n+1}=Qⁿ$），该表达式无约束条件，因反馈机制已排除不定态。

#### T' 触发器
**只有翻转功能**的触发器，每来一次脉冲改变一次状态，又称为**翻转**触发器和**计数**触发器。


#### T 触发器
同时具有**保持**和**翻转**功能的触发器。

次态表达式：$Q^{n+1} = Q^n \oplus T$

{%note info%}
- $T=0$ 时保持当前状态
- $T=1$ 时状态翻转
{%endnote%}
类比 JK 触发器，使得$J=K=T$，就可以将 JK 触发器转换为T 触发器。

{%note danger%}
T 触发器通常不单独生产芯片，而是由其他触发器转换而来
{%endnote%}


### (II) 边沿触发器
边沿触发器是对钟控触发器的进一步优化，**仅在时钟信号（CP）的上升沿（↑）或下降沿（↓）瞬间触发**，CP电平稳定期间（高/低电平）输入信号变化不影响输出，彻底解决了“空翻”问题。
{%note info%}
- **图中有尖尖三角形代表边沿触发的方式**
- **没有圈圈代表上升沿触发**
{%endnote%}
{%note danger%}
做题时现在波形图上将**上升下降沿标注**出来
{%endnote%}
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

### (III) 脉冲（主从）触发器
主触发器仅负责 “接收” 输入信号，在时钟有效期间存储信号状态，但不直接对外输出；整个主从触发器的最终输出状态，是从触发器当前的状态。

#### 主从RS触发器
触发方式：时钟信号 CP 的下降沿触发，新状态由 CLK 脉冲下降沿到来前的$R$、$S$决定（就是CLK=1时的$R$、$S$决定）
{%note info%}
出现`「`表示主从方式触发（其余与RS触发器完全相同）
{%endnote%}

#### 主从JK触发器
{%note info%}
- **没有圈圈代表下降沿触发**（其实是主触发器上升沿触发，但输出的是从触发器是脉冲（下降沿）触发）
{%endnote%}

### 多个触发器组合

{%fold into@由JK触发器转换为RS触发器（特性方程法）%}
**1. 明确特性方程：**
- 目标RS触发器：特性方程为 $Q^* = S + \overline{R}Q$，约束条件 $SR = 0$（$Q^*$为次态，$Q$为现态）；
- 原始JK触发器：特性方程为 $Q^* = J\overline{Q} + \overline{K}Q$（无不定态）。

**2. 对RS触发器方程配项变形：**
利用逻辑恒等式互补律，对RS方程的$S$项配项：$Q^* = S(Q + \overline{Q}) + \overline{R}Q$，展开后拆分两项：$Q^* = SQ + S\overline{Q} + \overline{R}Q$，结合RS触发器约束条件 $SR=0$（即$S=0$或$R=0$），分析$SQ$项：
- 当$S=1$时，$R=0$，则$\overline{R}=1$，$SQ = 1\cdot Q = \overline{R}Q$；
- 当$S=0$时，$SQ = 0\cdot Q = 0$，无影响；
因此$SQ + \overline{R}Q = \overline{R}Q$，最终简化为：$Q^{*} = S\overline{Q} + \overline{R}Q$

**3. 验证等价性：**
将变形后的RS方程 $Q^* = S\overline{Q} + \overline{R}Q$ 与JK方程 $Q^* = J\overline{Q} + \overline{K}Q$ 逐项对比：
- 对应 $\overline{Q}$ 项：$J\overline{Q} = S\overline{Q}$ → $J = S$；
- 对应 $Q$ 项：$\overline{K}Q = \overline{R}Q$ → $\overline{K} = \overline{R}$ → $K = R$。

最终转换关系：输入端口关联：$J = S$，$K = R$
{%endfold%}

| 触发器类型 | 特性方程（次态 $Q^{*}$） | 约束/说明 |
|:------------:|:--------------------------:|:-----------:|
| RS触发器   | $Q^{*} = S + \overline{R}Q$ | $SR = 0$（S=1且R=1时不定态） |
| T触发器    | $Q^{*} = T\overline{Q} + \overline{T}Q$ | T=0时保持，T=1时翻转 |
| JK触发器   | $Q^{*} = J\overline{Q} + \overline{K}Q$ | 无约束 |
| D触发器    | $Q^{*} = D$ | 无约束 |
## 六、时序逻辑电路
- 同步：所有触发器**共用一个CP**，更新状态时**所有触发器同时翻转**
- 异步：所有触发器没有共用一个 CP，有CP1,CP2

#### 分析时序逻辑电路
- **米里型**时序电路的输出是 “状态 + **输入**” 的函数
- **摩尔型**仅与状态有关

1. 写方程
   - 时钟方程（异步）
   - 驱动方程（JK触发器对应的值）
   - 状态方程（代入特性方程）
   - 输出方程（输出值）
2. 列状态
   - 状态表：现态、次态（看题目是否有初态，无则从000开始）
   - 状态转换图：有效状态和有效循环（被利用的状态）（注意还有无效状态）
   - 时序图：可以直接根据状态转换图进行绘制（注意时钟触发方式）
3. 说功能
   - 功能
   - 是否自启：电路在通电后，即便初始状态处于无效状态（非设计预期的工作状态），也能在时钟或输入信号的作用下，自动进入有效工作状态

#### 设计时序逻辑电路
1. 根据**状态图**写出**状态转移真值表**
2. **状态化简**（用卡诺图）（无效状态可以当0也可以当1）（若是异步需要看时钟方程，只考虑时钟有效的地方，即触发时刻的次态数据）    
3. 导出**激励方程**
4. 选触发器
5. 画电路图

案例介绍：[利用JK触发器和门电路设计计数器](https://www.bilibili.com/video/BV1Hz421879K/?spm_id_from=333.337.search-card.all.click&vd_source=54c2981c1a7a8e0433b7d23096150b7a)


### 寄存器
- 单拍：接受指令即可完成贮存
- 双拍：需要清零和接收两步完成，多用 RS 触发器

#### 移位寄存器
在指令（CP）下，触发器状态可向左右相邻的触发器传递

- **单向寄存器**：使用 D 触发器实现（类似击鼓传花）
- **双向移位寄存器**：单向移位寄存器+选通门，通过控制信号确定左移还是右移

#### 应用
- **环形计数器**：在单向移位寄存器的基础上将$D_0=Q_3$，实现循环4位环形计数器有效循环：🔄 1000 → 0100 → 0010 → 0001 → 1000 🔄（若初始状态为1000）
    - 缺点：状态利用率低
- **扭环形计数器**：在单向移位寄存器的基础上将$D_0=\overline{Q_3}$，实现循环计数：🔄 0000 → 1000 → 1100 → 1110 → 1111 → 0111 → 0011 → 0001 → 0000 🔄


### 计数器
- 74LS160/74LS162:偶数，十进制计数器
- 74LS161/74LS163:奇数，二/十六进制计数器

置数方式相同；前者异步清零，后者同步清零

|计数方式|不同点|
|:-----:|:-----:|
|同步清零|计数到$S_{n-1}$时，清零|
|异步清零|计数到$S_{n}$时，清零|

#### 复位法
从$S_0$一直到$S_{M-1}$，遇到$S_{M-1}$后，下一步归零。适合有复位（清零）输入端的计数器。

#### 置数法
1. 把节点送到置数（置位）端（$LD$）：
   - 要取前$M$个状态，把$S_{M-1}$送进去；
   - 要取后$M$个状态，把$S_{N-M}$送进去。
2. 取$S_i$开始的$M$个状态，把$S_{i+M-1}$送进去，预置数据输入端输入$S_i$。

#### 多片计数器级联
- **串行进位**：以低位片的**进位输出**信号作为高位片的**时钟输入**信号，两片始终处于计数状态

- **并行进位**：以低位片的**进位输出**信号作为高位片的**使能端**信号，两片的clk同时接计数输入信号

#### 计数容量M大于芯片容量N的计数器
- $M=M1·M2$：串行进位，并行进位，
- $M$是素数：整体清零，整体置数。

## 七、脉冲波形的产生与整形
### 五五五定时器
555定时器因内部包含三个阻值均为5kΩ的电阻（该电阻网络一端接电源$V_{CC}$、另一端接地，三个电阻串联构成分压电路）而得名。

该电阻网络对$V_{CC}$进行分压后，可提供两个基准电压：上方两个电阻的连接点（对应阈值端$TH$的比较参考点）电压为$\frac{2}{3}V_{CC}$，下方两个电阻的连接点（对应触发端$TR$的比较参考点）电压为$\frac{1}{3}V_{CC}$。

555定时器内部包含两个电压比较器：
- 阈值端$TH$接入**第一个比较器的反相输入端**，当$TH$的输入电压与$\frac{2}{3}V_{CC}$比较；
- 触发端$TR$接入**第二个比较器的同相输入端**，当$TR$的输入电压与$\frac{1}{3}V_{CC}$比较。

两个比较器的输出结果，会作为内部基本RS触发器的置位/复位信号，最终实现555定时器的核心逻辑功能。


| $\overline{\text{RD}}$ | 阈值端 $TH$       | 触发端 $TR$       | $v_o$ | 晶体管 T |
|:------------------------:|:----------:|:----------:|:-------:|:----------:|
| 0  | $\phi$            | $\phi$  | 0   | 导通 |
| 1  | 大于 $2V_{cc}/3$  | 大于 $V_{cc}/3$   | 0   | 导通 |
| 1  | 小于 $2V_{cc}/3$  | 大于 $V_{cc}/3$   | 保持| 保持 |
| 1  | 小于 $2V_{cc}/3$  | 小于 $V_{cc}/3$   | 1   | 截止 |

#### 单稳态触发器
1. 电路存在一个稳态与一个暂稳态。
2. 在外来触发脉冲的作用下，电路可由稳态翻转至暂稳态。
3. 暂稳态无法长期保持，经过一定时间后电路会自动回到稳态；且暂稳态的持续时间与触发脉冲无关，仅由电路自身参数（主要是RC参数）决定。

单稳态触发器的状态通过外接电容的充放电过程切换：当电路进入暂稳态后，外接电容开始充电；当电容电压上升至$\frac{2}{3}V_{CC}$（555定时器的阈值电压）时，定时器内部的三极管会导通，此时电容通过该三极管快速放电；待电容电压回落至$\frac{1}{3}V_{CC}$以下，电路便自动恢复至初始稳态。

- **核心定义**
$ t_W $ 是单稳态触发器中外接电容电压 $ v_C $ 从 $ 0 $（暂稳态起始电压）指数上升至 $ \frac{2}{3}V_{CC} $（555定时器阈值电压）所需的时间。


- **推导过程**
基于RC电路的暂态过程公式：
$t_W = RC \ln\frac{V_C(\infty) - V_C(0_+)}{V_C(\infty) - V_C(t)}$

其中：
  - 初始电压： $ v_C(0_+) = 0 $
  - 稳态电压： $ v_C(\infty) = V_{CC} $           
  - 触发翻转电压： $ v_C(t) = \frac{2}{3}V_{CC} $

$t_W = RC \ln\frac{V_{CC} - 0}{V_{CC} - \frac{2}{3}V_{CC}} = 1.1RC$


- **关键结论**
  - $ t_W $ 仅由电路自身的RC参数决定（与触发脉冲无关）。
  - 注意：$ t_W $ 过大会降低定时的精度与稳定度。
- 用处:
  - 定时： 产生固定宽度的脉冲
  - 整形： 将不规则的转换为宽度、幅度都相等的波形
  - 延时： 将输入信号延时输出

#### 多谐振荡器
1. 电路无稳态，仅存在两个暂稳态，通过RC充放电持续交替翻转。
2. 无需外部触发脉冲，仅依靠自身RC网络的充放电实现自动振荡。
3. 振荡周期由RC参数决定，与外部信号无关。


- **工作原理**
通过外接RC充放电网络交替切换状态：
1. 初始暂稳态1：电容充电，电压从$\frac{1}{3}V_{CC}$上升至$\frac{2}{3}V_{CC}$；
2. 翻转至暂稳态2：内部三极管导通，电容放电，电压从$\frac{2}{3}V_{CC}$回落至$\frac{1}{3}V_{CC}$；
3. 再次翻转回暂稳态1，循环往复产生连续脉冲。


- **振荡周期推导**
  - 充电时间（暂稳态1时长）：$t_{W1} = 0.7(R_1+R_2)C$（电压从$\frac{1}{3}V_{CC}$→$\frac{2}{3}V_{CC}$）
  - 放电时间（暂稳态2时长）：$t_{W2} = 0.7R_2C$（电压从$\frac{2}{3}V_{CC}$→$\frac{1}{3}V_{CC}$）
  - 总周期：$T = t_{W1}+t_{W2} = 0.7(R_1+2R_2)C$

- **关键结论**
  - 输出为连续方波，占空比可通过调整$R_1$、$R_2$改变（占空比$D = \frac{R_1+R_2}{R_1+2R_2}$）。
  - 振荡频率由RC参数唯一决定：$f = \frac{1}{T} \approx \frac{1.43}{(R_1+2R_2)C}$。

#### 施密特触发器
1. 双稳态触发器，状态切换由输入电压的**阈值电平**决定。
2. 具有**滞回特性**：上升沿触发的阈值（上限阈值$V_{T+}$）与下降沿触发的阈值（下限阈值$V_{T-}$）不同。
3. 电平触发：输出电平仅由当前输入电平相对阈值的大小决定，与信号变化速率无关。
   

| 电路类型         | 参数项                | 计算公式                         |
|------------------|-----------------------|----------------------------------|
| 555单稳态电路    | $t_W$（暂稳态时间）   | $t_W = 1.1RC$                    |
| 555多谐振荡器    | $t_{W1}$（充电时间）  | $t_{W1} = 0.7(R_1+R_2)C$         |
|     | $t_{W2}$（放电时间）  | $t_{W2} = 0.7R_2C$               |
| 555施密特触发器  | $V_{T+}$（上限阈值电压） | $V_{T+} = \frac{2}{3}V_{CC}$    |
|   | $V_{T-}$（下限阈值电压） | $V_{T-} = \frac{1}{3}V_{CC}$    |
|   | $\Delta V_T$（回差电压） | $\Delta V_T = V_{T+} - V_{T-} = \frac{1}{3}V_{CC}$ |

## 九、可编程逻辑器件
HDL(Hardware Description Language)：硬件描述语言是一种用形式化方法来描述数字电路和系统的语言。
- AHDL(AlteraHardwareDescriptionLanguage)：这是Altera公司设计的一种完全集成于 MAX+PLUSⅡ/QUARTUSⅡ 开发系统中的模块化硬件描述语言 

```AHDL
SUBDESIGN 文件名
（     --子设计段
       --定义输入输出管脚及其类型
）
VARIABLE--变量段
--定义触发器、节点、状态机
BEGIN--逻辑段
--对电路的逻辑关系进行语言描述
END；--结束电路描述
```
```AHDL
SUBDESIGN name -- 这里定义设计名 name
 (
 A,B ：INPUT; --定义输入A、B
 C : OUTPUT; --定义输出C
 )
 BEGIN  -- 开始描述逻辑功能
 C=!A & B; -- 逻辑功能：C=A'B
 END 
```
```AHDL
subdesign  YMQ
(a0,a1,b,d: input;
out1,out2: output;
)
variable
temp:node;
begin
temp=a0 & !a1;
out1=temp & b; 
out2=temp & d;
end
```
1. 语言字符不区分大小写，但建议使用**大写来写关键字**
2. 同一类型多个输入用逗号`,`分隔，完整的语句用分号`;`结束，用`-`来注释一行用`% %`来注释一整段
3. 所有的程序**并发**的执行，不依赖描述的前后顺序
4. 数值不能赋值给节点，必须使用 VCC 和 GND

#### 四位二进制计数器
```AHDL
subdesign  4bcnt
(inclk:input;
out[3..0]:output;
)
variable
count[3..0]:dff; -- 定义4个DFF触发器，作为计数器的状态寄存器
begin
count[].clk=inclk;
count[].q=count[].q+1;
out[]=count[]
end;]
```
#### BCD计数器
```AHDL
subdesign  BCDCNT
(inclk :input;
out[3..0]: output;)
variable
count[3..0]:dff;
begin
count[].clk=inclk;
if count[]==9 then 
count[]=0;
else
count[]=count[]+1;
end if;
out[]=count[]
end;
```
| 数制     | 值格式                                  |
|:------------:|:-----------------------------------------:|
| 十进制   | `<数字0到9的排列>`，无需前缀             |
| 二进制   | `B“<0, 1, X的排列>”`，X表示无关项        |
| 八进制   | `Q“<数字0到7的排列>”` 或 `O“<数字0到7的排列>”` |
| 十六进制 | `H“<数字0到9, A到F的排列>”` 或 `X“<数字0到9, A到F的排列>”` |

| 运算符   | 说明           | 
|:----------:|:---------------:|
| !、NOT     | 对1求补(非)    | 
| &、AND    | 与            | 
| !&、NAND  | 与非          | 
| !#、NOR   | 或非          |
| $、XOR    | 异或          |
| !$、NXOR | 异或非(同或)  |
| #、OR     | 或           |

如果一个操作数是数值，而另一个是一个单独的节点或节点组，那么该数值将被截至或带符号扩展至另一节点组的长度。

{%note info%}
例如: 表达式$(a,b,c)\text{ \& }1$, 1被转化为B"001"，表达式变成$(a,b,c)\text{ \& }(0,0,1)$，最后表达式被当作
$(a\text{ \& }0，b\text{ \& }0，c\text{ \& }1)=(0,0,c)$。
{%endnote%}

{%note danger%}
  在AHDL设计中，要注意在表达式中用 **Vcc** 作操作数与用 **1** 作操作数的意义是不同的。
{%note warning%}
对于等式：$(a,b,c) \text{ \& } 1 = (0,0,c)$，而 ${(a,b,c)\text{ \& }Vcc=(a,b,c)}$，可见在左边的那个等式中，数值1被带符号扩展至另一个节点组的长度，而在右边的那个等式中,节点Vcc是被复制为另一节点组的长度,然后各表达式再进行组运算。
{%endnote%}
{%endnote%}

#### 结点（node）
变量段的结点说明语句予以定义，它可被用来记载一个中间表达式的值。

### 二、逻辑段
{%note info%}
逻辑段一定以关键字BEGIN开始，以关键字END结束。
{%endnote%}

#### 1.条件判断
```AHDL
IF <条件> THEN 
语句1；
ELSE(或ELSIF)
语句2；
END IF；
```

- **优先权编码器**
 ```AHDL
 SUBDESIGN priority
(
    low, middle, high : INPUT;
    out[1..0]         : OUTPUT;
)
BEGIN
    IF high THEN
        out[] = 3;    -- 二进制11
    ELSIF middle THEN
        out[] = 2;    -- 二进制10
    ELSIF low THEN
        out[] = 1;    -- 二进制01
    ELSE
        out[] = 0;    -- 二进制00
    END IF;
END;
```

- **十分频器设计**

{%note info%}
**分频器**：将输入信号的频率**降低到较低的频率**
- 目的：实现不同设备需要不同速度的需求：
  - CPU核心：2.0 GHz（超快）
  - 串口通信：115200 Hz（很慢）
  - LED闪烁：1 Hz（肉眼可见）
{%endnote%}

- 同步时序逻辑：所有状态变化都在时钟边沿（inclk）同步发生
- 每5个输入时钟周期，输出翻转一次，一个完整输出周期需要10个输入时钟周期
```AHDL
SUBDESIGN fp10
(
    inclk : INPUT;
    fpf   : OUTPUT;
)

VARIABLE
    a[2..0], fp : DFF;   -- 定义D触发器

BEGIN
    a[].clk = inclk;
    fp.clk = inclk;
    
    IF a[] == 4 THEN     -- 4为计数常数
        a[] = 0;
        fp = !fp;
    ELSE
        a[] = a[] + 1;
        fp = fp;
    END IF;
    
    fpf = fp;
END;
```

{%note danger%}
注意这里的`fp`是一个D触发器，是**时序逻辑电路**，不需要每次都指定状态，而下面这段代码是组合逻辑电路（未定义为D触发器），所以在条件判断之后每次都需要指定电平状态！
```AHDL
SUBDESIGN BCDCNT1
(
    inclk    : INPUT;      -- 输入时钟
    out[3..0]: OUTPUT;     -- BCD计数输出
    cy       : OUTPUT      -- 进位标志
)

VARIABLE
    count[3..0] : DFF;     -- 4位BCD计数器

BEGIN
    -- 时钟连接
    count[].clk = inclk;
    
    -- BCD计数逻辑
    IF count[] == 9 THEN
        count[] = 0;       -- 计数到9时清零
        cy = VCC;          -- 产生进位信号
    ELSE
        count[] = count[] + 1;  -- 正常计数
        cy = GND;               -- 无进位
    END IF;
    
    -- 输出连接
    out[] = count[];
END;
```
{%endnote%}

- **八位比较器**
```AHDL
SUBDESIGN cmp8
(
    a[7..0], b[7..0] : INPUT;
    dy, xy, equ : OUTPUT;
)

BEGIN
    IF (a[7..0] > b[7..0]) THEN
        dy = VCC;    -- a > b
        xy = GND;
        equ = GND;
    ELSIF (a[7..0] == b[7..0]) THEN
        dy = GND;
        xy = GND;
        equ = VCC;    -- a = b
    ELSE
        dy = GND;
        xy = VCC;    -- a < b
        equ = GND;
    END IF;
END;
 ```
 #### 2.情况判断
 ```AHDL
 CASE   组[] IS
 WHEN 值1 =>  语句1;
 WHEN 值2 =>  语句2;
 WHEN 值3  => 语句3;
 WHEN  OTHERS =>语句4;
 END CASE
 ```

- **三人表决器**
```AHDL
SUBDESIGN VOTE3
(
    in[2..0] : INPUT;
    out : OUTPUT;
)
BEGIN
    CASE in[] IS
        WHEN B"000" => out = GND;  -- 0人赞成，不通过
        WHEN B"001" => out = GND;  -- 1人赞成，不通过  
        WHEN B"010" => out = GND;  -- 1人赞成，不通过
        WHEN B"100" => out = GND;  -- 1人赞成，不通过
        WHEN OTHERS => out = VCC;  -- 其他情况（2或3人赞成）通过
    END CASE;
END;
```

#### 3.真值表语句
```AHDL
SUBDESIGN BCDYMQ
(
    in[3..0] : INPUT;
    a, b, c, d, e, f, g : OUTPUT;
)
TABLE
    i[3..0] => a, b, c, d, e, f, g;
    
    H"0" => 1, 1, 1, 1, 1, 1, 0;    -- 字符0的字型码
    H"1" => 0, 1, 1, 0, 0, 0, 0;    -- 字符1的字型码
    H"2" => 1, 1, 0, 1, 1, 0, 1;    -- 字符2的字型码
    H"3" => 1, 1, 1, 1, 0, 0, 1;    -- 字符3的字型码
    H"4" => 0, 1, 1, 0, 0, 1, 1;    -- 字符4的字型码
    H"5" => 1, 0, 1, 1, 0, 1, 1;    -- 字符5的字型码
    H"6" => 1, 0, 1, 1, 1, 1, 1;    -- 字符6的字型码
    H"7" => 1, 1, 1, 0, 0, 0, 0;    -- 字符7的字型码
    H"8" => 1, 1, 1, 1, 1, 1, 1;    -- 字符8的字型码
    H"9" => 1, 1, 1, 1, 0, 1, 1;    -- 字符9的字型码
END TABLE;
END;
```

{%note info%}
并不是所有输入值的组合形式都有必要列出，可以在与输出结果无关的输入位上写X(无关)。
{%endnote%}

```AHDL
        B"0000" => 1, 1, 1, 1, 1, 1, 0;    -- 显示 0
        B"0001" => 0, 1, 1, 0, 0, 0, 0;    -- 显示 1
        B"0010" => 1, 1, 0, 1, 1, 0, 1;    -- 显示 2
        B"0011" => 1, 1, 1, 1, 0, 0, 1;    -- 显示 3
        B"0100" => 0, 1, 1, 0, 0, 1, 1;    -- 显示 4
        B"0101" => 1, 0, 1, 1, 0, 1, 1;    -- 显示 5
        B"0110" => 1, 0, 1, 1, 1, 1, 1;    -- 显示 6
        B"0111" => 1, 1, 1, 0, 0, 0, 0;    -- 显示 7
        B"1000" => 1, 1, 1, 1, 1, 1, 1;    -- 显示 8
        B"1001" => 1, 1, 1, 1, 0, 1, 1;    -- 显示 9
```

#### 4.默认语句
```AHDL
DEFAULTS
结点或组=值；
END DEFAULT；
```
{%note info%}
1. 在逻辑段中只能有一个 Defaults 语句，并且它必须是关键字 BEGIN 后的第一个语句
2. 如果在一个Defaults语句中为一个变量多次赋值，那么只有**最后一次赋值有效**
{%endnote%}

- **defaults设计三人表决器**
```AHDL
SUBDESIGN   VOTE3
 ( in[2..0] : INPUT；
out : OUTPUT；
)
 BEGIN
 DEFAULTS
 out=Vcc；
END DEFAULTS；
13 AHDL语言及其应用
IF (in[]==0)OR(in[]==1)OR (in[]==2)OR (in[]==4) THEN
 out=GND;
 END IF;
 END;
```

#### AHDL中触发器的类别
触发器分类与功能与数字电子电路中完全一样,不需要知道触发器的内部结构和实现,只需会正确调用就可以了。

| 触发器类型 | 触发器名称 | 清除信号（低有效） | 时钟信号端口 | 输入端口 | 输出端口 |
|:------------:|:------------:|:--------------------:|:--------------:|:----------:|:----------:|
| D 触发器   | DFF        | CLRN               | CLK          | D        | Q        |
| T 触发器   | TFF        | CLRN               | CLK          | T        | Q        |
| JK 触发器  | JKFF       | CLRN               | CLK          | J/K      | Q        |
| SR 触发器  | SRFF       | CLRN               | CLK          | S/R      | Q        |

在变量段进行声明：
```AHDL
VARIABLE
寄存器名[n..0]: 触发器名； ---n为触发器的数量
```

{%note info%}
- FPGA 查找表技术
- CPLD 乘积项技术
{%endnote%}

{%fold into@试用 AHDL 设计一个优先编码器%}
设计要求：
（1）输入端用 H、M、L 表示，高电平有效；
（2）输出端要求个数最少；
（3）输入端优先级 H 最高，L 最低。

```AHDl
SUBDESIGN code_priority
(
  H, M, L : INPUT;
  Y1, Y2 : OUTPUT;
)
BEGIN
  TABLE
    H, M, L => Y1, Y2;
    1, X, X => 1, 1;
    0, 1, X => 1, 0;
    0, 0, 1 => 0, 1;
    0, 0, 0 => 0, 0;
  END TABLE;
END;
```
{%endfold%}

{%fold into@用层次化、模块化的思想设计线路图实现56进制计数功能%}
设计要求：
（1）INK 为 1KHZ 信号输入端；
（2）56进制计数器只接受占空比为50%的秒脉冲；
（3）对模块进行接线。
```AHDL
design fp1000to1(
     inclk = input;  -- 输入：1000Hz时钟（题目中的INK）
     outfp = output; -- 输出：1Hz秒脉冲（占空比50%）
)

variable
  count[8..0] : dff; -- 计数器（9位：0~511，足够计到499）
  fp : dff;          -- 输出翻转寄存器（实现50%占空比）

begin
count[].clk=inclk;
fp.clk=inclk;

 -- 计数到499时，计数器清零 + 翻转输出（实现50%占空比）
  if count[] == 499 then
    count[] = 0;
    fp = !fp;
  else
    count[] = count[] + 1; -- 未到499则继续计数
    fp = fp;
  endif;
 outfp=fp;
 ```
 ```AHDL
 design 55to0
(
  inclk = input;          -- 输入：1Hz秒脉冲（来自分频模块的outfp）
  outhigh[3..0], outlow[3..0] : output; -- 输出：高位（0~5）、低位（0~9）
)
variable
  hw[3..0] : dff; -- 高位计数器（记录“十位”，0~5）
  lw[3..0] : dff; -- 低位计数器（记录“个位”，0~9）
begin
  hw[].clk = inclk; -- 高位时钟接1Hz输入
  lw[].clk = inclk; -- 低位时钟接1Hz输入

  -- 计满55（高位5、低位5）时，高低位同时清零
  if (hw[] == 5) and (lw[] == 5) then
    hw[] = 0;
    lw[] = 0;
  -- 低位计满9时，低位清零、高位+1
  elsif lw[] == 9 then
    hw[] = hw[] + 1;
    lw[] = 0;
  -- 否则低位+1
  else
    hw[] = hw[];
    lw[] = lw[] + 1;
  endif;

  outhigh[] = hw[]; -- 输出高位（十位）
  outlow[] = lw[];  -- 输出低位（个位）
end;
```




{%endfold%}