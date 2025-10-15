---
title: Basic Python Tutorial
date: 2025-09-13 10:45:52
tags:
category: Python
category_bar: true
archive: true
---

## I/O
我们用输入做一个简单的小游戏
```python
# Madlibs game
# Word game where you create a story
# by filling in blanks with random words

adjective1=input("Enter an adjective(description): ")
noun1=input("Enter a noun(person,place,thing): ")
adjective2=input("Enter an adjective(description): ")
verb1=input("Enter a verb ending with 'ing': ")
adjective3=input("Enter an adjective(description): ")

print(f"Today I went to a {adjective1} zoo")
print(f"In an exhibit,I saw a {noun1}")
print(f"{noun1} was {adjective2} and {verb1}")
print(f"I was {adjective3}!")
```
`f` 是 Python 中的 “格式化字符串前缀”，可以直接在**字符串中嵌入变量、表达式**，并让 Python **自动计算并替换其值**。

```text
Today I went to a dangerous zoo
In an exhibit,I saw a Richard
Richard was wonderful and fucking
I was joyful!
```
Python 中支持使用**逻辑符号** `and`、`or`、`not` 来进行逻辑判断：
```python
sunny=true
temperature = int(input("Enter the temperature: "))
if temperature > 30 and temperature < 40:
    print("It's hot outside")
elif temperature < 30 or not sunny:
    print("It's cold outside")
else:
    print("It's not hot outside")
```

### String in Python
相比于 C/C++ 中，在Python中，字符串在使用上相对来说更加灵活，方法也更丰富。

#### 1.大小写转换
- `str.capitalize()`：将字符串的第一个字符大写，其余字符小写。
```python
name = "hello world"
print(name.capitalize())  # 输出: Hello world
```

- `str.casefold()`：将字符串转换为小写，支持更多语言（如德语）。
```python
name = "HELLO WORLD"
print(name.casefold())  # 输出: hello world
```

- `str.lower()`：将字符串转换为小写。
```python
name = "HELLO WORLD"
print(name.lower())  # 输出: hello world
```
- `str.upper()`：将字符串转换为大写。
```python
name = "hello world"
print(name.upper())  # 输出: HELLO WORLD
```

- `str.swapcase()`：将字符串中的大小写互换。
```python
name = "Hello World"
print(name.swapcase())  # 输出: hELLO wORLD
```

- `str.title()`：将字符串中每个单词的首字母大写。
```python
name = "hello world"
print(name.title())  # 输出: Hello World
```

#### 2.查找与替换

- `str.find(sub[, start[, end]])`：查找子字符串，返回第一次出现的索引，未找到返回 -1。

```python
name = "hello world"
print(name.find("world"))  # 输出: 6
```

- `str.rfind(sub[, start[, end]])`：从右向左查找子字符串，返回第一次出现的索引，未找到返回 -1。
```python
name = "hello world"
print(name.rfind("o"))  # 输出: 7
```

- `str.replace(old, new[, count])`：替换字符串中的子字符串。
```python
name = "hello world"
print(name.replace("world", "python"))  # 输出: hello python
```
#### 3.判断
- `str.isalnum()`：判断字符串是否只包含字母和数字。
```python
name = "hello123"
print(name.isalnum())  # 输出: True
```

- `str.isalpha()`：判断字符串是否只包含字母。
```python
name = "hello"
print(name.isalpha())  # 输出: True
```

- `str.isdigit()`：判断字符串是否只包含数字。
```python
num = "123"
print(num.isdigit())  # 输出: True
```

- `str.islower()`：判断字符串是否全部为小写。
```python
name = "hello"
print(name.islower())  # 输出: True
```
- `str.isupper()`：判断字符串是否全部为大写。
```python
name = "HELLO"
print(name.isupper())  # 输出: True
```

#### 4.统计
- `str.count(sub[, start[, end]])`：统计子字符串出现的次数。
```python
name = "hello world"
print(name.count("o"))  # 输出: 2
```
# DataStructures
## (I)List
可以存储**任意类型**的数据，**动态改变大小**，类似于 CPP 中的`vector`动态数组，用方括号`[]`表示，元素之间用逗号隔开。

