---
title: Probability Theory
date: 2025-09-04 12:54:45
tags:
archive: true
math: true
category: Review
category_bar: true
---
# 概率论与数理统计核心知识点整理

## 一、随机事件及其概率
1. **差事件概率公式**  
   因 $A\overline{B} = A - B$，结合概率性质 $P(A - B) = P(A) - P(AB)$，可得：  
   $$P(A\overline{B}) = P(A) - P(AB)$$

2. **条件概率**  
   - 定义公式：$P(A \mid B) = \frac{P(AB)}{P(B)}$（要求 $P(B) > 0$）  
   - 反向应用（求交事件概率）：$P(AB) = P(A \mid B) \cdot P(B)$


## 二、随机变量及其分布
### 2.1 常见离散型分布
#### （1）二项分布
- 概率公式：$P(X = k) = C_{n}^{k} p^k (1-p)^{n-k}$（$k = 0,1,2,\dots,n$）  
- 记作：$X \sim B(n, p)$  
- 参数含义：$n$ 为试验次数，$p$ 为单次试验成功概率

#### （2）泊松分布
- 概率公式：$P(X = k) = \frac{\lambda^k}{k!} \mathrm{e}^{-\lambda}$（$k = 0,1,2,\dots$）  
- 记作：$X \sim \pi(\lambda)$  
- 近似关系：当二项分布满足 **$n$ 很大、$p$ 很小**，且 $\lambda = np$ 时，$B(n,p) \approx \pi(\lambda)$


### 2.2 随机变量的分布函数
#### （1）定义与作用
- 定义：$F(x) = P(X \leq x)$，用于计算随机变量落在区间 $(-\infty, x]$ 内的概率  
- 核心性质（有界性）：  
  $$\lim_{x \to +\infty} F(x) = 1, \quad \lim_{x \to -\infty} F(x) = 0$$
{%note danger%}
根据这些性质**判断表达式是否可以是分布函数**
{%endnote%}
#### （2）三类核心函数对比
| 函数类型         | 适用场景       | 描述内容                     |
|------------------|----------------|------------------------------|
| 分布律           | 仅离散型       | 随机变量取**某一具体值**的概率 |
| 概率密度函数     | 仅连续型       | 随机变量的**概率密度分布**   |
| 分布函数         | 离散型+连续型  | 随机变量**小于等于x**的概率  |

{%note info%}
若已知随机变量 X 的概率密度函数为 $f_X(x)$，且 Y 与 X 满足单调可导的函数关系 $Y = g(X)$（其反函数为 $X = h(Y)$），则可通过公式法求解 Y 的概率密度函数：$f_Y(y) = f_X(h(y)) \cdot |h'(y)|$，其中 $|h'(y)|$ 为反函数 \(h(y)\) 导数的绝对值。
{%endnote%}

#### （3）连续型随机变量的特殊关系
- 分布函数与概率密度函数的积分关系：$F(x) = \int_{-\infty}^{x} f(t) dt$  
{%note danger%}
- ⚠️ **重要注意事项：** 
  - 离散型分布函数为**阶梯函数**，需注意区间边界的等号  
  - 连续型随机变量取**单个具体值的概率为0**，仅能计算区间概率
{%endnote%}

### 2.3 常见连续型分布
#### （1）指数分布
- **概率密度函数**：  
  - 当 $ x < 0 $ 时，$ f(x) = 0 $；
  - 当 $ x \geq 0 $ 时，$ f(x) = \lambda e^{-\lambda x} $。
- **分布函数**：  
  - 当 $ x < 0 $ 时，$ F(x) = P(X \leq x) = 0 $；
  - 当 $ x \geq 0 $ 时，$ F(x) = P(X \leq x) = 1 - e^{-\lambda x} $。
- 核心性质：**无记忆性**（即 $P(X > s + t \mid X > s) = P(X > t)$）
{%note danger%}
- $E(X) = \frac{1}{\lambda}$
- $D(X) = \frac{1}{\lambda^2}$
{%endnote%}

#### （2）均匀分布
- 记作：$X \sim U(a,b)$ 
- **概率密度函数**：  
  - 当 $ a \leq x \leq b $ 时：$ f(x) = \frac{1}{b-a} $
  - 其他情况：$ f(x) = 0 $
