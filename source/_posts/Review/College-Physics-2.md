---
title: College Physics 2
date: 2025-09-02 21:11:58
tags: [College Physics]
category: Review
category_bar: true
archive: true
math: true
---

## 六、热力学基础

### (I)物态方程
理想气体的**物态方程**

$$PV=\frac{m}{M_{mol}}RT$$

其中，$\frac{m}{M}$ 叫做物质的量，$R=8.31J/(mol\cdot K)$ 叫做普适气体常量

上述式子也可以写成：$$P=nkT$$

其中，$n=\frac{N}{V}$为单位体积分子数，$k=1.38 \times 10^{-23} J/K$为玻尔兹曼常数

***

### (II) 分压定律
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

### (III)能量均分定理、内能
#### 气体分子
- **平均平动动能**：$\overline{\varepsilon}=\frac{3}{2}kT$
- **内能**：$\overline{\varepsilon}=\frac{i}{2}kT$

其中：$k = \frac{R}{N_A}$

| 分子类型 | 自由度 $i$ | 平动动能 $\overline{\varepsilon}_\text{平动}$ | 转动动能 $\overline{\varepsilon}_\text{转动}$ | 总动能（内能） $\overline{\varepsilon}_\text{总}$ |
|:----------:|:---------:|:--------:|:------------:|:--------:|
| 单原子 | $ 3（3+0）$ | $ \frac{3}{2}kT $ | $ 0 $ | $ \frac{3}{2}kT $ |
| 双原子 | $ 5（3+2）$ | $ \frac{3}{2}kT $ | $ \frac{2}{2}kT $ | $ \frac{5}{2}kT $ |
| 多原子 | $ 6（3+3）$ | $ \frac{3}{2}kT $ | $ \frac{3}{2}kT $ | $ \frac{6}{2}kT $ |


#### 气体
- 动能：$E= \frac{3}{2}\frac{m}{M}RT $
- 内能：$E= \frac{i}{2}\frac{m}{M}RT $

### (IV)麦克斯韦速率分布
速率分布曲线**归一化**处理：
$$\int_{0}^{\infty} f(v)  dv = 1$$

- 平均速率：$\overline{\nu} = \int_{0}^{\nu_o} vf(v)  dv=\sqrt{\frac{8RT}{\pi M}}$

- 方均根速率：$\overline{\nu^2} = \int_{0}^{\infty} v^2 f(\nu)  d\nu$、$\sqrt{\overline{\nu^2}}=\sqrt{\frac{3RT}{M}}$

- 最概然速率：$v_p=\sqrt{\frac{2RT}{M}}$

### (V)分子碰撞与自由程

#### 平均碰撞频率
$$\overline{Z}=\sqrt{2} \pi d^2 \overline{\nu} n$$

#### 平均自由程
$$\overline{\lambda} = \frac{\overline{v}}{\overline{Z}} = \frac{1}{\sqrt{2} \pi d^2 n}$$


| 过程   | 特征| 过程方程  | 吸放热量 | 对外做功 | 内能增量 |
|:----------:|:----------:|:----------:|:----------:|:---------:|:----------:|
| 等容  | $V=$常量| $\frac{p}{T}=$常量                | $\frac{m}{M}C_v(T_2 - T_1)$ <br>$\frac{m}{M}\frac{i}{2}R(T_2 - T_1)$ |$0$| $\frac{m}{M}\frac{i}{2}R(T_2 - T_1)$ |
| 等压   |$p=$常量| $\frac{V}{T}=$常量 | $\frac{m}{M}C_p(T_2 - T_1)$ <br> $\frac{m}{M}\frac{i + 2}{2}R(T_2 - T_1)$ | $p(V_2 - V_1)$ <br> $\frac{m}{M}R(T_2 - T_1)$  | $\frac{m}{M}\frac{i}{2}R(T_2 - T_1)$ |
| 等温   |$T=$常量| $pV=$常量 | $\frac{m}{M}RT\ln\frac{V_2}{V_1}$ <br> $\frac{m}{M}RT\ln\frac{P_1}{P_2}$   | $\frac{m}{M}RT\ln\frac{V_2}{V_1}$ <br> $\frac{m}{M}RT\ln\frac{P_1}{P_2}$ | $0$ |
| 绝热   |$Q=0$|$pV^\gamma=$常量 <br> $V^{\gamma - 1}T=$常量 <br> $p^{\gamma - 1}T^{-\gamma}=$常量 |$0$|$-\frac{m}{M}\frac{i}{2}R(T_2 - T_1)$ | $\frac{m}{M}\frac{i}{2}R(T_2 - T_1)$ |