#### 1.列表的创建
- 使用方括号 `[]`创建列表。
- 使用 `list()` 函数将其他可迭代对象转换为列表。
```python
my_list=[1,str,1.2,'hello']
print(my_list)
# [1, <class 'str'>, 1.2, 'hello']
```
```python
# 创建空列表
empty_list = []
# 创建包含元素的列表
fruits = ["apple", "banana", "cherry"]
# 使用 list() 函数创建列表
numbers = list(range(5))  # 输出: [0, 1, 2, 3, 4]
```
#### 2.访问列表元素
- 使用索引访问列表元素（索引从 0 开始）。
- 使用负数索引从列表末尾访问元素。
```python
fruits = ["apple", "banana", "cherry"]
print(fruits[0])   # 输出: apple
print(fruits[-1])  # 输出: cherry
```

#### 3.修改列表元素
- 通过索引直接修改列表元素
```python
fruits = ["apple", "banana", "cherry"]
fruits[1] = "blueberry"
print(fruits)  # 输出: ['apple', 'blueberry', 'cherry']
```
#### 4.其他操作列表的方法
**添加元素**    
- `append()`：在列表末尾添加一个元素。
```
fruits = ["apple", "banana"]
fruits.append("cherry")
print(fruits)  # 输出: ['apple', 'banana', 'cherry']
```

- `extend()`：将另一个可迭代对象的元素添加到列表末尾。
```python
fruits = ["apple", "banana"]
fruits.extend(["cherry", "orange"])
print(fruits)  # 输出: ['apple', 'banana', 'cherry', 'orange']
```

- `insert()`：在指定位置插入一个元素。
```python
fruits = ["apple", "banana"]
fruits.insert(1, "cherry")
print(fruits)  # 输出: ['apple', 'cherry', 'banana']
```

**删除元素**
- `remove()`：删除列表中第一个匹配的元素。
```python
fruits = ["apple", "banana", "cherry"]
fruits.remove("banana")
print(fruits)  # 输出: ['apple', 'cherry']
```

- `pop()`：删除并返回指定位置的元素（默认删除最后一个元素）。
```python
fruits = ["apple", "banana", "cherry"]
fruits.pop(1)  # 删除索引为 1 的元素
print(fruits)  # 输出: ['apple', 'cherry']
```

- `clear()`：清空列表。
```python
fruits = ["apple", "banana", "cherry"]
fruits.clear()
print(fruits)  # 输出: []
```
**查找元素**
- `index()`：返回指定元素的索引（如果存在）。
```python
fruits = ["apple", "banana", "cherry"]
print(fruits.index("banana"))  # 输出: 1
```
- `count()`：返回指定元素在列表中出现的次数。
```
fruits = ["apple", "banana", "cherry", "banana"]
print(fruits.count("banana"))  # 输出: 2
```

**排序与反转**
- `sort()`：对列表进行排序（默认升序）。
```
numbers = [3, 1, 4, 1, 5, 9]
numbers.sort()
print(numbers)  # 输出: [1, 1, 3, 4, 5, 9]
```
- `reverse()`：反转列表中的元素。
```python
fruits = ["apple", "banana", "cherry"]
fruits.reverse()
print(fruits)  # 输出: ['cherry', 'banana', 'apple']
```
- `sorted()`：对列表进行临时排序
```python
fruits = ["banana","apple", "cherry"]
print(sorted(fruits))
print(fruits)
'''
['apple', 'banana', 'cherry']
['banana', 'apple', 'cherry']
'''
```
**列表的遍历**
使用 for 循环遍历列表。
```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```
**列表的切片**
使用切片操作获取列表的子集。
语法：list[start:stop:step]
```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(numbers[2:5])    # 输出: [2, 3, 4]
print(numbers[::2])    # 输出: [0, 2, 4, 6, 8]
print(numbers[::-1])   # 输出: [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```
**列表的复制**
使用切片或 `copy()` 方法复制列表。

```python
fruits = ["apple", "banana", "cherry"]
fruits_copy = fruits[:]  # 使用切片复制
fruits_copy2 = fruits.copy()  # 使用 copy() 方法复制
```

**列表推导式**
使用列表推导式快速生成列表。
```python
# 生成平方数列表
squares = [x**2 for x in range(10)]
print(squares)  # 输出: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# 生成偶数列表
evens = [x for x in range(10) if x % 2 == 0]
print(evens)  # 输出: [0, 2, 4, 6, 8]
```

**列表的嵌套**
列表可以包含其他列表，形成嵌套列表。
```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
print(matrix[1][2])  # 输出: 6
```

