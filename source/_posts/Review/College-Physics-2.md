---
title: College Physics 2
date: 2025-09-02 21:11:58
tags:
archive: true
math: true
category: Review
category_bar: true
description: 大学物理（下）期末复习笔记，涵盖热力学基础、机械振动与电磁振荡、机械波与电磁波、光学、量子力学等核心知识点。
---

## 第六章 热力学基础

### 物态方程

理想气体的**物态方程**：

$$PV = \frac{m}{M}RT$$

其中，$\frac{m}{M}$ 叫做物质的量，$R = 8.31 \text{ J/(mol·K)}$ 叫做普适气体常量。

上述式子也可以写成：

$$P = nkT$$

其中，$n = \frac{N}{V}$ 为单位体积分子数，$k = 1.38 \times 10^{-23} \text{ J/K}$ 为玻尔兹曼常数。

### 分压定律

道尔顿**分压定律**：混合气体的压强等于各个气体的压强之和。

$$P = P_1 + P_2 + P_3 + \cdots = (n_1 + n_2 + n_3 + \cdots)kT$$

压强公式的推导：

$$\begin{aligned}
P &= \frac{F}{S} = \frac{I}{l_2 l_3 \Delta t} = \frac{m_0}{l_1 l_2 l_3} \sum_{i=1}^{N} v_{ix}^2 = \frac{Nm_0}{l_1 l_2 l_3} \frac{\sum_{i=1}^{N} v_{ix}^2}{N} \\
  &= \frac{N}{V}m_0 \overline{v_x^2} = nm_0 \overline{v_x^2} \\
  &= \frac{1}{3}nm_0 \overline{v^2} = \frac{2}{3}n\overline{\varepsilon}
\end{aligned}$$

又因为：

$$\overline{\varepsilon} = \frac{1}{2}m_0 \overline{v^2} = \frac{3}{2}kT$$

所以，**方均根速率**：

$$\sqrt{\overline{v^2}} = \sqrt{\frac{3kT}{m_0}} = \sqrt{\frac{3RT}{M}}$$

### 能量均分定理与内能

#### 气体分子

- **平均平动动能**：$\overline{\varepsilon} = \frac{3}{2}kT$
- **分子平均总动能**：$\overline{\varepsilon} = \frac{i}{2}kT$

其中：$k = \frac{R}{N_A}$

| 分子类型 | 自由度 $i$ | 平动动能 | 转动动能 | 总动能（内能） |
|:--------:|:----------:|:--------:|:--------:|:--------------:|
| 单原子 | $3 \ (3+0)$ | $\frac{3}{2}kT$ | $0$ | $\frac{3}{2}kT$ |
| 双原子 | $5 \ (3+2)$ | $\frac{3}{2}kT$ | $kT$ | $\frac{5}{2}kT$ |
| 多原子 | $6 \ (3+3)$ | $\frac{3}{2}kT$ | $\frac{3}{2}kT$ | $3kT$ |

#### 理想气体

- 动能：$E = \frac{3}{2}\frac{m}{M}RT$
- 内能：$E = \frac{i}{2}\frac{m}{M}RT$

### 麦克斯韦速率分布

速率分布曲线**归一化**处理：

$$\int_{0}^{\infty} f(v) \, dv = 1$$

- **平均速率**：$\overline{v} = \int_{0}^{\infty} vf(v) \, dv = \sqrt{\dfrac{8RT}{\pi M}}$
- **方均根速率**：$\sqrt{\overline{v^2}} = \sqrt{\dfrac{3RT}{M}}$
- **最概然速率**：$v_p = \sqrt{\dfrac{2RT}{M}}$

{%note info%}
**三种速率的大小关系**：$v_p < \overline{v} < \sqrt{\overline{v^2}}$
{%endnote%}

### 分子碰撞与自由程

**平均碰撞频率**：

$$\overline{Z} = \sqrt{2}\pi d^2 \overline{v} n$$

**平均自由程**：

$$\overline{\lambda} = \frac{\overline{v}}{\overline{Z}} = \frac{1}{\sqrt{2}\pi d^2 n}$$

### 热力学过程

