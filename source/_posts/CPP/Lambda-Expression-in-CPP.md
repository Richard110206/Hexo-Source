---
title: Lambda Expression in CPP
date: 2025-08-25 11:36:54
tags: [CPP, syntax]
category: CPP
category_bar: true
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/LambdaExpression.png?raw=true
description: This article provides a straightforward introduction to the basic usage of lambda expressions in C++. 
---

## What's Lambda Expression?

`Lambda` 表达式是 C++11 标准引入的一种用于**创建匿名函数对象**的强大特性。它允许你在需要函数的地方内联地定义函数，而无需单独命名和定义函数或函数对象，这使得代码**更简洁**、**更易读**，尤其在使用 STL 算法时。

## Basic Syntax
```cpp
[ 捕获列表 ] ( 参数列表 ) -> 返回类型 {
    // 函数体
}
```

1. 捕获列表 (Capture Clause) [ ]
这是 `Lambda` 表达式的开端，也是它最独特和强大的部分。它定义了`Lambda`函数体中可以访问的外部作用域中的变量及其访问方式。

{%note info %}
- [] ：**空捕获列表**，表示不捕获任何外部变量。
- [=] ：以**值捕获**的方式捕获**所有外部变量**。`Lambda` 体内使用的是这些变量的副本，修改副本不会影响外部变量。
- [&] ：以**引用捕获**的方式捕获**所有外部变量**。`Lambda` 体内使用的是这些变量本身，修改它们会影响外部变量。
```cpp
int x = 10,y =5;
auto f = [=]() { return x + y; };  // 按值捕获所有变量
auto g = [&]() { x += y; };        // 按引用捕获所有变量
```
- [var] ：仅以**值捕获**的方式捕获**特定变量** var。
```cpp
int x=10;
auto f=[x]() -> int{ //按值捕获x
    return x+1;
};
std::cout<<f(); //输出11，x的值为10未改变
```
- [&var] ：仅以**引用捕获**的方式捕获**特定变量** var。
```cpp
int x = 10;
auto f = [&x]() { x += 1; };
f();
std::cout << x;  // 输出11，x被修改
```
- 混合捕获：可以**组合使用**，例如 [=, &x] 表示以值捕获所有外部变量，但变量 x 除外，它以引用方式捕获。[&, x] 则表示以引用捕获所有外部变量，但变量 x 以值方式捕获。
```cpp
int x = 10, y = 20;
auto f = [x, &y]() { 
    y+=x;
    return x + y; 
};  // x按值，y按引用
    std::cout << f();
```

{% endnote %}

2. 参数列表 (Parameter List) ( )
3. 返回类型 (Return Type) -> return_type
{%note info %}
可以显式地使用 `->` 后缀语法来指定 `Lambda` 的返回类型。在大多数情况下，编译器可以**自动推导出返回类型**
```cpp
// 自动推导返回类型为 int
auto simple = [](int x) { return x * 2; };
```
```cpp
// 显式指定返回类型为 double（即使 x 是 int）
auto explicit_return = [](int x) -> double { return x * 2.5; };
```

当函数体包含**多个返回语句且类型不同**，或者返回语句过于复杂编译器无法推导时，需要**显式指定**。

```cpp
// 需要显式指定返回类型的例子：多个返回语句
// auto ambiguous = [](bool test) {
//     if (test) return 10; // 返回 int
//     else return 20.0;    // 返回 double -> 错误！编译器无法推导
// };
```
```cpp
auto fixed = [](bool test) -> double { // 显式指定为 double
    if (test) return 10; // int 可隐式转换为 double
    else return 20.0;
};
```
{% endnote %}

4. 函数体 (Body) { }
和普通函数一样，包含了 `Lambda` 被调用时要执行的代码。

## Application
1. 与 STL 算法配合使用
简单的说就是STL算法用来**遍历容器**，使用`Lambda`表达式设定**特定的程序**，来处理不同的任务！
{%note info %}
### `std::sort` 自定义排序规则


#### 方法1：使用默认排序（升序）

```cpp
    std::vector<int> numbers = {4, 2, 8, 5, 1};
    std::sort(numbers.begin(), numbers.end());
    // numbers 变为 {1, 2, 4, 5, 8}
```

#### 方法2：使用函数指针（传统方式，不推荐）

