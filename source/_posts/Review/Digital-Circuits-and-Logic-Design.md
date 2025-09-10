---
title: Digital Circuits and Logic Design
date: 2025-09-04 12:58:09
tags:
archive: true
---


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