| 过程 | 特征 | 过程方程 | 吸放热量 $Q$ | 对外做功 $A$ | 内能增量 $\Delta E$ |
|:----:|:----:|:--------:|:------------:|:------------:|:-------------------:|
| 等容 | $V=$ 常量 | $\frac{p}{T}=$ 常量 | $\frac{m}{M}C_v(T_2-T_1)$ | $0$ | $\frac{m}{M}\frac{i}{2}R(T_2-T_1)$ |
| 等压 | $p=$ 常量 | $\frac{V}{T}=$ 常量 | $\frac{m}{M}C_p(T_2-T_1)$ | $p(V_2-V_1)$ | $\frac{m}{M}\frac{i}{2}R(T_2-T_1)$ |
| 等温 | $T=$ 常量 | $pV=$ 常量 | $\frac{m}{M}RT\ln\dfrac{V_2}{V_1}$ | $\frac{m}{M}RT\ln\dfrac{V_2}{V_1}$ | $0$ |
| 绝热 | $Q=0$ | $pV^\gamma=$ 常量 | $0$ | $-\Delta E$ | $\frac{m}{M}\frac{i}{2}R(T_2-T_1)$ |

{%note info%}
- 绝热过程曲线是最陡峭的（比等温线陡）
- 等温过程中 $Q = A$（吸热全部用来做功）
- 绝热过程中 $A = -\Delta E$（靠消耗内能做功）
{%endnote%}

### 摩尔热容

- $C_{V,m}$：1 mol 气体在体积不变的情况下，温度改变 1K 所吸收或放出的热量，$C_{V,m} = \frac{i}{2}R$
- $C_{p,m} = C_{V,m} + R = \frac{i+2}{2}R$，即 1 mol 气体升高 1K，等压过程比等容过程多吸收 8.31J 能量（用于对外做功）

{%note info%}
**迈耶公式**：$C_{p,m} = C_{V,m} + R$

**比热容比**：$\gamma = \frac{C_{p,m}}{C_{V,m}} = \frac{i+2}{i}$
{%endnote%}

### 热力学第一定律

$$Q = \Delta E + A$$

系统吸收的热量等于内能增量与系统对外做功之和。

{%note info%}
**符号约定**：
- $Q > 0$：系统吸热；$Q < 0$：系统放热
- $A > 0$：系统对外做功；$A < 0$：外界对系统做功
- $\Delta E > 0$：内能增加；$\Delta E < 0$：内能减少
{%endnote%}

### 热力学第二定律

- **开尔文表述**：不可能从单一热源吸收热量，使之完全变为有用功而不产生其他影响（第二类永动机不可能）
- **克劳修斯表述**：热量不可能自动地从低温物体传到高温物体

**可逆过程**：系统从状态 A 经某过程变到状态 B，如果能使系统进行逆向变化，恢复到状态 A 且外界同时恢复原状。

**熵增原理**：孤立系统的熵永不减少，可逆过程熵不变，不可逆过程熵增加。

$$\Delta S = \int \frac{dQ}{T}$$

### 卡诺循环

一切自发过程都不可逆！

卡诺循环由两个等温过程 + 两个绝热过程组成（为理论最大效率）：

$$\eta = \frac{A}{Q_1} = \frac{Q_1 - Q_2}{Q_1} \leq 1 - \frac{T_2}{T_1}$$

### 制冷机

卡诺制冷机制冷系数：

$$w = \frac{Q_2}{A} = \frac{T_2}{T_1 - T_2}$$

其中，$Q_2$ 为从低温物体吸收的热量；$A$ 为制冷机消耗的功；$T_2$ 为低温物体的热力学温度；$T_1$ 为高温物体的热力学温度。

{%note info%}
**例题**：1 mol 双原子理想气体（$i = 5$）经过等温膨胀、等容降温、绝热压缩三个过程完成一个循环。已知等温膨胀中 $V_1 = V_0$，$V_2 = 2V_0$，温度 $T_1 = 400 \text{ K}$。求循环效率。

**解**：
- 等温膨胀（吸热）：$Q_1 = RT_1 \ln\frac{V_2}{V_1} = 8.31 \times 400 \times \ln 2 \approx 2302 \text{ J}$
- 等容降温（放热）：$\Delta E = \frac{5}{2}R(T_1 - T_2)$，$Q_2 = \Delta E = \frac{5}{2} \times 8.31 \times (400 - T_2)$
- 绝热压缩：$Q = 0$，由绝热方程确定 $T_2$

效率 $\eta = 1 - \frac{Q_2}{Q_1}$（具体数值需由绝热过程方程确定 $T_2$）
{%endnote%}

---

## 第十章 机械振动和电磁振荡

### 简谐振动

**线性回复力**：物体所受到的合外力大小总是与物体离开平衡位置的位移大小成正比且方向相反。

$$F = -kx$$