```cpp
// 传统方式：定义一个独立的比较函数
bool compareDescending(int a, int b) {
    return a > b; // 如果a大于b，返回true，这样a就会排在b前面
}
    std::sort(numbers.begin(), numbers.end(), compareDescending);
// numbers 变为 {8, 5, 4, 2, 1}
```

#### 方法3：使用Lambda表达式（现代方式，推荐！）

```cpp
// 语法：std::sort(开始迭代器, 结束迭代器, [](参数){ 比较逻辑 });
    std::sort(numbers.begin(), numbers.end(),
              [](int a, int b) {
                  return a < b; // 升序排序
              });

    std::sort(numbers.begin(), numbers.end(),
              [](int a, int b) {
                  return a > b; // 降序排序
              });
```
```cpp
    // 更复杂的排序规则：按奇偶性排，偶数在前，奇数在后，各自内部从小到大
    std::vector<int> mixed = {3, 6, 1, 8, 2, 7};
    std::sort(mixed.begin(), mixed.end(),
              [](int a, int b) {
                  if (a % 2 == 0 && b % 2 != 0) return true;  // a是偶数，b是奇数 -> a在前
                  if (a % 2 != 0 && b % 2 == 0) return false; // a是奇数，b是偶数 -> b在前
                  return a < b; // 同为奇数或同为偶数，数值小的在前
              });
    // mixed 变为 {2, 6, 8, 1, 3, 7}
```


### `std::for_each` 对每个元素执行操作

```cpp
    std::vector<std::string> words = {"apple", "banana", "cherry", "date"};
    // 传统for循环
    for (const auto& word : words) {
        std::cout << word << " ";
    }
    std::cout << std::endl;

    // 使用 std::for_each + Lambda
    // 语法：std::for_each(开始迭代器, 结束迭代器, [](元素){ 操作 });
    std::for_each(words.begin(), words.end(),
                  [](const std::string& w) {
                      std::cout << w << " ";
                  });
    std::cout << std::endl;

    // 更实用的例子：修改元素（注意这里用引用捕获&，或者直接传引用）
    std::vector<int> nums = {1, 2, 3, 4, 5};
    std::for_each(nums.begin(), nums.end(),
                  [](int& n) { // 注意参数是 int&，这样才能修改原值
                      n *= 2; // 每个元素乘以2
                  });
    // nums 变为 {2, 4, 6, 8, 10}
```

### `std::find_if` 按条件查找元素

```cpp
struct Person {
    std::string name;
    int age;
};
    std::vector<Person> people = {
        {"Alice", 25},
        {"Bob", 17},
        {"Charlie", 30},
        {"David", 16}
    };

    // 查找第一个年龄大于18岁的人
    // 语法：auto result = std::find_if(开始, 结束, [](元素){ 判断条件 });
    auto adultIt = std::find_if(people.begin(), people.end(),
                                [](const Person& p) {
                                    return p.age >= 18;
                                });

    if (adultIt != people.end()) {
        std::cout << "First adult: " << adultIt->name << std::endl;
    }

    // 查找名字以'C'开头的人
    auto nameIt = std::find_if(people.begin(), people.end(),
                               [](const Person& p) {
                                   return !p.name.empty() && p.name[0] == 'C';
                               });
```
### `std::count_if` 统计满足条件的元素个数

```cpp
    std::vector<int> scores = {85, 92, 78, 90, 65, 88, 72, 95, 60, 81};

    // 统计及格（>=60）的人数
    int passCount = std::count_if(scores.begin(), scores.end(),
                                  [](int score) {
                                      return score >= 60;
                                  });

    // 统计优秀（>=90）的人数
    int excellentCount = std::count_if(scores.begin(), scores.end(),
                                       [](int score) {
                                           return score >= 90;
                                       });

    std::cout << "Pass: " << passCount << ", Excellent: " << excellentCount << std::endl;
```
### `std::transform` 转换容器中的元素 
```cpp
    std::vector<int> numbers = {1, 4, 9, 16, 25};

    // 计算每个数的平方根
    std::vector<double> roots(numbers.size());
    std::transform(numbers.begin(), numbers.end(), roots.begin(),
                   [](int n) {
                       return std::sqrt(n);
                   });
    // roots = {1, 2, 3, 4, 5}

    // 将所有数字转为字符串
    std::vector<std::string> strNumbers(numbers.size());
    std::transform(numbers.begin(), numbers.end(), strNumbers.begin(),
                   [](int n) {
                       return std::to_string(n);
                   });
```
{% endnote %}

2. 简化代码，增强可读性