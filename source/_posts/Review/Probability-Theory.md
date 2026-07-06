---
title: Probability Theory
date: 2025-09-04 12:54:45
tags:
archive: true
math: true
category: Review
category_bar: true
description: 概率论与数理统计期末复习笔记，涵盖随机事件与概率、随机变量与分布、数字特征、大数定律、抽样分布及参数估计等核心知识点。
---

## 第一章 随机事件及其概率

### 基本概念

- **样本空间** $\Omega$：所有可能结果的集合
- **随机事件**：样本空间的子集
- **互斥（互不相容）**：$A \cap B = \emptyset$
- **对立事件**：$\overline{A} = \Omega - A$，$P(\overline{A}) = 1 - P(A)$

### 概率的基本公式

**加法公式**：

$$P(A \cup B) = P(A) + P(B) - P(AB)$$

若 $A, B$ 互斥：$P(A \cup B) = P(A) + P(B)$

**差事件概率公式**：

$$P(A\overline{B}) = P(A) - P(AB)$$

### 条件概率与独立性

- **条件概率**：$P(A \mid B) = \dfrac{P(AB)}{P(B)}$（要求 $P(B) > 0$）
- **乘法公式**：$P(AB) = P(A \mid B) \cdot P(B) = P(B \mid A) \cdot P(A)$
- **独立性**：$P(AB) = P(A) \cdot P(B)$

### 全概率公式与贝叶斯公式

**全概率公式**：设 $B_1, B_2, \ldots, B_n$ 为样本空间的一个划分，则：

$$P(A) = \sum_{i=1}^{n} P(A \mid B_i) P(B_i)$$

**贝叶斯公式**：

$$P(B_i \mid A) = \frac{P(A \mid B_i) P(B_i)}{\sum_{j=1}^{n} P(A \mid B_j) P(B_j)}$$

{%note info%}
**例题**：某工厂有甲、乙、丙三条生产线，产量分别占总量的 25%、35%、40%，次品率分别为 5%、4%、2%。现随机取一件产品，发现是次品，求这件产品来自乙生产线的概率。

**解**：设 $A$ = "取到次品"，$B_1, B_2, B_3$ 分别为来自甲、乙、丙

由全概率公式：
$$P(A) = 0.25 \times 0.05 + 0.35 \times 0.04 + 0.40 \times 0.02 = 0.0345$$

由贝叶斯公式：
$$P(B_2 \mid A) = \frac{0.35 \times 0.04}{0.0345} = \frac{0.014}{0.0345} \approx 0.406$$
{%endnote%}

---

## 第二章 随机变量及其分布

### 常见离散型分布

#### 二项分布

$$P(X = k) = C_n^k p^k (1-p)^{n-k}, \quad k = 0, 1, 2, \ldots, n$$

记作 $X \sim B(n, p)$。参数含义：$n$ 为试验次数，$p$ 为单次试验成功概率。

- 期望：$E(X) = np$
- 方差：$D(X) = np(1-p)$

#### 泊松分布

$$P(X = k) = \frac{\lambda^k}{k!} e^{-\lambda}, \quad k = 0, 1, 2, \ldots$$

记作 $X \sim P(\lambda)$（或 $\pi(\lambda)$）。

- 期望：$E(X) = \lambda$
- 方差：$D(X) = \lambda$

{%note info%}
**泊松近似**：当二项分布满足 **$n$ 很大、$p$ 很小**，且 $\lambda = np$ 适中时，$B(n, p) \approx P(\lambda)$
{%endnote%}

### 分布函数

$$F(x) = P(X \leq x)$$

**基本性质**：
1. $0 \leq F(x) \leq 1$
2. 单调不减
3. $F(-\infty) = 0$，$F(+\infty) = 1$
4. 右连续

{%note danger%}
**注意**：可以根据这些性质**判断一个函数是否可以作为分布函数**。常见考点！
{%endnote%}

### 三类核心函数对比

| 函数类型 | 适用场景 | 描述内容 |
|:--------:|:--------:|:--------:|
| 分布律 | 仅离散型 | 随机变量取**某一具体值**的概率 |
| 概率密度函数 | 仅连续型 | 随机变量的**概率密度分布** |
| 分布函数 | 离散型+连续型 | 随机变量**小于等于 x** 的概率 |

### 连续型随机变量

分布函数与概率密度函数的关系：

$$F(x) = \int_{-\infty}^{x} f(t) \, dt$$