简谐振动的运动方程：

$$x = A\cos(\omega t + \phi)$$

简谐振动的微分方程：

$$\frac{d^2 x}{dt^2} + \omega^2 x = 0$$

角频率（固有频率）：

$$\omega = \sqrt{\frac{k}{m}}$$

周期与频率：

$$T = \frac{2\pi}{\omega} = 2\pi\sqrt{\frac{m}{k}}, \quad \nu = \frac{1}{T}$$

{%note info%}
注意对弹簧"串并联"情况下的等效劲度系数 $k$ 进行分析：
- 串联：$\frac{1}{k} = \frac{1}{k_1} + \frac{1}{k_2}$
- 并联：$k = k_1 + k_2$

**单摆**：$\omega = \sqrt{\dfrac{g}{l}}$（小角度摆动时）

**复摆**：$\omega = \sqrt{\dfrac{mgl}{J}}$（$J$ 为转动惯量，$l$ 为质心到转轴距离）
{%endnote%}

### 简谐振动的旋转矢量法

用旋转矢量（振幅矢量）来表示简谐振动：矢量长度 = 振幅 $A$，角速度 = 角频率 $\omega$，初始角度 = 初相 $\phi$，矢量在 $x$ 轴上的投影 = 位移 $x$。

{%note info%}
**旋转矢量法的应用**：
- 已知 $x$ 和 $v$ 的方向确定 $\phi$：$v > 0$ 时矢量在下半圆，$v < 0$ 时在上半圆
- 求两振动的相位差
- 求从某一状态到另一状态的最短时间：$\Delta t = \frac{\Delta \phi}{\omega}$
{%endnote%}

### 简谐振动的能量

总能量：

$$E = \frac{1}{2}kA^2$$

$$E = E_k + E_p = \frac{1}{2}mv^2 + \frac{1}{2}kx^2$$

{%note info%}
- 在平衡位置（$x = 0$）：$E_k$ 最大，$E_p = 0$
- 在最大位移处（$x = \pm A$）：$E_k = 0$，$E_p$ 最大
- 动能和势能周期性变化，总能量守恒
{%endnote%}

### 简谐振动的合成

#### 同方向同频率

设有两个同方向同频率的简谐振动：

$$x_1 = A_1 \cos(\omega t + \phi_1)$$

$$x_2 = A_2 \cos(\omega t + \phi_2)$$

合位移为两个位移的代数和，仍为简谐振动：

$$x = A\cos(\omega t + \phi)$$

其中：

$$A = \sqrt{A_1^2 + A_2^2 + 2A_1 A_2 \cos(\phi_1 - \phi_2)}$$

$$\tan \phi = \frac{A_1 \sin \phi_1 + A_2 \sin \phi_2}{A_1 \cos \phi_1 + A_2 \cos \phi_2}$$

{%note info%}
**特殊情况**：
- $\phi_1 - \phi_2 = 2k\pi$（同相）：$A = A_1 + A_2$（加强）
- $\phi_1 - \phi_2 = (2k+1)\pi$（反相）：$A = |A_1 - A_2|$（减弱）
{%endnote%}

{%note warning%}
若题目中一个振动为 $\sin$ 形式，需要先化成 $\cos$ 的形式再合成：$\sin\theta = \cos(\theta - \frac{\pi}{2})$
{%endnote%}

#### 同方向不同频率（拍）

两个频率相近的简谐振动合成时产生"拍"现象：

- **拍频**：$\nu_{\text{拍}} = |\nu_1 - \nu_2|$（单位时间内振幅强弱变化的次数）

#### 相互垂直的同频率

两个相互垂直的同频率简谐振动合成，轨迹一般为椭圆（利萨如图形）。当 $\Delta\phi = 0$ 或 $\pi$ 时退化为直线。

### 阻尼振动与受迫振动

- **阻尼振动**：振幅随时间衰减，$A(t) = A_0 e^{-\beta t}$
- **受迫振动**：在周期性外力作用下振动，稳定后以驱动力频率振动
- **共振**：当驱动力频率接近系统固有频率时，振幅达到最大

---

## 第十一章 机械波和电磁波

### 波动方程

$$y(x,t) = A\cos\left[\omega\left(t \pm \frac{x}{v}\right) + \phi\right]$$

当波向 $x$ 轴正方向传播时，由于 $x$ 越大振动相位越滞后，因此用负号：

$$y(x,t) = A\cos\left[\omega\left(t - \frac{x}{v}\right) + \phi\right]$$

