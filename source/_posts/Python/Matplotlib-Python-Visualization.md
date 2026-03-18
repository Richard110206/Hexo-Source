---
title: Matplotlib:Python Visualization
date: 2025-09-13 09:52:54
tags: [updating]
category: Python
category_bar: true
archive: true
---

### 折线图

```python
import matplotlib.pyplot as plt 
import numpy as np

plt.style.use('default')

x=[1,2,3,4,5]
y=[2,4,6,8,10]

plt.plot(x,y)
plt.plot(y,x)
plt.xlabel("X Axis")
plt.ylabel("Y Axis")
plt.title("Simple Line Plot")
plt.show()
```

```python
import matplotlib.pyplot as plt 
import numpy as np

plt.style.use('default')

x=np.linspace(0,10,100)
y=np.sin(x)

plt.plot(x,y)
plt.xlabel("X Axis")
plt.ylabel("Y Axis")
plt.title("y=sin(x)")
plt.show()
```

```python
import matplotlib.pyplot as plt 
import numpy as np

x=np.linspace(0,10,100)
y1=np.sin(x)
y2=np.cos(x)
y3=np.sin(x)*np.cos(x)

plt.figure(figsize=(10,6))
plt.plot(x,y1,label="sin(x)")
plt.plot(x,y2,label="cos(x)")
plt.plot(x,y3,label="sin(x)*cos(x)")
plt.xlabel("X Axis")
plt.ylabel("Y Axis")
plt.title("Mutiple Functions")
plt.legend()
plt.grid(True,alpha=0.3)
plt.show()
```

我们可以通过一个简洁的格式字符串 `[color][marker][line]`来按顺序组合颜色、标记和线型。

```python
import matplotlib.pyplot as plt 
import numpy as np

x=np.linspace(0,10,20)
plt.figure(figsize=(10,6))
plt.plot(x,x,"b-",label="Solid line",linewidth=2)
plt.plot(x,x+1,"r--",label="Dashed line",linewidth=2)
plt.plot(x,x+2,"g-",label="Dash-dot line",linewidth=2)
plt.plot(x,x+3,"m:",label="Dotted line",linewidth=2)
plt.plot(x,x+4,"co-",label="Circles",linewidth=2)
plt.plot(x,x+5,"s-",label="Squares",linewidth=2)
plt.plot(x,x+6,"^-",label="Triangles",linewidth=2)

plt.xlabel("X Axis")
plt.ylabel("Y Axis")
plt.title("Line Styles and Markers")
plt.legend()
plt.grid(True,alpha=0.3)
plt.show()
```

#### 颜色

| 符号 |       颜色        |
| :--: | :---------------: |
| `b`  |   blue（蓝色）    |
| `r`  |    red（红色）    |
| `g`  |   green（绿色）   |
| `c`  |   cyan（青色）    |
| `m`  | magenta（品红色） |
| `y`  |  yellow（黄色）   |
| `k`  |   black（黑色）   |

#### 标记

| 符号 |      标记样式      |
| :--: | :----------------: |
| `o`  |   circle（圆圈）   |
| `s`  |  square（正方形）  |
| `^`  | triangle（三角形） |
| `*`  |    star（星形）    |
| `+`  |    plus（加号）    |
| `x`  |     x（叉号）      |

#### 线型

| 符号 |        线型        |
| :--: | :----------------: |
| `-`  |   solid（实线）    |
| `--` |   dashed（虚线）   |
| `-.` | dash-dot（点划线） |
| `:`  |   dotted（点线）   |

```python
import matplotlib.pyplot as plt 
import numpy as np

x=np.linspace(0,10,100)
plt.figure(figsize=(12,8))
plt.plot(x,np.sin(x),color="steelblue",linewidth=2,label="strrlblue")
plt.plot(x,np.cos(x),color="#FF6B6B",linewidth=2,label="#FF6B6B(Hex)")
plt.plot(x,np.sin(x)*np.cos(x),color="darkgreen",linestyle="--",label="darkgreen dashed")


plt.xlabel("X Axis",fontsize=12)
plt.ylabel("Y Axis",fontsize=12)
plt.title("Custom Colors and Styles",fontsize=14,fontweight="bold")
plt.legend(fontsize=12)
plt.grid(True,alpha=0.3)
plt.show()
```

### 散点图

```python
import matplotlib.pyplot as plt 
import numpy as np

np.random.seed(1)
x=np.random.randn(200)
y=2*x+np.random.randn(200)*0.5

plt.figure(figsize=(8,6))
plt.scatter(x,y,alpha=0.6)
plt.xlabel("X values")
plt.ylabel("Y values")
plt.title("Scatter plot")
plt.legend(fontsize=12)
plt.grid(True,alpha=0.3)
plt.show()
```