{%note danger%}
- 离散型分布函数为**阶梯函数**，需注意区间边界的等号
- 连续型随机变量取**单个具体值的概率为 0**，即 $P(X = a) = 0$
{%endnote%}

### 随机变量函数的分布

若 $Y = g(X)$，$X$ 的概率密度为 $f_X(x)$，$g$ 单调可导，反函数为 $X = h(Y)$，则：

$$f_Y(y) = f_X(h(y)) \cdot |h'(y)|$$

### 常见连续型分布

#### 均匀分布

$X \sim U(a, b)$

- **概率密度函数**：$f(x) = \begin{cases} \dfrac{1}{b-a}, & a \leq x \leq b \\ 0, & \text{其他} \end{cases}$

- **分布函数**：$F(x) = \begin{cases} 0, & x < a \\ \dfrac{x-a}{b-a}, & a \leq x \leq b \\ 1, & x > b \end{cases}$

- 落在子区间的概率与**子区间长度成正比**，与位置无关

{%note danger%}
- $E(X) = \dfrac{a + b}{2}$
- $D(X) = \dfrac{(b - a)^2}{12}$
{%endnote%}

#### 指数分布

- **概率密度函数**：$f(x) = \begin{cases} \lambda e^{-\lambda x}, & x \geq 0 \\ 0, & x < 0 \end{cases}$

- **分布函数**：$F(x) = \begin{cases} 1 - e^{-\lambda x}, & x \geq 0 \\ 0, & x < 0 \end{cases}$

- 核心性质：**无记忆性**，$P(X > s + t \mid X > s) = P(X > t)$

{%note danger%}
- $E(X) = \dfrac{1}{\lambda}$
- $D(X) = \dfrac{1}{\lambda^2}$
{%endnote%}

#### 正态分布

$X \sim N(\mu, \sigma^2)$

**概率密度函数**：

$$f(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x-\mu)&#94;2}{2\sigma&#94;2}}$$

- $\mu$：均值，决定曲线**中心位置**
- $\sigma$：标准差，决定曲线**离散程度**（$\sigma$ 越小越集中）

**标准正态分布**：$\mu = 0, \sigma = 1$，记作 $X \sim N(0, 1)$

- 对称性：$\varphi(x) = \varphi(-x)$，$\Phi(-x) = 1 - \Phi(x)$

**标准化变换**：若 $X \sim N(\mu, \sigma^2)$，令 $Y = \dfrac{X - \mu}{\sigma}$，则 $Y \sim N(0, 1)$

$$P(a \leq X \leq b) = \Phi\left(\frac{b - \mu}{\sigma}\right) - \Phi\left(\frac{a - \mu}{\sigma}\right)$$

{%note info%}
**正态分布的性质**：
- 若 $X \sim N(\mu_1, \sigma_1^2)$，$Y \sim N(\mu_2, \sigma_2^2)$ 且独立，则 $X \pm Y \sim N(\mu_1 \pm \mu_2, \sigma_1^2 + \sigma_2^2)$
- 有限个独立正态分布的线性组合仍为正态分布
- 用含 $\Phi$ 的表达式表示概率，计算期望时系数可消去
{%endnote%}

#### 伽马分布

$X \sim \Gamma(\alpha, \beta)$

- $E(X) = \alpha\beta$
- $D(X) = \alpha\beta^2$

---

## 第三章 多维随机变量及其分布

### 二维随机变量的联合分布

#### 连续型

**联合概率密度** $f(x, y)$：

$$P\{(X, Y) \in D\} = \iint_D f(x, y) \, dx \, dy$$

**联合分布函数**：

$$F(x, y) = \int_{-\infty}^{y} \int_{-\infty}^{x} f(u, v) \, du \, dv$$

#### 离散型

**联合分布律**：$P(X = x_i, Y = y_j) = p_{ij}$

满足：$p_{ij} \geq 0$，$\sum_i \sum_j p_{ij} = 1$

### 边缘分布

#### 连续型

- $X$ 的边缘概率密度：$f_X(x) = \int_{-\infty}^{+\infty} f(x, y) \, dy$
- $Y$ 的边缘概率密度：$f_Y(y) = \int_{-\infty}^{+\infty} f(x, y) \, dx$

#### 离散型

- $X$ 的边缘分布律：$P(X = x_i) = p_{i\cdot} = \sum_j p_{ij}$
- $Y$ 的边缘分布律：$P(Y = y_j) = p_{\cdot j} = \sum_i p_{ij}$

### 独立性

$X$ 与 $Y$ **相互独立** $\Leftrightarrow$ 联合分布等于边缘分布的乘积