**列表的其他操作**
- `len()`：获取列表的长度。
```python
fruits = ["apple", "banana", "cherry"]
print(len(fruits))  # 输出: 3
in 和 not in：检查元素是否在列表中。
```python
fruits = ["apple", "banana", "cherry"]
print("banana" in fruits)  # 输出: True
+ 和 *：列表的拼接和重复。
```
```python
list1 = [1, 2, 3]
list2 = [4, 5, 6]
print(list1 + list2)  # 输出: [1, 2, 3, 4, 5, 6]
print(list1 * 2)      # 输出: [1, 2, 3, 1, 2, 3]
```
## (II) Tuple 

在 Python 中，元组（Tuple） 是一种不可变的序列类型，用于存储一组有序的元素。元组与列表（list）类似，但元组一旦创建，其内容不可修改。

- **不可变性**：元组一旦创建，其元素不能被修改、添加或删除。
- **有序性**：元组中的元素是有序的，可以通过索引访问。
- **异构性**：元组可以存储不同类型的元素（如整数、字符串、列表等）。
- **支持嵌套**：元组可以嵌套其他元组或列表。
- 元组使用圆括号 () 定义，元素之间用逗号 , 分隔。

#### 元组的创建
```python
# 创建一个元组
my_tuple = (1, 2, 3)
print(my_tuple)  # 输出: (1, 2, 3)

# 创建包含不同类型元素的元组
mixed_tuple = (1, "hello", 3.14, [1, 2, 3])
print(mixed_tuple)  # 输出: (1, 'hello', 3.14, [1, 2, 3])

# 创建空元组
empty_tuple = ()
print(empty_tuple)  # 输出: ()
```

如果元组只有一个元素，需要在元素后面加一个逗号 ,，否则会被认为是普通括号。
```python
single_element_tuple = (42,)  # 这是一个元组
not_a_tuple = (42)            # 这是一个整数
```

#### 元组的遍历
```python
my_tuple = (10, 20, 30, 40, 50)

# 访问第一个元素
print(my_tuple[0])  # 输出: 10

# 访问最后一个元素
print(my_tuple[-1])  # 输出: 50

# 访问切片
print(my_tuple[1:4])  # 输出: (20, 30, 40)
```

元组是不可变的，因此不能修改、添加或删除元素。
```python
my_tuple = (1, 2, 3)

# 尝试修改元素
my_tuple[0] = 10  # 会抛出 TypeError: 'tuple' object does not support item assignment

# 尝试添加元素
my_tuple.append(4)  # 会抛出 AttributeError: 'tuple' object has no attribute 'append'

# 尝试删除元素
del my_tuple[0]  # 会抛出 TypeError: 'tuple' object doesn't support item deletion

# 但是可以给表示元组的变量赋值
my_tuple=(4,5,6) #VALID
tuple1 = (1, 2, 3)
tuple2 = (4, 5, 6)
combined_tuple = tuple1 + tuple2
print(combined_tuple)  # 输出: (1, 2, 3, 4, 5, 6)
```

#### 元组的基本操作
```python
# 元组的基本操作
# 元组的拼接
tuple1 = (1, 2, 3)
tuple2 = (4, 5, 6)
combined_tuple = tuple1 + tuple2
print(combined_tuple)  # 输出: (1, 2, 3, 4, 5, 6)

# 元组的重复
my_tuple = (1, 2, 3)
repeated_tuple = my_tuple * 3
print(repeated_tuple)  # 输出: (1, 2, 3, 1, 2, 3, 1, 2, 3)、

# 元组的长度
my_tuple = (1, 2, 3, 4, 5)
print(len(my_tuple))  # 输出: 5

# 元组的遍历
my_tuple = (1, 2, 3, 4, 5)
for item in my_tuple:
    print(item)
    
# 元组的解包
my_tuple = (10, 20, 30)
a, b, c = my_tuple
print(a, b, c)  # 输出: 10 20 30