- **分布函数**：  
  - 当 $x < a$ 时：$F(x) = P(X \leq x) = 0$
  - 当 $a \leq x \leq b$ 时：$F(x) = P(X \leq x) = \frac{x-a}{b-a}$
  - 当 $x > b$ 时：$F(x) = P(X \leq x) = 1$
- 核心性质：落在子区间的概率与**子区间长度成正比**，与区间位置无关
{%note danger%}
- $E(X) = \frac{1}{b-a}$,
- $D(X) = \frac{(b-a)^2}{12}$
{%endnote%}

#### （3）正态分布
记作：$X \sim N(\mu,\sigma^2)$
- **一般正态分布**：  
  - 概率密度函数：$ f(x) = \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{ (x-\mu)^2} {2\sigma^2} }$  
  - 参数含义：  
    - $\mu$：均值，决定曲线**中心位置**  
    - $\sigma$：标准差，决定曲线**离散程度**（$\sigma$ 越小，曲线越集中于 $\mu$）  

- **标准正态分布**：  
  - 定义：当 $\mu = 0, \sigma = 1$ 时，记为 $X \sim N(0,1)$  
  - 分布函数：$\Phi(x) = \int_{-\infty}^{x} \varphi(t) dt$  
  - 对称性：$\varphi(x) = \varphi(-x)$，故 $\Phi(-x) = 1 - \Phi(x)$

- **一般转标准的线性变换**：  
  若 $X \sim N(\mu, \sigma^2)$，令 $Y = \frac{X - \mu}{\sigma}$，则 $Y \sim N(0,1)$  
  - 区间概率计算：  $P(a \leq X \leq b) = \Phi\left(\frac{b - \mu}{\sigma}\right) - \Phi\left(\frac{a - \mu}{\sigma}\right)$
{%note info%}
用含有 $\Phi$ 的参数式表示在不同范围的概率，计算期望时系数可以消去
{%endnote%}
若有 $X \sim N(\mu_1, \sigma_1^2)$，$Y \sim N(\mu_2, \sigma_2^2)$ ，则有 $X - Y \sim N(\mu_1 - \mu_2, \sigma_1^2 + \sigma_2^2)$
#### （4）泊松分布的数字特征
{%note danger%}
- $E(X) = \lambda$  
- $D(X) = \lambda$  
{%endnote%}
- 推导依据：$D(X) = E(X^2) - [E(X)]^2 = (\lambda^2 + \lambda) - \lambda^2 = \lambda$
- 记作：$X∼P(\lambda)$

#### （5）伽马分布
{%note danger%}
- $E(X)=\alpha\beta$	
- $D(X)=\alpha\beta^2$
{%endnote%}
- 记作：$X \sim \Gamma(\alpha, \beta)$

## 三、多维随机变量及其分布
### 3.1 基本概念
- **定义**：多维随机变量是将多个随机变量作为一个整体研究的概率模型，最常见的是**二维随机变量** $(X,Y)$。  
  - 整体视角：$(X,Y)$ 具有联合概率分布，描述两个变量取值的联合概率规律；  
  - 个体视角：$X$ 和 $Y$ 分别具有各自的“边缘分布”，描述单个变量的概率规律。


### 3.2 二维随机变量的联合分布
#### （1）连续型二维随机变量
- **联合概率密度函数**：设 $(X,Y)$ 为连续型，若存在非负函数 $f(x,y)$（$x,y \in \mathbb{R}$），使得对任意平面区域 $D$，有  $P\{ (X,Y) \in D \} = \iint_{D} f(x,y) dxdy$ ，则称 $f(x,y)$ 为 $(X,Y)$ 的联合概率密度函数。  
- **联合分布函数**：$F(x,y) = P(X \leq x, Y \leq y) = \int_{-\infty}^{y} \int_{-\infty}^{x} f(u,v) \, dudv$，描述 $(X,Y)$ 落在区域 $(-\infty,x] \times (-\infty,y]$ 内的概率。
{%note info%}
若 $X$,$Y$相互独立，其联合概率密度分布可对应平面直角坐标系中的区域。概率大小可通过该区域在全平面内所占的面积比例推断，而直线因面积为 0，对应的概率也为 0。
{%endnote%}
 
