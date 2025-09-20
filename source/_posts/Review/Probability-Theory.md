---
title: Probability Theory
date: 2025-09-04 12:54:45
tags:
archive: true
math: true
category: Review
category_bar: true
---

## 随机事件及其概率
因为$A\overline{B} = A - B$，所以在概率计算中，有 $P(A\overline{B}) = P(A - B)$。结合概率的性质 $P(A - B) = P(A) - P(AB)$，因此有：

$$P(A\overline{B}) = P(A） - P（AB)$$

条件概率：$P(A \mid B) = \frac{P(AB)}{P(B)}$

通常反向利用：$P(A \mid B) {P(B)} = {P(AB)}$

## 随机变量及其分布
### 二项分布：

$$P(X = k) = C^{k}_{n} p^k (1-p)^{n-k}$$

记为：$X \sim B(n, p)$
### 泊松分布：

$$P(X = k) = \frac{\lambda^k}{k!} \mathrm{e}^{-\lambda}$$

记为：$X \sim\pi(\lambda)$

当二项分布的**n很大**，**p很小**，且$\lambda=np$时，**二项分布可近似为泊松分布**。

### 随机变量的分布函数

用于研究某一变量落某个区间内的概率
$$F(x)=P{(X\leq x)}$$

满足分布函数有界性（正无穷、负无穷极限）
$$\lim_{x \to +\infty} F(x) = 1, \quad \lim_{x \to -\infty} F(x) = 0$$

- **分布律**（**仅用于离散型随机变量**）：取**某一个具体值**的概率
- **概率密度函数**（**仅用于连续型随机变量**）：随机变量的**概率密度分布**
- **分布函数**（均适用）：随机变量**小于等于某一值x的概率**


$$F(x)=\int_{-\infty}^{x}f(t)dt$$

{%note danger%}
- **离散型随机变量**有阶梯（注意边界等号）
- **连续型随机变量**取某一指定值的概率为0（只能为范围）
{%endnote%}

### 指数分布
其概率密度函数：

$$f(x) = 0 \quad (x < 0)$$ $$f(x) = \lambda e^{-\lambda x} \quad (x \geq 0)$$

其分布函数：
$$F(x) = P(X \leq x) = 0 \quad (x < 0)$$ $$F(x) = P(X \leq x) = 1 - e^{-\lambda x} \quad (x \geq 0)$$
分布具有**无记忆性**


### 均匀分布
其概率密度函数：
$$f(x) = \frac{1}{b-a} \quad (a \leq x \leq b)$$ $$f(x) = 0 \quad (\text{其他情况})$$

其分布函数：
$$F(x) = P(X \leq x) = 0 \quad (x < a)$$ $$F(x) = P(X \leq x) = \frac{x-a}{b-a} \quad (a \leq x \leq b)$$ $$F(x) = P(X \leq x) = 1 \quad (x > b)$$

**落在子区间的概率与子区间的长度成正比**，与区间位置无关

### 正态分布
$$ f(x) = \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{ (x-\mu)^2} {2\sigma^2} }$$

其中：
- $x$ 是随机变量的取值，范围为 $(-\infty, +\infty)$
- $\mu$ 是分布的**均值** ，决定了曲线的**中心位置**
- $\sigma$ 是分布的**标准差**，决定了曲线的"胖瘦"或**离散程度**（越小越**集中**于$\mu$）

当 $\mu = 0, \sigma = 1$ 时，称为**标准正态分布**，记为$X\sim N(0,1)$，其概率密度记$\varphi(x)$，为分布函数记为$\Phi(x)$

由$$\varphi(x)=\varphi(-x)$$

容易得到：
$$\Phi(-x)=1-\Phi(x)$$

对于一般的正态分布，我们可以通过线性变换将其转换为标准正态分布：
$$Y = \frac{X - \mu}{\sigma}$$

那么对于任意区间$[a, b]$，有：
$$P(a \leq X \leq b) = P(\frac{a - \mu}{\sigma} \leq Y \leq \frac{b - \mu}{\sigma}) = \Phi(\frac{b - \mu}{\sigma}) - \Phi(\frac{a - \mu}{\sigma})$$