- 离散型：$p_{ij} = p_{i\cdot} \cdot p_{\cdot j}$
- 连续型：$f(x, y) = f_X(x) \cdot f_Y(y)$

### 随机变量函数的分布

{%note info%}
**$Z = X + Y$ 的概率密度（卷积公式）**：

$$f_Z(z) = \int_{-\infty}^{+\infty} f_X(x) f_Y(z - x) \, dx$$

若 $X, Y$ 独立，则 $f_Z = f_X * f_Y$（卷积）
{%endnote%}

{%note info%}
**$Z = \max(X, Y)$ 和 $Z = \min(X, Y)$**（$X, Y$ 独立）：

- $F_{\max}(z) = F_X(z) \cdot F_Y(z)$
- $F_{\min}(z) = 1 - [1 - F_X(z)][1 - F_Y(z)]$
{%endnote%}

---

## 第四章 随机变量的数字特征

### 数学期望

**离散型**：$E(X) = \sum_i x_i p_i$

**连续型**：$E(X) = \int_{-\infty}^{+\infty} x f_X(x) \, dx$

**性质**：
- $E(C) = C$（$C$ 为常数）
- $E(aX + b) = aE(X) + b$
- $E(X + Y) = E(X) + E(Y)$
- 若 $X, Y$ 独立：$E(XY) = E(X)E(Y)$

**随机变量函数的期望**：

$$E[g(X)] = \int_{-\infty}^{+\infty} g(x) f_X(x) \, dx$$

$$E[g(X, Y)] = \int_{-\infty}^{+\infty} \int_{-\infty}^{+\infty} g(x, y) f(x, y) \, dx \, dy$$

{%note info%}
**做题技巧**：期望中含有复杂表达式时，将平方等展开，利用期望的线性性质逐项计算。
{%endnote%}

### 方差

$$D(X) = E\{[X - E(X)]^2\} = E(X^2) - [E(X)]^2$$

**性质**：
- $D(C) = 0$
- $D(aX + b) = a^2 D(X)$
- 若 $X, Y$ 独立：$D(X \pm Y) = D(X) + D(Y)$
- $D(X) = 0 \Leftrightarrow P(X = C) = 1$

{%note info%}
**常用计算方法**：$D(X) = E(X^2) - [E(X)]^2$

做题时先求 $E(X)$ 和 $E(X^2)$，再相减即得方差。
{%endnote%}

### 协方差与相关系数

**协方差**：

$$\text{Cov}(X, Y) = E\{[X - E(X)][Y - E(Y)]\} = E(XY) - E(X)E(Y)$$

**相关系数**：

$$\rho_{XY} = \frac{\text{Cov}(X, Y)}{\sqrt{D(X)} \cdot \sqrt{D(Y)}}$$

**性质**：
- $\text{Cov}(X, Y) = \text{Cov}(Y, X)$
- $\text{Cov}(aX, bY) = ab \cdot \text{Cov}(X, Y)$
- $\text{Cov}(X_1 + X_2, Y) = \text{Cov}(X_1, Y) + \text{Cov}(X_2, Y)$
- $D(X \pm Y) = D(X) + D(Y) \pm 2\text{Cov}(X, Y)$
- $\text{Cov}(X, X) = D(X)$

**相关系数的意义**：$\rho$ 反映 $X, Y$ 之间的**线性关系**程度：

| $\rho$ 值 | 含义 |
|:---------:|:----:|
| $\rho = 0$ | 不相关（无线性关系，但可能有非线性关系） |
| $\rho = 1$ | 完全正相关 |
| $\rho = -1$ | 完全负相关 |
| $|\rho|$ 越大 | 线性依赖关系越显著 |

{%note info%}
若已知 $\rho_{XY} = 1$，则 $Y = aX + b$。对等式两边取期望和方差：
- $E(Y) = aE(X) + b$ → 求 $b$
- $D(Y) = a^2 D(X)$ → 求 $|a|$
- 再由 $\rho = 1$ 确定 $a$ 的符号
{%endnote%}

### 常用分布的数字特征汇总

| 分布 | 记号 | $E(X)$ | $D(X)$ |
|:----:|:----:|:-------:|:-------:|
| 二项分布 | $B(n, p)$ | $np$ | $np(1-p)$ |
| 泊松分布 | $P(\lambda)$ | $\lambda$ | $\lambda$ |
| 均匀分布 | $U(a, b)$ | $\dfrac{a+b}{2}$ | $\dfrac{(b-a)^2}{12}$ |
| 指数分布 | $Exp(\lambda)$ | $\dfrac{1}{\lambda}$ | $\dfrac{1}{\lambda^2}$ |
| 正态分布 | $N(\mu, \sigma^2)$ | $\mu$ | $\sigma^2$ |
| 伽马分布 | $\Gamma(\alpha, \beta)$ | $\alpha\beta$ | $\alpha\beta^2$ |