当波向 $x$ 轴负方向传播时用正号：

$$y(x,t) = A\cos\left[\omega\left(t + \frac{x}{v}\right) + \phi\right]$$

{%note info%}
**波动方程中各量的物理意义**：
- $A$：振幅
- $\omega$：角频率，$\omega = 2\pi\nu = \frac{2\pi}{T}$
- $v$：波速（相速度），$v = \lambda\nu = \frac{\lambda}{T}$
- $\phi$：坐标原点处的初相
- 波长 $\lambda$：同一波线上相位差为 $2\pi$ 的两点间的距离
{%endnote%}

### 波的能量

能流密度（波强）：

$$I = \frac{1}{2}\rho A^2 \omega^2 v$$

### 驻波

两列频率、振幅相同，传播方向相反的波叠加形成驻波：

$$y = 2A\cos\frac{2\pi x}{\lambda}\cos\omega t$$

- **波腹**：振幅最大的位置，$\cos\frac{2\pi x}{\lambda} = \pm 1$，即 $x = \frac{k\lambda}{2}$
- **波节**：振幅为 0 的位置，$\cos\frac{2\pi x}{\lambda} = 0$，即 $x = \frac{(2k+1)\lambda}{4}$

反射：自由端反射时，无半波损失；固定端反射时，有半波损失（相位突变 $\pi$）。

{%note info%}
**波动能量 vs 弹簧振子能量**：
- 波动的能量：能量不守恒，动能和势能同步变化，在平衡位置处同时达到最大
- 弹簧振子的能量：能量守恒，动能和势能相互转换

**多普勒效应**：当波源或观察者运动时，接收到的频率发生变化：
$$\nu' = \frac{v + v_o}{v - v_s}\nu$$
其中 $v_o$ 为观察者朝波源方向的速度（相向为正），$v_s$ 为波源朝观察者方向的速度（相向为正）。
{%endnote%}

{%note info%}
**例题**：一平面简谐波沿 $x$ 轴正方向传播，已知 $t = 0$ 时波形如图，原点处质元位于正最大位移处。已知波速 $v = 200 \text{ m/s}$，振幅 $A = 0.02 \text{ m}$，波长 $\lambda = 4 \text{ m}$。求波动方程。

**解**：
- $\omega = \frac{2\pi v}{\lambda} = \frac{2\pi \times 200}{4} = 100\pi \text{ rad/s}$
- $t = 0$ 时原点 $x = 0$ 处 $y = A = 0.02$，即 $\cos\phi = 1$，所以 $\phi = 0$
- 波动方程：$y = 0.02\cos\left[100\pi\left(t - \frac{x}{200}\right)\right] \text{ m}$
{%endnote%}

---

## 第十二章 光学

获得相干光的方法：
1. **分波阵面法**（如双缝干涉）
2. **分振幅法**（如薄膜上下表面的反射光）

基本概念：
- **光程** = 折射率 × 路径长 = $n \times r$
- **光程差**：$\Delta = n_2 r_2 - n_1 r_1$
- **相位差**：$\Delta \phi = 2\pi \frac{\Delta}{\lambda}$

### 干涉

#### 双缝干涉

杨氏双缝干涉产生间距相等的干涉条纹。设双缝间距为 $d$，屏距双缝 $D$，到第 $k$ 级明纹的距离为 $x$：

$$\delta = r_2 - r_1 = \frac{dx}{D}$$

- $\delta = \pm k\lambda$ → **明纹**（$k = 0, 1, 2, \ldots$）
- $\delta = \pm (2k+1)\dfrac{\lambda}{2}$ → **暗纹**（$k = 0, 1, 2, \ldots$）

#### 等倾干涉

一块玻璃板，上表面和下表面的反射光经过透镜形成内疏外密的圆环，内小（$k$ 小）外大。

$$\delta = 2nd\cos\gamma + \frac{\lambda}{2}$$

其中，$n$ 为被射入介质的折射率，$d$ 为玻璃片厚度，$\gamma$ 为折射角。

{%note info%}
**半波损失判断**：
- 只在反射面产生
- 光疏→光密：有半波损失（附加 $\frac{\lambda}{2}$）
- 光密→光疏：无半波损失

**增透膜原理**：透射增强 = 反射相消
{%endnote%}

可见光波长范围：400nm ~ 760nm

#### 劈尖干涉

- 相邻两明（暗）条纹对应的高度差：$\Delta h = \dfrac{\lambda}{2n}$
- 相邻两明（暗）条纹间距：$l \sin\theta = \dfrac{\lambda}{2n}$