#### （2）离散型二维随机变量
- **联合分布律**：设 $(X,Y)$ 的所有可能取值为 $(x_i, y_j)$（$i,j=1,2,\dots$），则称  $P(X = x_i, Y = y_j) = p_{ij} \quad (i,j=1,2,\dots)$  为 $(X,Y)$ 的联合分布律，满足性质：$p_{ij} \geq 0$ 且 $\sum_{i=1}^{\infty} \sum_{j=1}^{\infty} p_{ij} = 1$。  
- **联合分布函数**：$F(x,y) = \sum_{x_i \leq x} \sum_{y_j \leq y} p_{ij}$，即对所有满足 $x_i \leq x$ 且 $y_j \leq y$ 的 $(x_i, y_j)$ 对应的概率求和。


### 3.3 边缘分布
边缘分布是从联合分布中“提取”单个随机变量（$X$ 或 $Y$）的分布，分为边缘概率密度（连续型）和边缘分布律（离散型）。

#### （1）连续型：边缘概率密度
- **X 的边缘概率密度**：对联合概率密度 $f(x,y)$ 关于 $y$ 积分，消去 $y$ 的影响：  $f_X(x) = \int_{-\infty}^{+\infty} f(x,y) \, dy$ 。对应的边缘分布函数：$F_X(x) = P(X \leq x) = \int_{-\infty}^{x} f_X(t) \, dt = \int_{-\infty}^{x} \left( \int_{-\infty}^{+\infty} f(t,y) \, dy \right) dt$  
- **Y 的边缘概率密度**：对联合概率密度 $f(x,y)$ 关于 $x$ 积分，消去 $x$ 的影响：  $f_Y(y) = \int_{-\infty}^{+\infty} f(x,y) \, dx$。对应的边缘分布函数：$F_Y(y) = P(Y \leq y) = \int_{-\infty}^{y} f_Y(t) \, dt = \int_{-\infty}^{y} \left( \int_{-\infty}^{+\infty} f(x,t) \, dx \right) dt$

{%note info%}
若题目问$Z=X+Y$的概率密度，则有$f_Z(z) = f_X * f_Y(z) = \int_{-\infty}^{+\infty} f_X(x) f_Y(z - x) dx$
{%endnote%}
#### （2）离散型：边缘分布律
- **X 的边缘分布律**：对联合分布律 $p_{ij}$ 按列求和（固定 $x_i$，对所有 $y_j$ 求和）：  $P(X = x_i) = p_{i\cdot} = \sum_{j=1}^{\infty} p_{ij} \quad (i=1,2,\dots)$ 
- **Y 的边缘分布律**：对联合分布律 $p_{ij}$ 按行求和（固定 $y_j$，对所有 $x_i$ 求和）：  $P(Y = y_j) = p_{\cdot j} = \sum_{i=1}^{\infty} p_{ij} \quad (j=1,2,\dots)$


### 3.4 二维随机变量的独立性
#### （1）核心定义
若对任意实数 $x,y$，二维随机变量 $(X,Y)$ 的联合分布函数等于 $X$ 和 $Y$ 边缘分布函数的乘积，即  $F(x,y) = F_X(x) \cdot F_Y(y)$ ，则称 $X$ 与 $Y$ **相互独立**。

#### （2）不同类型的等价判定条件
- **离散型**：对所有可能的 $(x_i, y_j)$，满足  $P(X = x_i, Y = y_j) = P(X = x_i) \cdot P(Y = y_j) \quad (\text{即 } p_{ij} = p_{i\cdot} \cdot p_{\cdot j})$
- **连续型**：对几乎所有 $(x,y)$（除面积为0的区域外），满足 $f(x,y) = f_X(x) \cdot f_Y(y)$

## 四、随机变量的数字特征
### 4.1数学期望
- 定义公式：$E(X)= \int_{-\infty}^{+\infty} x \, f_X(x) \, dx$
- 性质：
  - $E(aX+bY)=aE(X)+bE(Y)$
  - $E(XY)=E(X)E(Y)$

{%note info%}
期望中含有表达式的将平方等拆开，运用期望的性质进行分解计算
{%endnote%}
### 4.2 方差
- 定义公式（展开式）：  
$D(X) = E\{ [X - E(X)]^2 \}$
$= E\{ X^2 - 2X \cdot E(X) + [E(X)]^2 \}$
$= E(X^2) - 2E(X) \cdot E(X) + [E(X)]^2$
$= E(X^2) - [E(X)]^2$
- 性质：
  - $D(常数)=0$
  - $D(aX+bY)=a^2 D(X)+b^2 D(Y)$
