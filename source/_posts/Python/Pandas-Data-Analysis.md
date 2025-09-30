---
title: 'Pandas: Data Analysis'
date: 2025-09-13 13:01:56
tags: [Python, Pandas]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/Pandas.png?raw=true
category: Python
category_bar: true
description: The document introduces the fundamental methods of data analysis using the pandas library, covering key operations such as reading Excel files and processing data within them.
---

{%note info%}
{%fold into @ 什么是 pip ？ %}
`pip`是 Python 的包管理工具，全称是 Python Install Packages，所有人都可以下载或上传自己开发的库到 PyPI 上，常见的指令有：
```bash
pip install pandas
```
{%endfold%}
{%endnote%}

### 一、Series对象
Series对象是pandas中可**存储多种类型的一维数组**，由**数据**与**索引**组成。
#### 默认索引为0
```python
import pandas as pd
s1=pd.Series([1,2,3,4,5])
print(s1)
```
```
0    1
1    2
2    3
3    4
4    5
```
这里就可以采用**位置**进行索引
#### 自定义索引
```python
import pandas as pd
s1=pd.Series([1,2,3,4,5],index=['a','b','c','d','e'])
print(s1)
```
```
a    1
b    2
c    3
d    4
e    5
```
这里就可以使用自定义的**标签**进行索引
***
同时上述两种方法也都可以支持**切片索引**
```python
import pandas as pd
s1=pd.Series([1,2,3,4,5],index=['a','b','c','d','e'])
print(s1['a':'d'])
```
```
a    1
b    2
c    3
d    4
```
***
我们也可以**同时获取数据与索引**
```python
import pandas as pd
s1=pd.Series([1,2,3,4,5],index=['a','b','c','d','e'])
print(s1.index)
print(s1.values)
```
```
Index(['a', 'b', 'c', 'd', 'e'], dtype='object')
[1 2 3 4 5]
```
***

### 二、DataFrame对象
```python
pandas.DataFrame(data,index,columns)
# 分别对应数据 行索引 列索引
```
#### 列表创建DataFrame对象
当对象更注重**列方向**数据关联时：
```python
import pandas as pd
data=[[110,135,125],[115,140,130],[120,145,135]]
columns=['语文','数学','英语']
df=pd.DataFrame(data=data,columns=columns)
print(df)
```
#### 字典创建DataFrame对象
当对象更注重**行方向**数据关联时：
```python
import pandas as pd
df=pd.DataFrame({
'语文':[110,115,120],
'数学':[135,140,145],
'英语':[125,130,135],
'班级':'24-3班'
})
print(df)
```
```
    语文   数学   英语     班级
0  110  135  125  24-3班
1  115  140  130  24-3班
2  120  145  135  24-3班
```

## 三、导入外部数据
### 导入 .xls 和 .xlsx 文件
```python
pandas.read_excel(file_path,sheet_name)
```
默认情况下，sheet_name参数为0，表示读取第一个工作表。
- `sheet_name=1` 表示读取第二个工作表
- `sheet_name='sheet_name'` 表示读取**指定名称的工作表**
- `sheet_name=None` 读取**所有工作表**
- `sheet_name=[1,2,'Sheet4']` 读取**指定索引或名称的工作表**

```python
import pandas as pd
df=pd.read_excel('附件.xlsx','男胎检测数据')
print(df.head()) # 输出前五行
print(df.tail()) # 输出最后五行
```
```
  序号  孕妇代码  年龄     身高    体重  ... 被过滤掉读段数的比例 染色体的非整倍体 怀孕次数  生产次数 胎儿是否健康
0   1  A001  31  160.0  72.0  ...   0.027484      NaN    1     0      是
1   2  A001  31  160.0  73.0  ...   0.019617      NaN    1     0      是
2   3  A001  31  160.0  73.0  ...   0.022312      NaN    1     0      是
3   4  A001  31  160.0  74.0  ...   0.023280      NaN    1     0      是
4   5  A002  32  149.0  74.0  ...   0.024212      NaN    2     1      否
```
#### 行/列索引
```python
import pandas as pd
df=pd.read_excel('附件.xlsx','男胎检测数据',index_col=3)
print(df.head())
print(df.tail())
# 将第四列放在首列作为行索引
```
```python
import pandas as pd
df=pd.read_excel('附件.xlsx','男胎检测数据',header=None)
print(df.head())
print(df.tail())
# 将数字作为列索引
```
#### 导入指定行/列
```python
import pandas as pd
df=pd.read_excel('附件.xlsx','男胎检测数据',usecols=[1])
print(df.head())
print(df.tail())
# 导入第二列的数据
```
- `usecols=[1,2,3]` 指定**多列进行导入**，导入第1、2、3列的数据
- `usecols='A:C'` 导入A列到C列的数据
```
   序号  孕妇代码  年龄
0   1  A001  31
1   2  A001  31
2   3  A001  31
3   4  A001  31
4   5  A002  32
```
- `usecols=['孕妇代码','年龄']` 导入**指定列名**的数据

