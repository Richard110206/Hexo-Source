---
title: Probability Theory
date: 2025-09-04 12:54:45
tags:
archive: true
math: true
---

## 随机事件及其概率
因为$A\overline{B} = A - B$，所以在概率计算中，有 $P(A\overline{B}) = P(A - B)$。结合概率的性质 $P(A - B) = P(A) - P(AB)$，因此有：

$$P(A\overline{B}) = P(A） - P（AB)$$

条件概率：$P(A \mid B) = \frac{P(AB)}{P(B)}$

通常反向利用：$P(A \mid B) {P(B)} = {P(AB)}$

## 随机变量及其分布
二项分布：

$$P(X = k) = C^{k}_{n} p^k (1-p)^{n-k}$$

泊松分布：

$$P(X = k) = \frac{\lambda^k}{k!} \mathrm{e}^{-\lambda}$$

当二项分布的n很大，p很小，且$\lambda=np$时，二项分布可近似为泊松分布。

随机变量的分布函数

用于研究某一变量落某个区间内的概率
$$F(x)=P{X\leq x}$$
满足：
分布函数有界性（正无穷、负无穷极限）
$$\lim_{x \to +\infty} F(x) = 1, \quad \lim_{x \to -\infty} F(x) = 0$$

- 分布律（**仅用于离散型随机变量**）：取**某一个具体值**的概率
- 概率密度函数（**仅用于连续型随机变量**）：随机变量的**概率密度分布**
- 分布函数（均适用）：随机变量**小于等于某一值x的概率**


$$F(x)=\int_{-\infty}^{x}f(t)dt$$

{%note danger%}
- 离散型随机变量有阶梯（注意边界等号）
- 连续型随机变量取某一指定值的概率为0（只能为范围）
{%endnote%}

### 指数分布
其概率密度函数：
$$f(x) =
\begin{cases}
\lambda e^{-\lambda x} & ,\ x \geq 0 \
0 & ,\ x < 0
\end{cases} $$
其分布函数：
$$F(x) = P(X \leq x) =
\begin{cases}
0 & ,\ x < 0 \
1 - e^{-\lambda x} & ,\ x \geq 0
\end{cases}$$
分布具有无记忆性

### 均匀分布
其概率密度函数：
$$f(x) =
\begin{cases}
\frac{1}{b-a} & ,\ a \leq x \leq b \
0 & ,\ \text{其他}
\end{cases}$$

其分布函数：
$$
F(x) = P(X \leq x) =
\begin{cases}
0 & ,\ x < a \
\frac{x-a}{b-a} & ,\ a \leq x \leq b \
1 & ,\ x > b
\end{cases}$$
落在子区间的概率与子区间的长度成正比，与区间位置无关

### 正态分布