### 4.3 协方差与相关系数
- 定义式：
  - $Cov=E[X-E(X)][Y-E(Y)]$
  - $\rho=\frac{\text{Cov}(X,Y)}{\sigma_X \sigma_Y}$
- 性质：
  - $Cov(X,Y)=Cov(Y,X)$
  - $Cov(aX,bY)=abCov(X,Y)$
  - $Cov(X_1+X_2,Y)=Cov(X_1,Y)+Cov(X_2,Y)$
  - $D(X \pm Y) = D(X) + D(Y) \pm 2\,\text{Cov}(X,Y)$
  - $Cov(X,Y)=E(XY)-E(X)E(Y)$ 有 $Cov(X,X)=D(X)$

$\rho$是反映随机变量$X$、$Y$之间线性关系程度大小的一个量，其绝对值越大表明$X$、$Y$之间现行依赖关系越显著。
- $\rho=0$  不相关
- $\rho=1$  完全正相关
- $\rho=-1$  完全负相关
{%note info%}
若已知 $\rho_{xy}=1$，则说明有 $Y=aX+b$ ，根据题给条件，对**等式两边取期望和方差**，求得参数的值
{%endnote%}

## 五、大数定律与中心极限定理
- 条件：随机变量具有同一分布，且具有相同的期望和方差
- 切比雪夫不等式：$P(|X - E(X)| \geq \epsilon) \leq \frac{D(X)}{\epsilon^2}$ 
  - 其等价形式是：$P(|X - E(X)| \leq \epsilon) \geq 1 - \frac{D(X)}{\epsilon^2}$ 
{%note info%}
可以看出方差越小，取值在区间内的概率也就越大，**取值就越集中于均值附近**
{%endnote%}

## 六、数理统计基础
### 6.1 数理统计基本概念
样本方差的期望=总体方差：$E(S^2) = E\left[ \frac{1}{n-1} \sum_{i=1}^{n} (X_i - \bar{X})^2 \right] = \sigma^2$
{%note danger%}
注意分母是$n-1$，不是$n$
{%endnote%}

$E(S^2) = \frac{1}{n-1} \sum_{i=1}^{n} E\left[ (X_i - \bar{X})^2 \right]$
### 6.2 三大抽样分布
#### （1）卡方分布（$\chi^2$ 分布）
- 构造：若 $X_1, X_2, \dots, X_n \sim N(0,1)$ 且相互独立，则 $\chi^2 = \sum_{i=1}^{n} X_i^2 \sim \chi^2(n)$  
- 记法：$\chi^2 \sim \chi^2(n)$（$n$ 为自由度）  

- **数字特征**：  
  1. 期望：$E(\chi^2(n)) = n$  
  2. 方差：$D(\chi^2(n)) = 2n$  
  3. 可加性：若 $X \sim \chi^2(m)$、$Y \sim \chi^2(n)$ 且独立，则 $X + Y \sim \chi^2(m+n)$ 
  4. $\frac{(n-1)S^2}{\sigma} \sim \chi^2(n-1)$ 

#### （2）t 分布
- 构造：若 $X \sim N(0,1)$、$Y \sim \chi^2(n)$ 且相互独立，则 $T = \frac{X}{\sqrt{Y/n}} \sim t(n)$  
- 记法：$T \sim t(n)$（$n$ 为自由度）  

- **数字特征**：  
  1. 期望：当 $n > 1$ 时，$E(T) = 0$  
  2. 方差：当 $n > 2$ 时，$D(T) = \frac{n}{n-2}$  

- **与其他分布的关系**：  
  1. 当 $n$ 较小时，t 分布比标准正态分布更“平坦”，尾部更厚（极端值概率更高）  
  2. 当 $n \to \infty$ 时，$t(n) \to N(0,1)$；通常 $n > 30$ 时两者近似程度高


#### （3）F 分布
- 构造：若 $U \sim \chi^2(m)$、$V \sim \chi^2(n)$ 且相互独立，则 $F = \frac{U/m}{V/n} \sim F(m,n)$  
- 记法：$F \sim F(m,n)$（$m$ 为第一自由度，$n$ 为第二自由度）  

- **数字特征**：  
  1. 期望：当 $n > 2$ 时，$E(F) = \frac{n}{n-2}$  
  2. 方差：当 $n > 4$ 时，$D(F) = \frac{2n^2(m + n - 2)}{m(n - 2)^2(n - 4)}$

