---
title: Basic Python Tutorial
date: 2025-09-13 10:45:52
tags:
category: Python
category_bar: true
archive: true
---
最近决心入门Python，之前仅仅高中的底子显然不够看，遂再来进行学习！

## 一、数据结构
### （一）列表
可以存储**任意类型**的数据，**动态改变大小**，类似于CPP中的`vector`，用方括号`[]`表示，元素之间用逗号隔开。

```python
my_list=[1,2,3,4,5]
```

```python
my_list=[1,str,1.2,'hello']
print(my_list)
# [1, <class 'str'>, 1.2, 'hello']
```


#### 添加元素
`append()`方法
```python
my_list.append(6)
my_list.append('world')
```
#### 修改元素
直接通过索引值进行修改
```python
my_list[0]=100
```
#### 删除元素
`del`关键字
```python
del my_list[0]
```


### （二）元组
元组与列表类似，但是元组是**不可变的**，用圆括号`()`表示，元素之间用逗号隔开。
```python
my_tuple=(1,1.2,'hello')
```


### （三）字典
字典`dict`，字典中的元素是**键值对**，类似与CPP中的`unordered_map`，用花括号`{}`表示，键-值对之间用**冒号**分隔，元素之间用**逗号**分隔
```python
my_dict={'name':'richard','age':18,'city':'Nanjing'}
print(my_dict['name']) # 输出键为'name'的值：richard
print(my_dict['age']) # 输出键为'age'的值：18
print(my_dict['city']) # 输出键为'city'的值：Nanjing
```


### （四）集合
集合`set`，集合中的元素是**不重复且无序**的，用花括号`{}`表示，元素之间用逗号隔开。
```python
my_set={1,2,3,4,5,5,5}
```
#### 添加元素
`add()`方法
```python
my_set.add(6)
```
#### 删除元素
`remove()`方法
```python
my_set.remove(1)
```
### （五）字符串
字符串`str`，用引号`''`或`""`表示，引号中的内容就是字符串的内容。
```python
my_string='hello world'
```
## 二、序列
序列可以帮助我们方便的存储和访问多个元素：
### 索引(index)
索引`index`，索引用于访问序列中的元素，索引从0开始。
- **正整数**表示从前往后数
- **负整数**表示从后往前数
```python
my_string='hello world'
print(my_string[0]) # 输出第一个字符：h
print(my_string[-1]) # 输出最后一个字符：d
```
### 切片(slice)
获取序列中的子序列，用**冒号**分隔，**左闭右开**，通用格式为：

$$[start:end] 、 [start: end:step]$$
- **start**表示起始索引，默认为0
- **end**表示结束索引，默认为序列长度
- **step**表示**步长**，默认为1
```python
my_string='hello world'
print(my_string[0:5]) # 输出从索引0到索引4的子序列：hello
```

### 长度（length）
```python
my_list=[1,2,3,4,5]
print(len(my_list)) # 输出列表的长度：5
```
### 追加（append）
```python
my_list.append(6)
```

### 拼接（concatenate）
```python
my_list1=[1,2,3]
my_list2=[4,5,6]
new_list=my_list1+my_list2
my_list1.append(my_list2)
```
### 删除（delete）
```python
del my_list[0]
```
### 重复（repeat）
```python
my_list=[1,2,3,4,5]
my_list*2
```
### 查找（find）
```python
my_list=[1,2,3,4,5]
print(my_list.index(3)) # 输出3的索引：2
```
### 计数（count）
 ```python
 my_list=[1,2,3,4,5]
 print(my_list.count(1)) # 输出1的个数：1
```
### 排序（sort）
```python
my_list=[3,1,5,2,4]
my_list.sort()
print(my_list) # 输出排序后的列表：[1, 2, 3, 4, 5]
```
## 三、程序控制语句
### 条件语句
```python
if condition:
    do something
elif another_condition:
    do something else
else:
    do something else
```
### for 循环语句
#### 列表
```python
fruits=['apple','banana','orange']
for fruit in fruits:
    print(fruit)
```
#### 字符串
```python
for char in 'hello':
    print(char)
```
#### range()范围函数
```python
for i in range(10):
    print(i)
```
#### 字典
```python
person={'name':'richard','age':18,'city':'Nanjing'}
for key,value in person.items():
    print(key,value)
```
### 其他语句
#### with 语句
with语句用于简化资源的管理，能在代码块执行完毕后自动关闭资源。
```python
with open('file.txt','r') as f:
    data=f.read()
    print(data)
```
#### try 语句
常用于**捕获和处理可能发生的异常**。
- try: 可能出现异常的代码
- except: 特定类型的异常
- else: 未发生异常的情况
- finally: 无论是否发生异常都会执行
```python
try:
    result=10/0
except:
    print('发生异常')
else:
    print('没有发生异常')
finally:
    print('finally')
```