## 四、数据清洗
### （一）缺失值处理
#### 查找缺失值
```python
import pandas as pd
df=pd.read_excel('附件.xlsx','男胎检测数据')
print(df.info())
```
{%fold into @查看运行结果%}
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1082 entries, 0 to 1081
Data columns (total 31 columns):
 #   Column        Non-Null Count  Dtype  
---  ------        --------------  -----  
 0   序号            1082 non-null   int64  
 1   孕妇代码          1082 non-null   object 
 2   年龄            1082 non-null   int64  
 3   身高            1082 non-null   float64
 4   体重            1082 non-null   float64
 5   末次月经          1070 non-null   object 
 6   IVF妊娠         1082 non-null   object 
 7   检测日期          1082 non-null   object 
 8   检测抽血次数        1082 non-null   int64  
 9   检测孕周          1082 non-null   object 
 10  孕妇BMI         1082 non-null   float64
 11  原始读段数         1082 non-null   int64  
 12  在参考基因组上比对的比例  1082 non-null   float64
 13  重复读段的比例       1082 non-null   float64
 14  唯一比对的读段数      1082 non-null   int64  
 15  GC含量          1082 non-null   float64
 16  13号染色体的Z值     1082 non-null   float64
 17  18号染色体的Z值     1082 non-null   float64
 18  21号染色体的Z值     1082 non-null   float64
 19  X染色体的Z值       1082 non-null   float64
 20  Y染色体的Z值       1082 non-null   float64
 21  Y染色体浓度        1082 non-null   float64
 22  X染色体浓度        1082 non-null   float64
 23  13号染色体的GC含量   1082 non-null   float64
 24  18号染色体的GC含量   1082 non-null   float64
 25  21号染色体的GC含量   1082 non-null   float64
 26  被过滤掉读段数的比例    1082 non-null   float64
 27  染色体的非整倍体      126 non-null    object 
 28  怀孕次数          1082 non-null   object 
 29  生产次数          1082 non-null   int64  
 30  胎儿是否健康        1082 non-null   object 
dtypes: float64(17), int64(6), object(8)
memory usage: 262.2+ KB
None
```
缺失值一般用**NaN**表示，在第三列中大部分非空数量为1082，而第二十七行为126行，说明第二十七行有缺失值。
{%endfold%}

也可以使用`isnull()`方法查看缺失值：
```python
import pandas as pd
df=pd.read_excel('附件.xlsx','男胎检测数据')
print(df.isnull())
```

{%fold into @查看运行结果%}
```
         序号   孕妇代码     年龄     身高  ...  染色体的非整倍体   怀孕次数   生产次数  胎儿是否健康
0     False  False  False  False  ...      True  False  False   False
1     False  False  False  False  ...      True  False  False   False
2     False  False  False  False  ...      True  False  False   False
3     False  False  False  False  ...      True  False  False   False
4     False  False  False  False  ...      True  False  False   False
...     ...    ...    ...    ...  ...       ...    ...    ...     ...
1077  False  False  False  False  ...     False  False  False   False
1078  False  False  False  False  ...     False  False  False   False
1079  False  False  False  False  ...      True  False  False   False
1080  False  False  False  False  ...      True  False  False   False
1081  False  False  False  False  ...      True  False  False   False
```
{%endfold%}
***
#### 删除缺失值
`dropna()`方法可以删除缺失值，默认情况下，删除所有行中**有缺失值的行**：
```python
import pandas as pd
df=pd.read_excel('附件.xlsx','男胎检测数据')
df.dropna(inplace=True)
print(df)
```
{%fold into @查看运行结果%}
未删除前：
```
[1082 rows x 31 columns]
```
删除后：
```
[126 rows x 31 columns]
```
{%endfold%}
***
#### 填充缺失值
### （二）重复值处理
#### 判断重复值
`duplicated()`方法可以判断数据是否重复，若没有传入参数，则默认会按照所有列的组合来判断重复值，即如果两行的所有列的值完全相同，则判定为重复行，返回 True

```python
import pandas as pd
df=pd.read_excel('附件.xlsx','男胎检测数据')
df=df.duplicated('年龄')
# 注意要将处理值赋给原始数据df
print(df)
```
{%fold into @查看运行结果%}
默认情况，**无参数**：
```
0       False
1       False
2       False
3       False
4       False
```
**含参数**，只判断某一列重复值：
```
0       False
1        True
2        True
3        True
4       False
```
{%endfold%}
***
#### 删除重复值
`drop_duplicates()`方法可以删除重复值，默认情况下，删除所有行中**有重复值的行**：
```python
import pandas as pd
df=pd.read_excel('附件.xlsx','男胎检测数据')
df=df.drop_duplicates('孕妇代码')
print(df)
```
```python
df=df.drop_duplicates('孕妇代码',keep='last')
```
参数**默认保留重复的第一行**，可选参数保留重复的最后一行或任意行
{%fold into @查看运行结果%}
默认情况，**无参数**：
```
        序号  孕妇代码  年龄     身高     体重  ... 被过滤掉读段数的比例 染色体的非整倍体 怀孕次数  生产次数 胎儿是否健康