---

## 第五章 大数定律与中心极限定理

### 切比雪夫不等式

$$P(|X - E(X)| \geq \epsilon) \leq \frac{D(X)}{\epsilon^2}$$

等价形式：

$$P(|X - E(X)| < \epsilon) \geq 1 - \frac{D(X)}{\epsilon^2}$$

{%note info%}
方差越小，取值在均值附近的概率越大，**取值越集中于均值附近**。
{%endnote%}

### 大数定律

- **切比雪夫大数定律**：独立随机变量序列的算术平均依概率收敛于期望的算术平均
- **伯努利大数定律**：频率依概率收敛于概率，即 $\dfrac{n_A}{n} \xrightarrow{P} p$
- **辛钦大数定律**：独立同分布序列的算术平均依概率收敛于期望 $\mu$

### 中心极限定理

**独立同分布中心极限定理（林德伯格-列维）**：设 $X_1, X_2, \ldots, X_n$ 独立同分布，$E(X_i) = \mu$，$D(X_i) = \sigma^2$，则当 $n$ 充分大时：

$$\frac{\sum_{i=1}^{n} X_i - n\mu}{\sigma\sqrt{n}} \xrightarrow{d} N(0, 1)$$

即 $\sum X_i$ 近似服从 $N(n\mu, n\sigma^2)$。

**棣莫弗-拉普拉斯定理**：二项分布 $B(n, p)$ 当 $n$ 很大时近似为 $N(np, np(1-p))$。

{%note info%}
**例题**：某车间有 200 台独立工作的机床，每台机床开工率为 0.6，开工时耗电 1kW。问供电至少多少 kW 才能以 99.9% 的概率保证不缺电？

**解**：设同时开工的机床数为 $X$，$X \sim B(200, 0.6)$

$n = 200$，$np = 120$，$np(1-p) = 48$

由中心极限定理：$X \approx N(120, 48)$

$$P(X \leq x) \geq 0.999 \Rightarrow \Phi\left(\frac{x - 120}{\sqrt{48}}\right) \geq 0.999$$

查表 $\Phi(3.09) \approx 0.999$，$\sqrt{48} \approx 6.93$

$x = 120 + 3.09 \times 6.93 \approx 141.4$

答：至少需要供电 142 kW。
{%endnote%}

---

## 第六章 数理统计基础

### 基本概念

- **总体**：研究对象的全体
- **样本**：从总体中抽取的 $n$ 个独立同分布的随机变量 $X_1, X_2, \ldots, X_n$
- **统计量**：样本的不含任何未知参数的函数

**常用统计量**：
- **样本均值**：$\overline{X} = \dfrac{1}{n}\sum_{i=1}^{n} X_i$
- **样本方差**：$S^2 = \dfrac{1}{n-1}\sum_{i=1}^{n} (X_i - \overline{X})^2$

{%note danger%}
**注意**：样本方差的分母是 $n - 1$，不是 $n$！因为 $E(S^2) = \sigma^2$（无偏估计）。
{%endnote%}

### 三大抽样分布

#### 卡方分布（$\chi^2$ 分布）

**构造**：若 $X_1, X_2, \ldots, X_n \sim N(0, 1)$ 且独立，则 $\chi^2 = \sum_{i=1}^{n} X_i^2 \sim \chi^2(n)$