- **重要性质**：  
  1. 倒数性质：若 $F \sim F(m,n)$，则 $\frac{1}{F} \sim F(n,m)$  
  2. 与 t 分布的关系：若 $T \sim t(n)$，则 $T^2 \sim F(1,n)$  


## 七、参数估计
### 7.1 点估计
1. 矩估计：已知总体服从某种分布（如正态分布、二项分布），给定样本 $ X_1, X_2, ..., X_n $，通过**样本矩等于总体矩**的原则，建立方程求解总体分布的未知参数。 
{%note info %} 
- 用样本一阶原点矩 $ \frac{1}{n}\sum_{i=1}^n X_i $ 估计总体一阶原点矩 $  E(X) $；
- 用样本二阶中心矩 $ \frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2 $ 估计总体二阶中心矩 $ D(X) $，以此类推。{%endnote%}

#### 最大似然估计
核心目标是找到未知参数 $ \theta $，使得“当前样本出现的概率/概率密度”最大，该 $ \theta $ 即为最大似然估计量 $ \hat{\theta}$
#### 离散型 && 连续型
设总体 $ X $ 为离散型，概率分布律为 $ P \( X = x \) = p(x; \theta) $，其中 $ \theta $ 为待估参数；样本观测值为 $ x_1, x_2, ..., x_n $。

1. **构造似然函数**：$L(\theta) = L(x_1, x_2, ..., x_n; \theta) = \prod_{i=1}^{n} p(x_i; \theta)$  
2. **构造对数似然函数对**：似然函数取自然对数（单调性不变）：$\ln L(\theta) = \sum_{i=1}^{n} \ln p(x_i; \theta)$  
3. **求解最大似然估计量**：对 $ \ln L(\theta) $ 关于 $ \theta $ 求导，并令导数等于0，解出 $ \hat{\theta} $：$\frac{d \ln L(\theta)}{d\theta} = 0$

{%note info %}
若总体含多个未知参数，则使用**拉格朗日乘数法**（对每个参数求偏导数，并令所有偏导数等于 0，解方程组得到各参数的最大似然估计量）{%endnote %}

{%note danger%}
注意 $\prod$ 符号对等式两端取对数后变为 $\sum$
{%endnote%}

#### 一阶原点矩
- **样本一阶原点矩**：$\bar{X} = \frac{1}{n}\sum_{i=1}^{n} X_i \quad \text{（即样本均值）}$
- **总体一阶原点矩**：$E(X) \quad \text{（即总体期望）}$

#### 一阶中心矩
- **样本一阶中心矩**：$\frac{1}{n}\sum_{i=1}^{n} (X_i - \bar{X}) = 0$
- **总体一阶中心矩**：$E[X - E(X)] = 0$

#### 二阶中点矩
- **样本二阶中心矩**：$S_0^2 = \frac{1}{n}\sum_{i=1}^{n} (X_i - \bar{X})^2$
- **总体二阶中心矩**：$E[X - E(X)]^2 = D(X) \quad \text{（即总体方差）}$

#### 二阶原点矩
- **样本二阶原点矩**：$\frac{1}{n}\sum_{i=1}^{n} X_i^2$
- **总体二阶原点矩**：$E(X^2)$

### 7.2 估计量的评选标准
#### 1. 无偏性
若对任意 $ \theta \in \Theta $ $\Theta $ 为参数空间），都有  $E\hat{\theta}) = \theta$  则称 $ \hat{\theta} $ 是 $ \theta $ 的**无偏估计量**；$ E(\hat{\theta})- \theta $为系统误差；若满足$\lim_{n \to \infty} E(\hat{\theta})   = \theta$ 则称 $ \hat{\theta} $ 是 $ \theta $ 的**渐近无偏估计量**
#### 2. 有效性
方差最小的无偏估计量最好，$D(\hat{\theta_1}) > D(\hat{\theta_2})$，$\theta_2$更有效
{%note info%}
**实例分析**
设 $X_1, X_2, X_3$ 是来自总体 $X$ 的独立同分布样本，且总体方差 $D(X)=\sigma^2$，考虑以下三个估计量（均为无偏估计）：
1. $M = \frac{1}{2}X_1 + \frac{1}{2}X_2$
2. $N_1 = \frac{1}{3}X_1 + \frac{1}{4}X_2 + \frac{7}{12}X_3$
3. $N_2 = \frac{1}{2}X_1 + \frac{1}{3}X_2 + \frac{1}{6}X_3$