- 绝热的曲线是最陡峭的

$C_{v,m}$ 1mol气体在体积不变的情况下，温度改变1k所吸收或者放出的热量

$C_{p,m}=C_{v,m}+R=\frac{i}{2}R+R$，即1mol气体升高1k，在等压过程中比等容过程多吸收8.31J能量



#### 卡诺循环
两个等温+两个绝热循环（为理论最大效率）：
$$\eta = \frac{A}{Q_1} = \frac{Q_1-Q_2}{Q_1} \leq 1 - \frac{T_2}{T_1}$$

#### 制冷机
卡诺制冷机制冷系数：$$w = \frac{Q_2}{A} = \frac{T_2}{T_1 - T_2}$$
其中，$Q_2$为从低温物体（冷藏室）吸收的热量；$A$为制冷机消耗的功；$T_2$为低温物体的热力学温度；$T_1$为高温物体的热力学温度


## 十、机械振动和电磁震荡
### (I) 简谐振动
线性回复力：物体所受到的合外力大小总是与物体离开平衡距离的位移大小成正比且大小相反（用于证明为简谐振动）

$$\frac{d^2x}{dt} + \omega^2 x = 0$$

有：

$$\omega=\sqrt{\frac{k}{m}}$$

（注意对弹簧“串并联”情况下的 $k$进行分析）
### (II) 简谐振动的能量
总能量：

$$E=\frac{1}{2}kA^2$$
$$E = E_k + E_p = \frac{1}{2}mv^2 + \frac{1}{2}kx^2$$

### (III) 简谐振动的合成
$$x_1=A_1cos(\omega t+ \phi_1)$$
$$x_2=A_2cos(\omega t+ \phi_2)$$
合位移为两个位移的代数和：（若题目中一个为$sin$需要化成$cos$的形式）
$$x=x_1+x_2=A_1cos(\omega t+ \phi_1)+A_2cos(\omega t+ \phi_2)$$

我们将其化为：
$$x=Acos(\omega t+ \phi)$$

计算出结果：
$$A = \sqrt{A_1^2 + A_2^2 + 2A_1A_2 \cos(\phi_1 - \phi_2)}$$

$$tan(\phi _0)=\frac{A_1sin(\phi_1)+A_2sin(\phi_2)}{A_1cos(\phi_1)+A_2cos(\phi_2)}$$

若是求表达式，最后 $\phi _0$ 用反三角函数进行简化：
$$\phi _0=arctan(tan(\phi _0))$$

## 十一、机械波和电磁波
波动方程：
$$y(x,t) = A{\cos\left[\omega\left(t + \frac{x}{v}\right) + \phi\right]}$$ 

- **波腹**：振幅最大的位置
- **波节**：振幅为0的位置

反射：自由端反射时，无半波损失（相位突变$\pi$）

- 波动的能量：能量不守恒，动能和势能同步变化在平衡位置处达到最大
- 弹簧振子的能量：能量守恒，动能和势能相互转换

  
## 十二、光学
获得相干光的的方法：
1. 分波阵面法（两个双缝）
2. 分振幅法（上表面和下表面的反射光）

光程=折射率$\times$路径长=$n\times r$
光程差：$\Delta r = n_2\times r_2 - n_1\times r_1$
相位差：$\Delta \phi = \phi_2 - \phi_1 = 2\pi \frac{\Delta r}{\lambda}$


### (I) 干涉
#### 双缝干涉
杨氏双缝干涉：间距相等的干涉条纹
- 双缝间距为$d$
- 距离双缝$D$
- 到0级明纹的距离$x$
$\delta = r_2 - r_1 = \frac{d x}{D}$

$\delta = $
- $\pm k\lambda  \text{明纹} \quad (k=0,1,2,\dots)$
- $\pm(2k+1)\dfrac{\lambda}{2}  \text{暗纹} \quad (k=0,1,2,\dots)$

#### 等倾干涉
一块玻璃板，上表面和下表面的反射光经过透镜形成的内疏外密的圆环，内小（$k=0$）外大
$$\delta = 2nd\cos\gamma+(\frac{\lambda}{2})$$

其中，$n$为被射入介质的折射率，$d$为玻璃片厚度，$\gamma$为折射角，$\frac{\lambda}{2}$为半波损失：
- 只在反射面产生
  - 光疏到光密有半波损失
  - 光密到光疏无半波损失  