**性质**：
- $E(\chi^2(n)) = n$
- $D(\chi^2(n)) = 2n$
- 可加性：$X \sim \chi^2(m)$，$Y \sim \chi^2(n)$ 独立 → $X + Y \sim \chi^2(m+n)$
- $\dfrac{(n-1)S&#94;2}{\sigma&#94;2} \sim \chi&#94;2(n-1)$

#### t 分布

**构造**：$X \sim N(0, 1)$，$Y \sim \chi^2(n)$ 独立 → $T = \dfrac{X}{\sqrt{Y/n}} \sim t(n)$

**性质**：
- $n > 1$ 时：$E(T) = 0$
- $n > 2$ 时：$D(T) = \dfrac{n}{n-2}$
- 对称分布，比标准正态分布更"平坦"，尾部更厚
- $n \to \infty$ 时 $t(n) \to N(0, 1)$，$n > 30$ 时近似程度高

#### F 分布

**构造**：$U \sim \chi^2(m)$，$V \sim \chi^2(n)$ 独立 → $F = \dfrac{U/m}{V/n} \sim F(m, n)$

**性质**：
- $n > 2$ 时：$E(F) = \dfrac{n}{n-2}$
- 倒数性质：$F \sim F(m, n)$ → $\dfrac{1}{F} \sim F(n, m)$
- 与 t 分布的关系：$T \sim t(n)$ → $T^2 \sim F(1, n)$

### 正态总体抽样分布

{%note info%}
设 $X_1, X_2, \ldots, X_n$ 来自 $N(\mu, \sigma^2)$，则：

1. $\overline{X} \sim N(\mu, \sigma^2/n)$
2. $\dfrac{\overline{X} - \mu}{\sigma/\sqrt{n}} \sim N(0, 1)$
3. $\dfrac{\overline{X} - \mu}{S/\sqrt{n}} \sim t(n-1)$
4. $\dfrac{(n-1)S&#94;2}{\sigma&#94;2} \sim \chi&#94;2(n-1)$
5. $\overline{X}$ 与 $S^2$ 独立
{%endnote%}

---

## 第七章 参数估计

### 矩估计

用**样本矩等于总体矩**的原则，建立方程求解未知参数。

- 用样本一阶原点矩 $\overline{X} = \dfrac{1}{n}\sum X_i$ 估计总体一阶原点矩 $E(X)$
- 用样本二阶中心矩 $\dfrac{1}{n}\sum (X_i - \overline{X})^2$ 估计总体二阶中心矩 $D(X)$

### 最大似然估计

核心目标：找到使"当前样本出现概率最大"的参数 $\hat{\theta}$。

**步骤**：
1. **构造似然函数**：$L(\theta) = \prod_{i=1}^{n} p(x_i; \theta)$
2. **取对数**：$\ln L(\theta) = \sum_{i=1}^{n} \ln p(x_i; \theta)$
3. **求导令其为零**：$\dfrac{d \ln L(\theta)}{d\theta} = 0$，解出 $\hat{\theta}$

{%note danger%}
**注意**：$\prod$ 取对数后变为 $\sum$。若含多个未知参数，则对每个参数求偏导，联立方程组。
{%endnote%}

{%note info%}
**例题**：设总体 $X \sim N(\mu, \sigma^2)$，$\mu$ 和 $\sigma^2$ 均未知，求 $\mu$ 和 $\sigma^2$ 的最大似然估计。

**解**：
似然函数：$L = \prod_{i=1}^{n} \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x_i - \mu)&#94;2}{2\sigma&#94;2}}$

取对数：$\ln L = -\frac{n}{2}\ln(2\pi) - \frac{n}{2}\ln\sigma^2 - \frac{1}{2\sigma^2}\sum(x_i - \mu)^2$

对 $\mu$ 求偏导令为零：$\hat{\mu} = \overline{X}$

对 $\sigma^2$ 求偏导令为零：$\hat{\sigma}^2 = \frac{1}{n}\sum(X_i - \overline{X})^2$
{%endnote%}

### 矩的定义汇总

| 矩 | 样本矩 | 总体矩 |
|:--:|:------:|:------:|
| 一阶原点矩 | $\overline{X} = \frac{1}{n}\sum X_i$ | $E(X)$ |
| 二阶原点矩 | $\frac{1}{n}\sum X_i^2$ | $E(X^2)$ |
| 一阶中心矩 | $\frac{1}{n}\sum (X_i - \overline{X}) = 0$ | $E[X - E(X)] = 0$ |
| 二阶中心矩 | $\frac{1}{n}\sum (X_i - \overline{X})^2$ | $D(X)$ |

### 估计量的评选标准

#### 无偏性

若 $E(\hat{\theta}) = \theta$，则 $\hat{\theta}$ 是 $\theta$ 的**无偏估计量**。

若 $\lim_{n \to \infty} E(\hat{\theta}) = \theta$，则为**渐近无偏估计量**。

#### 有效性

在无偏估计量中，**方差最小**的最有效。

若 $D(\hat{\theta}_1) < D(\hat{\theta}_2)$，则 $\hat{\theta}_1$ 比 $\hat{\theta}_2$ 更有效。

{%note info%}
**例题**：设 $X_1, X_2, X_3$ 来自总体 $X$，$D(X) = \sigma^2$。比较以下三个无偏估计量的有效性：

1. $\hat{\theta}_1 = \frac{1}{2}X_1 + \frac{1}{2}X_2$
2. $\hat{\theta}_2 = \frac{1}{3}X_1 + \frac{1}{4}X_2 + \frac{7}{12}X_3$
3. $\hat{\theta}_3 = \frac{1}{2}X_1 + \frac{1}{3}X_2 + \frac{1}{6}X_3$

**解**：由 $D(aX + bY) = a^2 D(X) + b^2 D(Y)$（独立时）

- $D(\hat{\theta}_1) = (\frac{1}{4} + \frac{1}{4})\sigma^2 = 0.5\sigma^2$
- $D(\hat{\theta}_2) = (\frac{1}{9} + \frac{1}{16} + \frac{49}{144})\sigma^2 \approx 0.507\sigma^2$
- $D(\hat{\theta}_3) = (\frac{1}{4} + \frac{1}{9} + \frac{1}{36})\sigma^2 \approx 0.389\sigma^2$

$D(\hat{\theta}_3) < D(\hat{\theta}_1) < D(\hat{\theta}_2)$，因此 $\hat{\theta}_3$ 最有效。
{%endnote%}

#### 一致性（相合性）

若 $\hat{\theta} \xrightarrow{P} \theta$（$n \to \infty$），则 $\hat{\theta}$ 是 $\theta$ 的一致估计量。

### 区间估计

置信度 $1 - \alpha$：在置信水平下，寻找区间长度最短的置信区间。

| 总体 | 待估参数 | 已知条件 | 枢轴量及分布 | 置信区间 |
|:----:|:--------:|:--------:|:------------:|:--------:|
| 单个正态 | $\mu$ | $\sigma^2$ 已知 | $\dfrac{\overline{X} - \mu}{\sigma/\sqrt{n}} \sim N(0,1)$ | $\left(\overline{X} \pm \dfrac{\sigma}{\sqrt{n}} z_{\alpha/2}\right)$ |
| 单个正态 | $\mu$ | $\sigma^2$ 未知 | $\dfrac{\overline{X} - \mu}{S/\sqrt{n}} \sim t(n-1)$ | $\left(\overline{X} \pm \dfrac{S}{\sqrt{n}} t_{\alpha/2}(n-1)\right)$ |
| 单个正态 | $\sigma^2$ | $\mu$ 未知 | $\dfrac{(n-1)S&#94;2}{\sigma&#94;2} \sim \chi&#94;2(n-1)$ | $\left(\dfrac{(n-1)S&#94;2}{\chi&#94;2_{\alpha/2}}, \dfrac{(n-1)S&#94;2}{\chi&#94;2_{1-\alpha/2}}\right)$ |
| 两个正态 | $\mu_1 - \mu_2$ | $\sigma_1^2, \sigma_2^2$ 已知 | $\dfrac{\overline{X} - \overline{Y} - (\mu_1 - \mu_2)}{\sqrt{\sigma_1^2/n_1 + \sigma_2^2/n_2}} \sim N(0,1)$ | $\left(\overline{X} - \overline{Y} \pm z_{\alpha/2}\sqrt{\dfrac{\sigma_1^2}{n_1} + \dfrac{\sigma_2^2}{n_2}}\right)$ |
| 两个正态 | $\mu_1 - \mu_2$ | $\sigma_1^2 = \sigma_2^2$ 未知 | $\dfrac{\overline{X} - \overline{Y} - (\mu_1 - \mu_2)}{S_w\sqrt{1/n_1 + 1/n_2}} \sim t(n_1+n_2-2)$ | $\left(\overline{X} - \overline{Y} \pm t_{\alpha/2} S_w\sqrt{\dfrac{1}{n_1} + \dfrac{1}{n_2}}\right)$ |
| 两个正态 | $\sigma_1^2 / \sigma_2^2$ | $\mu_1, \mu_2$ 未知 | $\dfrac{S_1&#94;2/\sigma_1&#94;2}{S_2&#94;2/\sigma_2&#94;2} \sim F(n_1-1, n_2-1)$ | $\left(\dfrac{S_1&#94;2}{S_2&#94;2} \cdot \dfrac{1}{F_{\alpha/2}}, \dfrac{S_1&#94;2}{S_2&#94;2} \cdot \dfrac{1}{F_{1-\alpha/2}}\right)$ |

其中 $S_w = \sqrt{\dfrac{(n_1-1)S_1^2 + (n_2-1)S_2^2}{n_1 + n_2 - 2}}$