#### 牛顿环

光程差：$\delta = 2ne + \dfrac{\lambda}{2}$

由几何关系 $(R - e)^2 + r^2 = R^2$，近似得：

- **明纹半径**：$r = \sqrt{\dfrac{(2k-1)R\lambda}{2n}}$
- **暗纹半径**：$r = \sqrt{\dfrac{kR\lambda}{n}}$

#### 迈克尔逊干涉仪

$$d = N \frac{\lambda}{2}$$

### 衍射

#### 单缝衍射

$$\delta = a\sin\theta$$

- $\delta = \pm k\lambda$ → **暗纹**（$k = 1, 2, 3, \ldots$）
- $\delta = \pm (2k+1)\dfrac{\lambda}{2}$ → **明纹**（$k = 1, 2, 3, \ldots$）

其中，$a$ 为单缝宽度，$\theta$ 为衍射角。

中央亮纹宽度：$\Delta x = \frac{2f\lambda}{a}$（$f$ 为透镜焦距）

{%note info%}
**注意**：单缝衍射的明暗条件与双缝干涉**相反**！
- 双缝干涉：$k\lambda$ 为明纹
- 单缝衍射：$k\lambda$ 为暗纹
{%endnote%}

#### 光栅衍射

**光栅方程**：

$$(a+b)\sin\theta = \pm k\lambda \quad \text{（明纹）}$$

其中，$a$ 为单缝宽度，$b$ 为缝间距，$d = a + b$ 为光栅常数。

**缺级**：当光栅衍射的主极大位置与单缝衍射的暗纹位置重合时，该主极大会消失：

$$k = \frac{a+b}{a}k'$$

{%note info%}
光栅常数 $d$ 可根据单位长度光栅条纹数量进行计算：$d = \frac{1}{\text{条纹数/单位长度}}$
{%endnote%}

### 偏振光

#### 马吕斯定律

- 穿过第一个偏振片（起偏）：$I_1 = \frac{1}{2}I_0$
- 穿过第二个偏振片（检偏）：$I_2 = I_1 \cos^2\theta$

#### 布儒斯特定律

- 反射光为完全偏振光
- 折射光（透射光）为部分偏振光
- 反射光与折射光垂直：$i_B + \gamma = \dfrac{\pi}{2}$
- 布儒斯特角：$\tan i_B = \dfrac{n_2}{n_1}$

{%note info%}
**例题**：用波长 $\lambda = 500 \text{ nm}$ 的单色光垂直照射每毫米有 500 条刻线的光栅，求第一级和第二级明纹的衍射角。并判断最多能看到第几级明纹。

**解**：
- 光栅常数 $d = \frac{1}{500} \text{ mm} = 2 \times 10^{-6} \text{ m} = 2000 \text{ nm}$
- 光栅方程：$d\sin\theta = k\lambda$
- 第一级（$k = 1$）：$\sin\theta_1 = \frac{500}{2000} = 0.25$，$\theta_1 = 14.5°$
- 第二级（$k = 2$）：$\sin\theta_2 = \frac{1000}{2000} = 0.5$，$\theta_2 = 30°$
- 最大级次：$\sin\theta \leq 1$，$k_{\max} = \frac{d}{\lambda} = \frac{2000}{500} = 4$，最多看到第 4 级
{%endnote%}

---

## 第十三章 量子力学

### 光子基本物理量

- 光子能量：$E = h\nu = h\dfrac{c}{\lambda}$
- 光子动量：$p = \dfrac{h\nu}{c} = \dfrac{h}{\lambda}$
- 光子质量：$m = \dfrac{h\nu}{c^2} = \dfrac{h}{c\lambda}$
- 光强：$I = Nh\nu$

### 黑体与黑体辐射

**黑体**：能完全吸收照射到它上面的各种频率的电磁辐射的物体。

- **斯特藩-玻耳兹曼定律**：$M(T) = \sigma T^4$，$\sigma$ 为斯特藩常量
- **维恩位移定律**：$\lambda_m T = b$，$b$ 为维恩常量

### 普朗克能量子假设

物体的能量 $E = nh\nu$，$h$ 为普朗克常量，$h\nu$ 是最小能量子能量。

### 光电效应

**爱因斯坦光电效应方程**：

$$E_k = h\nu - W_0$$

- **红限频率**：$\nu_0 = \dfrac{W_0}{h}$，称为该金属的光电效应截止频率
- **红限波长**：$\lambda_0 = \dfrac{c}{\nu_0}$