0        1  A001  31  160.0  72.00  ...   0.027484      NaN    1     0      是
1        2  A001  31  160.0  73.00  ...   0.019617      NaN    1     0      是
2        3  A001  31  160.0  73.00  ...   0.022312      NaN    1     0      是
3        4  A001  31  160.0  74.00  ...   0.023280      NaN    1     0      是
4        5  A002  32  149.0  74.00  ...   0.024212      NaN    2     1      否
...    ...   ...  ..    ...    ...  ...        ...      ...  ...   ...    ...
1077  1078  A266  30  159.0  83.35  ...   0.017951      T18    1     0      是
1078  1079  A267  28  155.0  73.76  ...   0.022549      T21    1     0      是
1079  1080  A267  28  155.0  74.06  ...   0.021330      NaN    1     0      是
1080  1081  A267  28  155.0  74.74  ...   0.022013      NaN    1     0      是
1081  1082  A267  28  155.0  75.85  ...   0.016906      NaN    1     0      是

[1082 rows x 31 columns]
```
**含参数**
```
        序号  孕妇代码  年龄     身高     体重  ... 被过滤掉读段数的比例 染色体的非整倍体 怀孕次数  生产次数 胎儿是否健康
0        1  A001  31  160.0  72.00  ...   0.027484      NaN    1     0      是
4        5  A002  32  149.0  74.00  ...   0.024212      NaN    2     1      否
9       10  A003  35  160.0  78.70  ...   0.021138      T21   ≥3     1      是
15      16  A004  26  158.0  71.50  ...   0.021022      NaN   ≥3     1      是
19      20  A005  30  150.0  67.40  ...   0.025210      NaN   ≥3     1      是
...    ...   ...  ..    ...    ...  ...        ...      ...  ...   ...    ...
1062  1063  A263  30  157.0  72.34  ...   0.024222      T21    1     0      是
1066  1067  A264  30  171.0  94.95  ...   0.024366      NaN    1     0      是
1070  1071  A265  32  168.0  95.17  ...   0.025949      NaN    1     0      是
1074  1075  A266  30  159.0  81.24  ...   0.025132   T13T18    1     0      是
1078  1079  A267  28  155.0  73.76  ...   0.022549      T21    1     0      是

[267 rows x 31 columns]
```
{%endfold%}

### （三）异常值处理
#### 箱型线法
箱型线法是基于数据的**四分位数**构建 “箱体” 和 “须”，通过设定合理范围识别异常值，超过**设定的上限或下限**都可以被认定为异常值，具有**不依赖数据分布**、**对极端值鲁棒**的特点。
```python
import pandas as pd
# 读取数据
df = pd.read_excel('附件.xlsx', '男胎检测数据')
# 1. 计算Q1、Q3、IQR和上下限（同步骤1）
Q1 = df['体重'].quantile(0.25) # 下四分位数
Q3 = df['体重'].quantile(0.75) # 上四分位数
IQR = Q3 - Q1 # 四分位距
lower_bound = Q1 - 1.5 * IQR # 下异常值下限
upper_bound = Q3 + 1.5 * IQR # 上异常值上限
# 2. 筛选异常值（低于下限 或 高于上限）
outliers_lower = df[df['体重'] < lower_bound]  # 下异常值
outliers_upper = df[df['体重'] > upper_bound]  # 上异常值
all_outliers = pd.concat([outliers_lower, outliers_upper])  # 所有异常值
```
#### 删除异常值
```python
import pandas as pd

df = pd.read_excel('附件.xlsx', '男胎检测数据')
Q1 = df['体重'].quantile(0.25)
Q3 = df['体重'].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

# 筛选正常数据（排除异常值）
df_cleaned = df[(df['体重'] >= lower_bound) & (df['体重'] <= upper_bound)]
```
#### 填充异常值
```python
import pandas as pd
df = pd.read_excel('附件.xlsx', '男胎检测数据')
Q1 = df['体重'].quantile(0.25)
Q3 = df['体重'].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR
# 计算填充值（用体重列的中位数）
weight_median = df['体重'].median()
# 用中位数填充下异常值（体重 < 下限）
df.loc[df['体重'] < lower_bound, '体重'] = weight_median
# 用中位数填充上异常值（体重 > 上限）
df.loc[df['体重'] > upper_bound, '体重'] = weight_median
```
#### 盖帽处理
```python
import pandas as pd
df = pd.read_excel('附件.xlsx', '男胎检测数据')
Q1 = df['体重'].quantile(0.25)
Q3 = df['体重'].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR
# 下异常值替换为下限
df.loc[df['体重'] < lower_bound, '体重'] = lower_bound
# 上异常值替换为上限
df.loc[df['体重'] > upper_bound, '体重'] = upper_bound
```