```python
import matplotlib.pyplot as plt 
import numpy as np

np.random.seed(1) # 随机数种子保证可复现
x=np.random.randn(100) # 生成100个标准正态分布的随机数
y=np.random.randn(100)
colors=np.random.rand(100) # [0,1)区间的均匀分布随机数
sizes=1000*np.random.rand(100)

plt.figure(figsize=(10,6))
plt.scatter(x,y,c=colors,s=sizes,alpha=0.6,cmap="plasma") # 对之前的[0,1)区间的颜色随机数进行“编译”
plt.colorbar(label="Color intensity")
plt.xlabel("X values")
plt.ylabel("Y values")
plt.title("Scatter Plot with Color and Size Variations",fontsize=12)
plt.legend(fontsize=12)
plt.grid(True,alpha=0.3)
plt.show()
```

### 柱状图

```python
import matplotlib.pyplot as plt 
import numpy as np

categories=['A','B','C','D','E']
values=[12,23,34,45,56]

plt.figure(figsize=(10,6))
plt.bar(categories,values,color="steelblue",edgecolor="black",linewidth=1.2) 
# plt.barh(categories,values,color="steelblue",edgecolor="black",linewidth=1.2) 翻转方向
plt.xlabel("categories")
plt.ylabel("values")
plt.title("Simple Bar Chart",fontsize=12)
plt.legend(fontsize=12)
plt.grid(True,alpha=0.3)
plt.show()
```

```python
import matplotlib.pyplot as plt 
import numpy as np
import pandas as pd

categories=['Q1','Q2','Q3','Q4']
product_a=[20,35,30,35]
product_b=[25,32,34,20]
product_c=[30,25,30,25]

x=np.arange(len(categories)) # 生成「从 0 开始、步长为 1」的连续整数序列，直到不超过传入的数值为止（左闭右开）
width=0.25 # 柱状图的宽度

plt.figure(figsize=(10,6))
plt.bar(x-width,product_a,width,label="Product A",color="#FF6B6B") # 偏移量
plt.bar(x,product_b,width,label="Product B",color="#4EDDC4") 
plt.bar(x+width,product_c,width,label="Product C",color="#95E1D3") 

plt.xlabel("Quarter")
plt.ylabel("Sales")
plt.title("Quarterly Sales Comparison")
plt.xticks(x,categories)
plt.legend()
plt.grid(True,alpha=0.3)
plt.show()
```

### 直方图

```python
import matplotlib.pyplot as plt 
import numpy as np

np.random.seed(1)
data=np.random.normal(100,15,1000)
# 生成1000个均值100、标准差15的正态分布随机数

plt.figure(figsize=(10,6))
plt.hist(data,bins=30,color="skyblue",edgecolor="black",alpha=0.7) # bins 就是直方图的柱形数量 / 分组区间数
plt.xlabel("Value")
plt.ylabel("Frequency")
plt.title("Histogram of Normal Distribution")
plt.grid(True,alpha=0.3)
plt.show()
```

```python
import matplotlib.pyplot as plt 
import numpy as np

np.random.seed(1)
data1=np.random.normal(100,15,1000)  
data2=np.random.normal(120,20,1000)

plt.figure(figsize=(10,6))
plt.hist(data1,bins=30,alpha=0.7,label="Dataset 1",color="blue")
plt.hist(data2,bins=30,alpha=0.6,label="Dataset 2",color="red")
plt.xlabel("Value")
plt.ylabel("Frequency")
plt.title("Overlapping Histograms")
plt.grid(True,alpha=0.3)
plt.legend()
plt.show()
```

### 组合子图

```python
import matplotlib.pyplot as plt 
import numpy as np

x=np.linspace(0,10,100)
fig,axes=plt.subplots(2,2,figsize=(12,10))
fig.suptitle("Mutiple Subplots",fontsize=16,fontweight="bold")

axes[0,0].plot(x,np.sin(x),"b-",linewidth=2)
axes[0,0].set_title("Sine Wave")
axes[0,0].grid(True,alpha=0.3)

axes[0,1].plot(x,np.cos(x),"r-",linewidth=2)
axes[0,1].set_title("Cosine Wave")
axes[0,1].grid(True,alpha=0.3)

np.random.seed(1)
x_scatter=np.random.randn(100)
y_scatter=np.random.randn(100)
axes[1,0].scatter(x_scatter,y_scatter,alpha=0.6,c=x_scatter,cmap="viridis")
axes[1,0].set_title("Scatter Plot")
axes[1,0].grid(True,alpha=0.3)

categories=["A","B","C","D","E"]
values=[12,23,34,45,56]
axes[1,1].bar(categories,values,color="coral")
axes[1,1].set_title("Bar Chart")
axes[1,1].grid(True,alpha=0.3,axis="y")

plt.tight_layout() # 自动计算并调整位置和间距
plt.show()
```

### 饼图

```python
import matplotlib.pyplot as plt 
import numpy as np

categories=["North","Sorth","East","West"]
sizes=[30,25,20,25]
colors=["#FF6B6B","#4ECDC4","#95E1D3","#F38181"]
explode=(0.1,0,0,0) # 突出的范围

fig,ax=plt.subplots(figsize=(8,8))
ax.pie(sizes,explode=explode,labels=categories,colors=colors,autopct="%1.1f%%",shadow=True,startangle=90)
ax.set_title("Sales Distribution by Region",fontsize=14,fontweight="bold")
plt.show()
```