### 康普顿散射公式

散射线波长偏移量：

$$\Delta\lambda = \lambda - \lambda_0 = \frac{h}{m_0 c}(1 - \cos\theta) = \frac{2h}{m_0 c}\sin^2\frac{\theta}{2}$$

式中 $\lambda_0$ 为入射光波长，$\lambda$ 为散射光波长，$m_0$ 为电子静止质量，$\theta$ 为散射角。

### 氢原子光谱与玻尔模型

**玻尔轨道角动量量子化条件**：

$$L = mvr = n\frac{h}{2\pi} = n\hbar, \quad n = 1, 2, 3, \cdots$$

其中约化普朗克常量：$\hbar = \dfrac{h}{2\pi}$

- 氢原子轨道半径：$r_n = r_1 n^2$
- 氢原子激发态能量：$E_n = \dfrac{E_1}{n^2}$

{%note info%}
玻尔氢原子的基态值：
- $r_1 = 0.529 \text{ Å}$（玻尔半径）
- $E_1 = -13.6 \text{ eV}$
{%endnote%}

### 海森堡不确定关系

波函数的空间分布越延展，动量不确定度越小，动量的测量越准确。

- **坐标-动量不确定关系**：$\Delta x \cdot \Delta p_x \geq \dfrac{\hbar}{2}$
- **能量-时间不确定关系**：$\Delta E \cdot \Delta t \geq \dfrac{\hbar}{2}$

### 德布罗意波

实物粒子也具有波粒二象性：

$$\lambda = \frac{h}{p} = \frac{h}{mv}$$

{%note info%}
**德布罗意波长估算**：
- 宏观物体（如 $m = 1 \text{ g}$，$v = 1 \text{ m/s}$）：$\lambda \approx 6.6 \times 10^{-31} \text{ m}$（极短，波动性可忽略）
- 电子（$m = 9.1 \times 10^{-31} \text{ kg}$，经 $100 \text{ V}$ 加速）：
  $v = \sqrt{\frac{2eV}{m}}$，$\lambda = \frac{h}{\sqrt{2meV}} \approx 0.123 \text{ Å}$

**常用公式**：电子经电压 $U$ 加速后的德布罗意波长（非相对论近似）：
$$\lambda = \frac{h}{\sqrt{2meU}} \approx \frac{1.226}{\sqrt{U}} \text{ nm} \quad (U \text{ 单位为 V})$$
{%endnote%}

{%note info%}
**例题**：铝的逸出功 $W_0 = 4.2 \text{ eV}$，用波长 $\lambda = 200 \text{ nm}$ 的紫外光照射铝表面。求：（1）光电子的最大初动能；（2）铝的红限频率和红限波长。

**解**：
（1）由光电效应方程：
$$E_k = h\nu - W_0 = h\frac{c}{\lambda} - W_0$$
$$h\frac{c}{\lambda} = \frac{6.626 \times 10^{-34} \times 3 \times 10^8}{200 \times 10^{-9}} = 9.94 \times 10^{-19} \text{ J} = 6.21 \text{ eV}$$
$$E_k = 6.21 - 4.2 = 2.01 \text{ eV}$$

（2）红限频率：$\nu_0 = \dfrac{W_0}{h} = \dfrac{4.2 \times 1.6 \times 10^{-19}}{6.626 \times 10^{-34}} = 1.01 \times 10^{15} \text{ Hz}$

红限波长：$\lambda_0 = \dfrac{c}{\nu_0} = \dfrac{3 \times 10^8}{1.01 \times 10^{15}} = 296 \text{ nm}$
{%endnote%}

{%note info%}
**例题**：在康普顿散射中，入射光波长 $\lambda_0 = 0.1 \text{ nm}$，散射角 $\theta = 90°$。求散射光的波长变化量和散射光波长。

**解**：
- 康普顿波长：$\lambda_C = \frac{h}{m_0 c} = \frac{6.626 \times 10^{-34}}{9.1 \times 10^{-31} \times 3 \times 10^8} = 2.43 \times 10^{-12} \text{ m} = 0.00243 \text{ nm}$
- 波长变化量：$\Delta\lambda = \lambda_C(1 - \cos 90°) = 0.00243 \times (1 - 0) = 0.00243 \text{ nm}$
- 散射光波长：$\lambda = \lambda_0 + \Delta\lambda = 0.1 + 0.00243 = 0.10243 \text{ nm}$
{%endnote%}
