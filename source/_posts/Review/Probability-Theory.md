---
title: Probability Theory
date: 2025-09-04 12:54:45
tags:
archive: true
---

## 随机事件及其概率
因为$A\overline{B} = A - B$，所以在概率计算中，有 $P(A\overline{B}) = P(A - B)$。结合概率的性质 $P(A - B) = P(A) - P(AB)$，因此有：

$$P(A\overline{B}) = P(A） - P（AB)$$

条件概率：$P(A \mid B) = \frac{P(AB)}{P(B)}$

通常反向利用：$P(A \mid B) {P(B)} = {P(AB)}$

## 随机变量及其分布
二项分布：
$$P{X=K}=C^k_n p^k(1-p)^{n-k}$$
泊松分布：
$$P{X=k}=\frac{\lambda^k}{k!}e^{-\lambda}$$
当二项分布的n很大，p很小，且$\lambda=np$时，二项分布可近似为泊松分布。
随机变量的分布函数
用于研究某一变量落某个区间内的概率
$$F(x)=P{X\leq x}$$
满足：
分布函数有界性（正无穷、负无穷极限）
$$\lim_{x \to +\infty} F(x) = 1, \quad \lim_{x \to -\infty} F(x) = 0$$