透射增强：反射相消
可见光：400nm-760nm

#### 劈尖干涉
相邻两明（暗）条纹高度差：$\delta h=\frac{\lambda}{2n}$
相邻两明（暗）条纹间距：$l\sin\theta=\frac{\lambda}{2n}$

#### 牛顿环
光程差：$\delta=2ne+\frac{\lambda}{2}$
由于公式推导：$(R-e)^2+r^2=R^2$
明纹半径：$r=\sqrt{\frac{(2k-1)R\lambda}{2n}}$
暗纹半径：$r=\sqrt{\frac{kR\lambda}{n}}$

#### 迈克尔逊干涉仪
$d=N\frac{\lambda}{2}$

### (II)衍射
#### 单缝衍射
$\delta =a\sin\theta $
- $\pm k\lambda  \text{暗纹} \quad (k=0,1,2,\dots)$
- $\pm(2k+1)\dfrac{\lambda}{2}  \text{明纹} \quad (k=0,1,2,\dots)$

其中，$a$为单缝宽度，$\theta$为衍射角

缝宽：$\Delta x = 2f\theta = \frac{a}{2f\lambda}$（$f$为透镜焦距）
​

#### 光栅衍射
- 光栅方程：
  $$(a+b)\sin\theta=\pm k\lambda（明纹）$$

其中，$a$为单缝宽度，$b$为缝间距

- 光栅常数：$d=a+b$
- 缺级：当**光栅衍射的主极大位置与单缝衍射的暗纹位置重合**时，该主极大会消失，公式：$k=\frac{a+b}{a}k'$

（$d$可根据单位长度光栅条纹数量进行计算）


### (III) 偏振光
#### 马吕斯定律
- 穿过第一个偏振片：$I_1=\frac{1}{2}I_0$
- 穿过第二个偏振片：$I_2={\cos\theta}^2 I_1$

#### 布儒斯特定律
- 反射光为完全偏振光
- 折射光（透射光）为部分偏振光
- 反射光与偏振光垂直：$i_\beta+\gamma=\frac{\pi}{2}$
- $\tan\theta=\frac{n_2}{n_1}$

## 十三、量子力学

### (I) 光子基本物理量
- 光子能量：$E=h\nu=h\frac{c}{\lambda}$
- 光子动量：$p=h\frac{\nu}{c}=\frac{h}{\lambda}$
- 光子质量：$m=h\frac{\nu}{c^2}=\frac{h}{c\lambda}$
- 光强：$I=Nh\nu$

### (II) 黑体与黑体辐射
**黑体**：能完全吸收照射到它上面的各种频率的电磁辐射的物体。

- 斯特藩——玻耳兹曼定律：$M(T) = \sigma T^{4}$，$\sigma$ 为常数。
- 维恩位移定律：$\lambda_{m}T=b$，$b$ 为常数。

### (III) 普朗克能量子假设
普朗克能量子假设：物体的能量 $E = nh\nu$，$h$ 为普朗克常量，$h\nu$ 是最小能量子能量。


### (IV) 光电效应

$$E_k=h\nu_k-W_0$$

- 红限频率：$\nu_{0}$ 称为该金属的光电效应截止频率，也称为红限频率。

- 红限波长：对应的波长可称为红限波长，也可称为红限，常用 $\lambda_{0}$ 表示。

### (V) 康普顿散射公式
散射线波长偏移量：
$$\Delta \lambda=\lambda-\lambda_{0}=\frac{h}{m_{0}c}(1-\cos \theta)=\frac{2h}{m_{0}c}\sin ^{2}\frac{\theta}{2}$$

式中 $\lambda_{0}$ 为入射光波长，$\lambda$ 为散射光波长，$m_{0}$ 为电子静止质量，$\theta$ 为散射角

### (VI) 氢原子光谱与玻尔模型
- 玻尔轨道角动量量子化条件：

$$L=mvr=n \frac{h}{2\pi}=n\hbar,\quad n=1,2,3,\cdots$$
  - 约化普朗克常量：$\hbar=\frac{h}{2\pi}$
- 氢原子轨道半径：$r_{n}=r_{1}n^{2}$
- 氢原子激发态能量：$E_{n}=\frac{E_{1}}{n^{2}}$



## (VII) 海森堡不确定关系

- 坐标-动量不确定关系：$\Delta x\cdot \Delta p_{x}\ge \frac{\hbar}{2}$
- 能量-时间不确定关系：$\Delta E\Delta t \ge \frac{\hbar}{2}$