# 元组嵌套其他元组或列表
nested_tuple = ((1, 2), (3, 4), [5, 6])
print(nested_tuple)  # 输出: ((1, 2), (3, 4), [5, 6])
```

**元组与列表的区别**

|特性|	元组 (tuple)|	列表 (list)|
|:----:|:----:|:----:|
|**可变性**	|不可变	|可变|
|**语法**|	使用圆括号 `()`|	使用方括号 `[]`|
|**性能**|	访问速度更快，占用内存更少|	访问速度较慢，占用内存较多|
|**适用场景**|	存储不可变数据（如常量、配置等）|	存储可变数据（如动态集合等）、

## (III) Dictionaries

字典`dict`，字典中的元素是**键值对**，类似与CPP中的`unordered_map`，**底层原理是哈希表**（Hash Table），用花括号`{}`表示，字典中的键必须是唯一的，而值可以是任意类型的数据。键-值对之间用**冒号**分隔，元素之间用**逗号**分隔
```python
my_dict={'name':'richard','age':18,'city':'Nanjing'}
print(my_dict['name']) # 输出键为'name'的值：richard
print(my_dict['age']) # 输出键为'age'的值：18
print(my_dict['city']) # 输出键为'city'的值：Nanjing
```

#### 字典的创建
```
# 创建一个字典
my_dict = {"name": "Alice", "age": 25, "city": "New York"}
# 注意：需要使用花括号！
print(my_dict)  # 输出: {'name': 'Alice', 'age': 25, 'city': 'New York'}

# 创建空字典
empty_dict = {}
print(empty_dict)  # 输出: {}

# 使用 dict() 函数创建字典
my_dict = dict(name="Alice", age=25, city="New York")
print(my_dict)  # 输出: {'name': 'Alice', 'age': 25, 'city': 'New York'}
```
#### 通过键来访问字典的值
```python
my_dict = {"name": "Alice", "age": 25, "city": "New York"}

# 访问键对应的值
print(my_dict["name"])  # 输出: Alice

# 访问不存在的键会抛出 KeyError
# print(my_dict["gender"])  # KeyError: 'gender'

# 使用get访问，一种更安全的方法
print(my_dict.get("name"))      # 输出: Alice
print(my_dict.get("gender"))    # 输出: None
print(my_dict.get("gender", "unknown"))  # 输出: unknown
```
#### 修改，插入，删除
```python
my_dict = {"name": "Alice", "age": 25, "city": "New York"}

# 修改值
my_dict["age"] = 26
print(my_dict)  # 输出: {'name': 'Alice', 'age': 26, 'city': 'New York'}

# 添加新的键值对
my_dict["gender"] = "female"
print(my_dict)  # 输出: {'name': 'Alice', 'age': 26, 'city': 'New York', 'gender': 'female'}

# 使用 del 删除键值对
del my_dict["city"]
print(my_dict)  # 输出: {'name': 'Alice', 'age': 25}

# 使用 pop() 删除键值对并返回值
age = my_dict.pop("age")
print(age)      # 输出: 25
print(my_dict)  # 输出: {'name': 'Alice'}

# 清空字典
my_dict.clear()
print(my_dict)  # 输出: {}
```
**其他操作**
```python
my_dict = {"name": "Alice", "age": 25, "city": "New York"}

# 遍历键
for key in my_dict:
    print(key)

# 遍历值
for value in my_dict.values():
    print(value)

# 遍历键值对
for key, value in my_dict.items():
    print(f"{key}: {value}")
    
# 使用in检查是否存在
my_dict = {"name": "Alice", "age": 25, "city": "New York"}
print("name" in my_dict)  # 输出: True
print("gender" in my_dict)  # 输出: False


# 获取所有键
print(my_dict.keys())  # 输出: dict_keys(['name', 'age', 'city'])

# 获取所有值
print(my_dict.values())  # 输出: dict_values(['Alice', 25, 'New York'])

# 获取所有键值对
print(my_dict.items())  # 输出: dict_items([('name', 'Alice'), ('age', 25), ('city', 'New York')])

dict1 = {"name": "Alice", "age": 25}
dict2 = {"city": "New York", "gender": "female","age":15}
dict1.update(dict2)
print(dict1)  # 输出: {'name': 'Alice', 'age': 15, 'city': 'New York', 'gender': 'female'}
# 合并冲突的键值对时会自动覆盖成新的
```
#### 字典推导式
```python
# 创建一个字典，键为数字，值为数字的平方（使用花括号{}）
squares = {x: x**2 for x in range(5)}
print(squares)  # 输出: {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

# Loops
### While-loops
在Python中，while循环的基本用法和 C/C++ 基本相同，唯一需要注意的是Python中**不加花括号**并且要**注意缩进**

