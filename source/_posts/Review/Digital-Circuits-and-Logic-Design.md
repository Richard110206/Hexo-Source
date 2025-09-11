---
title: Digital Circuits and Logic Design
date: 2025-09-04 12:58:09
tags:
archive: true
---

## 数制与编码
### 整数十进制转换：除X倒取余法
十进制整数 N 可表示为 X 进制的幂次和形式：
$$N = a_{n} \times X^n + a_{n-1} \times X^{n-1} + ... + a_{1} \times X^1 + a_{0} \times X^0$$通过 “除 X 取余”，可依次求出最低位，再 “倒取余” 得到最终结果。
### 小数十进制转换：乘X顺取整法
十进制小数 M（0<M<1）可表示为 X 进制的负幂次和形式：
$$M = b_{-1} \times X^{-1} + b_{-2} \times X^{-2} + ... + b_{-k} \times X^{-k} + ...$$通过 “乘 X 取整”，可依次求出小数点后第一位，再 “顺取整” 得到 X 进制小数。
### 原码
在最高位补0或1表示正负，0为正，1为负。
### 反码
- 对于正数，反码与原码相同
- 对于负数，反码为原码**除符号位外按位取反**
{%note info%}
在反码表示法中，0的表示方法不是唯一的
{%endnote%}
反码表示法的优点是在进行加减运算时**不需要判断两数的符号是否相同**，只要先求出两数的反码然后相加即可（**符号位也参与运算**，当符号位产生进位时，需要**循环进位**，即把符号位的进位加到和的最低位上去），比原码运算要简单的多。
### 补码
- 对于正数，补码与原码相同
- 对于负数，补码为原码**除符号位外按位取反**，再在**最低位加一**（**补码+1**）
{%note info%}
补码的补码是原码
{%endnote%}
补码进行运算时，符号位参与运算，而且**符号位的进位会丢弃**（区别于反码）

## 逻辑代数基础
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

- 真值表:arrow_forward::arrow_forward:逻辑式

1. 从真值表中**找出使得F=1的那些变量取值**
2. 把每一组变量取值写成对应的乘积项, **0 --> 1，1不变**
3. 将**乘积相加**得到逻辑式

### 波形图
1. 输入波形要**穷举所用可能的输入**
2. 输出波形与输入波形**一一对应**


### 反演规则
求反函数
1. **长非号不变**
2. 运算的**优先顺序不变**（**要加括号**）

- 摩根定律
$$\overline{A + B} = \overline{A} \cdot \overline{B}$$

$$\overline{A \cdot B} = \overline{A} + \overline{B}$$

- 吸收律
$$A + A \cdot B = A$$


$$A \cdot (A + B) = A$$


$$A + \overline{A} \cdot B = A + B$$


$$A \cdot (\overline{A} + B) = A \cdot B$$

- 包含律
$$A \cdot B + \overline{A} \cdot C + B \cdot C = A \cdot B + \overline{A} \cdot C$$

$$(A + B) \cdot (\overline{A} + C) \cdot (B + C) = (A + B) \cdot (\overline{A} + C)$$

- 尾部变换
$$A \cdot \overline{B} = A \cdot \overline{A \cdot B}$$

$$A \cdot \overline{A \cdot B} = A \cdot (\overline{A} + \overline{B}) = A \cdot \overline{A} + A \cdot \overline{B} = A \cdot \overline{B}$$

常用的化简方法

1. 并项法$$AB+A\overline{B}=A$$
2. 吸收法$$A+AB=A$$
3. 消去法$$A+\overline{A}B=A+B$$

$m_0$下标编号规则：原变量取1，反变量取0

n变量的最小项有n个相邻项：
相邻项：只有一个变量不同，一对相邻项可以消去一个变量

### 卡诺图
卡诺图表示按照格雷码（任意两个相邻的编码，仅有 1 位二进制数不同）进行标识，使得相邻变量的组合中只有一个变量不同。
卡诺图排列顺序：00->01->11->10
#### 卡诺图的绘制：
- 最小项在方格中填1
- 最大项在方格中填0
#### 卡诺图的化简
卡诺圈内小方格数N必须是$2^n$，相邻两个小方格中只有一个变量不同，可以合并为一项，消去一个互非的变量。
1.	卡诺圈越大越好
2.	卡诺圈越少越好
3.	每一个卡诺圈都要有新的成分
4.	先圈大圈，后圈小圈