**步骤1：计算各估计量的方差**
根据方差的性质 $D(aX+bY)=a^2 D(X)+b^2 D(Y)$（独立样本，方差满足可加性）

**步骤2：比较有效性**
方差大小关系：$D(N_1) > D(M) > D(N_2)$，因此：
- $N_2$ 比 $M$ 和 $N_1$ 更有效；
- $M$ 比 $N_1$ 更有效；
- $N_2$ 是这三个估计量中最优的无偏估计量。
{%endnote%}
### 区间估计
置信度($1-\alpha$)：在一定的置信水平下，寻找区间长度最短的置信区间

<table cellpadding="8" cellspacing="0" style="width: 100%; text-align: center; font-family: Arial, sans-serif;">
  <tr style="background-color:white; font-weight: bold;">
    <td>总体类型</td>
    <td>待估计参数</td>
    <td>其他<br>参数</td>
    <td>所用的枢轴量及其分布</td>
    <td>置信区间</td>
  </tr>

  <tr>
    <td rowspan="3" style="background-color:white; vertical-align: middle;">单个正态总体</td>
    <td>$\mu$</td>
    <td>$\sigma^2$ 已知</td>
    <td>$\frac{\bar{X} - \mu}{\sigma / \sqrt{n}} \sim N(0,1)$</td>
    <td>$\left( \bar{X} \pm \frac{\sigma}{\sqrt{n}} z_{\alpha/2} \right)$</td>
  </tr>
  <tr>
    <td>$\mu$</td>
    <td>$\sigma^2$ 未知</td>
    <td>$\frac{\bar{X} - \mu}{S / \sqrt{n}} \sim t(n-1)$</td>
    <td>$\left( \bar{X} \pm \frac{S}{\sqrt{n}} t_{\alpha/2}(n-1) \right)$</td>
  </tr>
  <tr>
    <td>$\sigma^2$</td>
    <td>$\mu$ 未知</td>
    <td>$\frac{(n-1)S^2}{\sigma^2} \sim \chi^2(n-1)$</td>
    <td>$\left( \frac{(n-1)S^2}{\chi^2_{\alpha/2}(n-1)}, \frac{(n-1)S^2}{\chi^2_{1-\alpha/2}(n-1)} \right)$</td>
  </tr>
  
  <tr>
    <td rowspan="3" style="background-color: white; vertical-align: middle;">两个正态总体</td>
    <td>$\mu_1 - \mu_2$</td>
    <td>$\sigma_1^2, \sigma_2^2$ 已知</td>
    <td>$\frac{\bar{X} - \bar{Y} - (\mu_1 - \mu_2)}{\sqrt{\frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}}} \sim N(0,1)$</td>
    <td>$\left( \bar{X} - \bar{Y} \pm z_{\alpha/2} \sqrt{\frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}} \right)$</td>
  </tr>
  <tr>
    <td>$\mu_1 - \mu_2$</td>
    <td>$\sigma_1^2 = \sigma_2^2$ 未知</td>
    <td>$\frac{\bar{X} - \bar{Y} - (\mu_1 - \mu_2)}{S_w \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}} \sim t(n_1 + n_2 - 2)$</td>
    <td>$\left( \bar{X} - \bar{Y} \pm t_{\alpha/2}(n_1 + n_2 - 2) S_w \sqrt{\frac{1}{n_1} + \frac{1}{n_2}} \right)$</td>
  </tr>
  <tr>
    <td>$\frac{\sigma_1^2}{\sigma_2^2}$</td>
    <td>$\mu_1, \mu_2$ 未知</td>
    <td>$\frac{S_1^2 / \sigma_1^2}{S_2^2 / \sigma_2^2} \sim F(n_1 - 1, n_2 - 1)$</td>
    <td>$\left( \frac{S_1^2}{S_2^2} \cdot \frac{1}{F_{\alpha/2}(n_1 - 1, n_2 - 1)}, \frac{S_1^2}{S_2^2} \cdot \frac{1}{F_{1-\alpha/2}(n_1 - 1, n_2 - 1)} \right)$</td>
  </tr>
</table>