```python
count = 0
while count < 5:
    print(f"Count is: {count}")
    count += 1
```
### for-loops
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
`range(start, stop, step)`
生成从 `start` 开始到 `stop-1` 的整数序列，步长为 `step`
```
for i in range(1, 10, 2):
    print(i)
'''
1
3
5
7
9
'''
```
#### 字典
```python
person={'name':'richard','age':18,'city':'Nanjing'}
for key,value in person.items():
    print(key,value)
```
#### 使用 `zip()`
`zip()` 函数可以同时遍历多个序列。
```python
names = ['Alice', 'Bob', 'Charlie']
scores = [85, 90, 95]
for name, score in zip(names, scores):
    print(f'{name} scored {score}')
```


## 其他语句
### 条件语句
```python
if condition:
    do something
elif another_condition:
    do something else
else:
    do something else
```
#### with 语句
with语句用于简化资源的管理，能在代码块执行完毕后自动关闭资源。
```python
with open('file.txt','r') as f:
    data=f.read()
    print(data)
```
#### try 语句
常用于**捕获和处理可能发生的异常**。

- **try**: 可能出现异常的代码
- **except**: 特定类型的异常
- **else**: 未发生异常的情况
- **finally**: 无论是否发生异常都会执行


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

#### 三元运算符
```python
age=int(input("Enter your age:"))
status="Adult" if age>=18 else "Child"
print(status)
```


### 用`for`循环制作一个简易的倒计时程序
```python
import time

my_time=int(input("Enter the time in seconds: "))

for x in range(my_time,0,-1):
    second=x%60
    minute=int(x/60)%60
    hour=int(x/3600)
    print(f"{hour:02}:{minute:02}:{second:02}")
    time.sleep(1)

print("Time's Up!")
```
```text
Enter the time in seconds: 4500
01:15:00
01:14:59
01:14:58
01:14:57
01:14:56
```
{%fold into@什么时候使用 if__name__=="__main__": ？%}
当我们在看一些开源项目时，经常能看到在结尾有一段“神秘代码”：
```python
if __name__ == "__main__":
    main()
```
或许会疑惑：没有这个其他代码也可以正常执行，为什么要有这一段代码呢？那么不妨先让我们看看`__name__`输出是什么：
```python
if __name__=="__main__":
    print(__name__)
# 输出： __main__
```
当我们在其他文件中引用这个文件时：
```python
# test_1.py
    print(__name__)
```

```python
# main.py
import test_1

if __name__=="__main__":
    print(__name__)
# 输出： 
# test_1
# __main__
```
这说明`__name__`在不同的地方是不一样的，当我们在当前文件中运行时，`__name__`为`__main__`，而当我们在其他文件中引用时，`__name__`为包名和模块名（`__name__` 是 Python 解释器自带的「内置变量」（不需要我们手动定义」），它的作用是「标识当前模块的运行状态）。

当我们导入一个模块时，可能我们只是想调用里面的函数，而不是运行模块中的代码。

```python
# calculator.py
def add(a,b):
    return a+b

def sub(a,b):
    return a-b

print("This is a simple calculator")
num1=int(input("Enter the first number: "))
num2=int(input("Enter the second number: "))
print(f"The sum of {num1} and {num2} is {add(num1,num2)}")
print(f"The difference of {num1} and {num2} is {sub(num1,num2)}")
```

```python
# program.py
import calculator

num1=10
num2=20

print("Using the calculator module")
print(f"The sum of {num1} and {num2} is {calculator.add(num1,num2)}")
print(f"The difference of {num1} and {num2} is {calculator.sub(num1,num2)}")
```
让我们看看结果何如：
```text
This is a simple calculator
Enter the first number: 2
Enter the second number: 2
The sum of 2 and 2 is 4
The difference of 2 and 2 is 0
Using the calculator module
The sum of 10 and 20 is 30
The difference of 10 and 20 is -10
```
可以看到我们明明只想要调用`calculator`模块中的`add`和`sub`函数，但是程序中却执行了`calculator`模块中的所有代码，包括打印提示信息和获取用户输入。这时，`if __name__ == '__main__':`就起作用了！

让我们对`calculator.py`稍作修改：
```python
def add(a,b):
    return a+b

def sub(a,b):
    return a-b

if __name__=="__main__":
    print("This is a simple calculator")
    num1 = int(input("Enter the first number: "))
    num2 = int(input("Enter the second number: "))
    print(f"The sum of {num1} and {num2} is {add(num1, num2)}")
    print(f"The difference of {num1} and {num2} is {sub(num1, num2)}")
```

运行结果如下：
```text
Using the calculator module
The sum of 10 and 20 is 30
The difference of 10 and 20 is -10
```

{%endfold%}
