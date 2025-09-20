---
title: College Physics 2
date: 2025-09-02 21:11:58
tags: [College Physics]
category: Review
category_bar: true
archive: true
math: true
---

理想气体的**物态方程**

$$PV=\frac{m}{M_{mol}}RT$$

$\frac{m}{M}$ 叫做物质的量

 $R $ 叫做普适气体常量

$$PV=nRT$$


道尔顿**分压定律**：混合气体的压强等于各个气体的压强之和
$$ P=P_{1}+P_{2}+P_{3}+...=(n_{1}+n_{2}+n_{3}+...)kT$$


$$ \begin{aligned}
P &= \frac{F}{S} = \frac{I}{l_2l_3\Delta t} = \frac{m_0}{l_1l_2l_3} \sum_{i=1}^{N} v_{ix}^2 = \frac{Nm_0}{l_1l_2l_3} \frac{\sum_{i=1}^{N} v_{ix}^2}{N} 
= \frac{N}{V}m_0\overline{v_x^2} = nm_0\overline{v_x^2} 
= \frac{1}{3}nm_0\overline{v^2} = \frac{2}{3}n\overline{\varepsilon}
\end{aligned} $$

又因为：

$$\overline{\varepsilon} = \frac{1}{2} m_0 \overline{v^2} = \frac{3}{2} kT$$
所以，**方均根速率**：
$$\sqrt{\overline{v^2}} = \sqrt{\frac{3kT}{m_0}} = \sqrt{\frac{3RT}{M}}$$

### 能量均分定理、内能
每个分子的**平均动能**：
$$\overline{E_k} = \frac{i}{2}kT$$

其中：$k = \frac{R}{N_A}$

| 分子类型           | 原子数 (N) | 平动自由度 | 转动自由度 | 振动自由度 | 总自由度 (3N) |
| :----------------- | :--------: | :--------: | :--------: | :--------: | :-----------: |
| **单原子**         |     1      |     3      |     0      |     0      |       3       |
| **双原子（线性）** |     2      |     3      |     2      |     1      |       6       |
| **多原子（线性）** |     N      |     3      |     2      |  3N - 5    |      3N       |
| **多原子（非线性）**|     N      |     3      |     3      |  3N - 6    |      3N       |

单原子分子有三个自由度：
$$\overline{E_k} = \frac{3}{2}kT$$

双原子分子有五个自由度：
$$\overline{E_k} = \frac{5}{2}kT$$

多原子分子有六个自由度：

$$\overline{E_k} = 3kT$$

理想气体内能：
$$E= \frac{m}{M}\frac{i}{2}RT $$

### 麦克斯韦速率分布
速率分布曲线**归一化**处理：
$$\int_{0}^{\infty} f(v)  dv = 1$$

#### 平均速率
$$\overline{\nu} = \int_{0}^{\nu_o} vf(v)  dv=1.60\sqrt{\frac{RT}{M}}$$

#### 方均根速率
$$\overline{\nu^2} = \int_{0}^{\infty} v^2 f(\nu)  d\nu$$

$$\sqrt{\overline{\nu^2}}=1.73\sqrt{\frac{RT}{M}}$$

#### 最概然速率
$$v_p=1.41\sqrt{\frac{RT}{M}}$$

### 分子碰撞与自由程

#### 平均碰撞频率
$$\overline{Z}=\sqrt{2} \pi d^2 \overline{\nu} n$$

#### 平均自由程
$$\overline{\lambda} = \frac{\overline{v}}{\overline{Z}} = \frac{1}{\sqrt{2} \pi d^2 n}$$


| 过程   | 特征| 过程方程  | 吸放热量 | 对外做功 | 内能增量 |
| ------ | ---------- | -------------- | -------------- | --------- | --------------- |
| 等容  | $V=$常量| $\frac{p}{T}=$常量                | $\frac{m}{M}C_v(T_2 - T_1)$ <br>$\frac{m}{M}\frac{i}{2}R(T_2 - T_1)$ |$0$| $\frac{m}{M}\frac{i}{2}R(T_2 - T_1)$ |
| 等压   |$p=$常量| $\frac{V}{T}=$常量 | $\frac{m}{M}C_p(T_2 - T_1)$ <br> $\frac{m}{M}\frac{i + 2}{2}R(T_2 - T_1)$ | $p(V_2 - V_1)$ <br> $\frac{m}{M}R(T_2 - T_1)$  | $\frac{m}{M}\frac{i}{2}R(T_2 - T_1)$ |
| 等温   |$T=$常量| $pV=$常量 | $\frac{m}{M}RT\ln\frac{V_2}{V_1}$ <br> $\frac{m}{M}RT\ln\frac{P_1}{P_2}$   | $\frac{m}{M}RT\ln\frac{V_2}{V_1}$ <br> $\frac{m}{M}RT\ln\frac{P_1}{P_2}$ | $0$ |
| 绝热   |$Q=0$|$pV^\gamma=$常量 <br> $V^{\gamma - 1}T=$常量 <br> $p^{\gamma - 1}T^{-\gamma}=$常量 |$0$|$-\frac{m}{M}\frac{i}{2}R(T_2 - T_1)$ | $\frac{m}{M}\frac{i}{2}R(T_2 - T_1)$ |


$C_{v,m}$ 1mol气体在体积不变的情况下，温度改变1k所吸收或者放出的热量

$C_{p,m}=C_{v,m}+R$ 1mol气体升高1k，在等压过程中比等容过程多吸收8.31J能量

$C_{p,m}=\frac{i}{2}R+R$