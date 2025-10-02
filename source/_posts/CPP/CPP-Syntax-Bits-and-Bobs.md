---
title: CPP Syntax Bits and Bobs
date: 2025-08-25 10:56:48
tags: [CPP, syntax,updating]
category: CPP
category_bar: true
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/CPPmemo.png?raw=true
description: The article serves as a memo for the C++ syntax that I have forgotten, which mainly includes the new features of C++14 and C++17.
---

对在Leetcode刷题过程中遗漏的CPP语法进行补充，内容较为零碎，就先委屈一点挤挤吧:grinning:，可能较多的是C++14/17**新特性**，如若后期深入学习Modern CPP，可能就会删减单拉出去为一章节了，敬请期待！

## (I)auto
`auto`是一个**类型占位符**，它指示编译器**自动推导变量的类型**。编译器会根据**初始化表达式**（等号右边的值）来确定`auto`变量的实际类型。
```cpp
int x=10;
//等效于
auto x=10;
```
```cpp
double y=10.0;
//等效于
auto y=10.0;
```
```cpp
auto f = 3.14;  //double
auto s("hello");  //const char*
auto z = new auto(9);  //int *
auto x1 = 5, x2 = 5.0, x3 = 'r';   //错误，必须是初始化为同一类型
```
但是显然，这么简单的类型推导，`auto`的作用并不大。

### auto常用场景
`auto`的真正作用是在**复杂的类型推导**中，例如**迭代器**、**lambda表达式**等，使程序更清晰易读。

#### 简化复杂类型声明
```cpp
std::vector<int> v = {1, 2, 3, 4, 5};
// for (std::vector<int>::iterator it = v.begin(); it != v.end(); ++it) {
for (auto it = v.begin(); it != v.end(); ++it) {
    std::cout << *it << std::endl;
}
```

#### 范围 for 循环
```cpp
for (auto element : container) {
    // 使用 element
}
```

#### Lambda 表达式
```cpp
auto lambda = [](int x, int y) { return x + y; };
```

### Limitations
{%note info %}
- 必须**初始化**
```cpp
auto x;  // 错误：无法推导类型
```
- 不能用于**函数参数**
```cpp
void func(auto x) {  // 错误：不能用于函数参数
    // ...
}
```
- 多变量声明需**类型一致**
```cpp
auto x = 10, y = 20.0;  // 错误：类型不一致
```
{%endnote%}

## (II)结构化绑定
**结构化绑定**（Structured Bindings）是C++17这是一个非常实用的**语法特性**，它允许我们**同时声明多个变量并从一个聚合类型**（如struct、pair 等）中**提取其成员**，使代码更加简洁易读。
```cpp
auto [var1, var2, ...] = 聚合类型;
```
### 解构函数
结构化绑定可以非常方便地处理**多个返回值**的函数，例如返回`tuple`或`pair`的函数。
```cpp
#include <iostream>
#include <tuple>
using namespace std;
tuple<int,int> calculate(int a,int b) {
    return {a+b,a-b};
}
int main() {
    auto [add,sub]=calculate(3,1);
    cout<<add<<endl;
    cout<<sub<<endl;
    return 0;
}
```
### 解构结构体（类）
```cpp
#include <iostream>
#include <string>
using namespace std;
struct Product {
    string name;
    double price;
    int stock;
};
Product apple={"apple",4.1,50};

int main() {
    auto [name,val,stock]=apple;
    cout<<name<<val<<stock;
    return 0;
}
```
### 解构数组（静态）
```cpp
#include <iostream>
using namespace std;
int main() {
    int numbers[5] = {10, 20, 30, 40, 50};
    auto [a, b, c, d, e] = numbers;
    cout << "Array elements: " << a << ", " << b << ", " << c << ", " << d << ", " << e << std::endl;
    
    // 二维数组解构
    int matrix[2][2] = {{1, 2}, {3, 4}};
    auto [row1, row2] = matrix;
    auto [r1c1, r1c2] = row1;
    auto [r2c1, r2c2] = row2;
    cout << "Matrix: " << r1c1 << "," << r1c2 << " | " << r2c1 << "," << r2c2 << endl;
    
    return 0;
}
```
### 遍历容器+范围for循环
```cpp
#include <iostream>
#include <unordered_map>
using namespace std;
int main() {
    unordered_map<int, string> myMap = {{1, "one"}, {2, "two"}, {3, "three"}};
    for (const auto& [key, value] : myMap) {
        cout << key << ": " << value << endl;
    }
}
```

## (III)容器函数
### 查找最大值、最小值
- `max_element`返回指向容器中最大元素的**迭代器**
- `min_element`返回指向容器中最小元素的**迭代器**
- `minmax_element`：返回一个**pair容器**，其中 first 是最小元素的**迭代器**，second 是最大元素的**迭代器**

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main() {
    vector<int> v={1,2,3,4,5};
    auto [min_it,max_it]=minmax_element(v.begin(),v.end());
    cout<<*min_it<<endl;
    cout<<*max_it<<endl;
    return 0;
}
```

{%note danger%}
:warning: 使用`*`**解引用迭代器**
`*max_element`和`*min_element`返回是**元素值**
{%endnote%}

### 判断排序
- `is_sorted`判断容器是否已排序(默认**升序**)：若满足`*(i+1)>=*i`时，判断**已排序**

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main() {
    vector<int> v1={1,2,3,4,5};
    vector<int> v2={1,3,2,4,5};
    cout<<"v1排序情况："<<is_sorted(v1.begin(),v1.end())<<endl;
    cout<<"v2排序情况："<<is_sorted(v2.begin(),v2.end())<<endl;
    return 0;
}
```

#### 重载比较函数
- `is_sorted`提供**重载版本**，可以接受**自定义比较函数**（**使用lambda expression**）

[Leetcode 665.非递减数列](https://leetcode.cn/problems/non-decreasing-array)

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main() {
    vector<int> v={5,4,3,2,1};
    bool is_descending=is_sorted(v.begin(),v.end(),[](int a,int b){return a>b;});
    cout<<"v是否降序排序："<<is_descending<<endl;
    return 0;
}
```

### (IV)最大公约数、最小公倍数
C++17 新增了 `<numeric>` 头文件，其中包含了 `gcd` 和 `lcm` 函数，用于计算最大公约数和最小公倍数。

- `gcd`：计算**最大公约数**
- `lcm`：计算**最小公倍数**
```cpp
#include <iostream>
#include <numeric>
using namespace std;
int main() {
	int a = 8, b = 12;
	int g = gcd(a, b);
	int l = lcm(a, b);
	cout << g << endl;
	cout << l << endl;
	return 0;
}
```
```
4
24
```
这里考虑到CSP考试中可能只支持C++11，这里再进行一次手动实现：
#### 最大公约数从大到小循环
```cpp
int gcd(int a, int b) {
    for (int i = min(a, b); i >= 1; i--) { 
        if (a % i == 0 && b % i == 0) {
            return i;  
        }
    }
    return 1;  
```
#### 最大公约数欧几里得算法（辗转相除法）
- **迭代**实现
```cpp
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```
- **递归**实现
```cpp
int gcd(int a, int b) {
    if (b == 0)
        return a;
    return gcd(b, a % b);
}
```
#### 最小公倍数
通常求得最大公约数后通过**公式**实现：
$$ LCM(a, b) = \frac{(a × b)}{GCD(a, b)}$$
```cpp
int lcm(int a, int b) {
    return (a / gcd(a, b)) * b;
}
```
## (V)