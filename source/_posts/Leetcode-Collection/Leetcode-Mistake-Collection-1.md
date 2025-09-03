---
title: Leetcode Mistake Collection 1
date: 2025-08-26 10:26:06
tags: [Leetcode,updating]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/Leetcode-Mistake-Collection.png?raw=true
category: Leetcode Mistake Collection
category_bar: true
description: The article is intended to systematically document the errors I encountered while solving LeetCode problems, along with the corresponding corrective strategies.
---


由于cpp基础并不扎实，因而打算分类刷题，在此过程中对基本语法与STL修修补补!

## Reference:

[CS-Notes](https://github.com/CyC2018/CS-Notes)

[LeetCode 刷题顺序，按标签分类，科学刷题！](https://blog.csdn.net/fengyuyeguirenenen/article/details/125099023?fromshare=blogdetail&sharetype=blogdetail&sharerId=125099023&sharerefer=PC&sharesource=m0_53058983&sharefrom=from_link)

[LeetCode Cookbook](https://books.halfrost.com/leetcode/)



## Leetcode 628.三个数的最大乘积

[原题链接](https://leetcode.cn/problems/maximum-product-of-three-numbers/solutions/567309/san-ge-shu-de-zui-da-cheng-ji-by-leetcod-t9sb/)

{%fold into @ Time Error Version :x:%}
一开始无脑遍历所有组合，显然时间复杂度是$O(n^3)$，会超时！
```cpp
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        int n=nums.size();
        int max=INT_MIN;
        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++){
                for(int k=j+1;k<n;k++){
                    int mul=nums[i]*nums[j]*nums[k];
                    if(mul>max){
                        max=mul;
                    }
                }
            }
        }
        return max;
    }
};
```
{%endfold%}

我们这里考虑使用数学方法：

- 数组**均为正数**，乘积最大即为**最大三个正数相乘**
- 数组**均为负数**，乘积最大即为**最大三个负数相乘**
- 数组**有正有负**
  - **两个负数与一个正数相乘**
  - **三个最大正数相乘**

我们可以先将数组**排序**，然后分情况讨论：
- 1、2、4都取最大的三个数
- 3取最小的两个负数（数组首端）与最大的一个正数（数组尾端）

```cpp
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int n=nums.size();
        return max(nums[0]*nums[1]*nums[n-1],nums[n-3]*nums[n-2]*nums[n-1]);
    }
};
```

## Leetcode 645.错误的集合
[原题链接](https://leetcode.cn/problems/set-mismatch)

{%fold into @ Error Version :x:%}
需要注意题目要求的数组中数字的**顺序**，若是使用`if`，则先找到谁会先输出谁！
```cpp
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int n=nums.size();
        vector <int> vec;
        unordered_map <int,int> error;
        for(auto it:nums){
            error[it]++;
        }
        for(int i=1;i<=n;i++){
            int count=error[i];
            if(count==2){
                vec.push_back(i);
            } else if(count==0){
                vec.push_back(i);
            }
        }
        return vec;
    }
```
{%endfold%}

这里采用**哈希表法**
- **重复的数字**在数组中出现**2次**
- **丢失的数字**在数组中出现**0次**
- 其余的每个数字在数组中出现**1次**
  
因此可以使用**哈希表**记录每个元素在数组中出现的次数，然后遍历从1到n的每个数字，分别找到出现2次和出现0次的数字，即为重复的数字和丢失的数字。
```cpp
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int n=nums.size();
        vector <int> vec(2);
        unordered_map <int,int> error;
        for(auto it:nums){
            error[it]++;
        }
        for(int i=1;i<=n;i++){
            int count=error[i];
            if(count==2){
                vec[0]=i;
            } else if(count==0){
                 vec[1]=i;
            }
        }
        return vec;
    }
};
```

访问一个尚未在 map 中的键：

1. 在 map 中**创建**一个键为 it 的新元素。
2. 对这个新元素的值进行**值初始化**。对于 int 类型，值初始化就是将其初始化为 0。
3. 然后对这个**新初始化的值**执行操作。

## Leetcode 448.找到数组中消失的数字

[原题链接](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array)

{%fold into @ Error Version :x:%}
企图使用键key和值val相配对的方式**找到缺失数字**，但是又包含**重复数字**，意图使用键值大小比较，另设`extra`进行**偏移量调整**，但是使用`auto`迭代器，**索引无法完成补偿**，且代码过于冗余，遂放弃！
```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        unordered_map <int,int> arr;
        sort(nums.begin(),nums.end());
        int n=nums.size();
        for(int i=1;i<=n;i++){
            arr[i]=nums[i-1];
        }
        vector <int> vec;
        int extra=0;
        for(auto it:arr){
            if((it+extra).first>it.second){
                extra--;
            }
            else if((it+extra).first<it.second){
                extra++;
                vec.push_back((--it.second)
            }
        }
        return vec;
    }
};
```
{%endfold%}

```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        unordered_map <int,bool> arr;
        vector <int> vec;
        int n=nums.size();
        for(auto it:nums){
            arr[it]=true;
        }
        for(int i=1;i<=n;i++){
            if(arr.find(i)==arr.end()){
                vec.push_back(i);
            }
        }
        return vec;
    }
};
```
采用哈希表`<int,bool>`，对**数组进行遍历**
- **整型记录数字**（索引）
- **布尔型记录数字是否出现**


## Leetcode 274.H指数

[原题链接](https://leetcode.cn/problems/h-index)

{%fold into @ Error Version :x:%}
想先排序，再按照`citations[i]`的值与`index`**数组索引值**（计算>的至少有多少篇），但未考虑到若是`4、4、4`这样分布，H指数未必是数组中的数字。
```cpp
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n=citations.size();
        sort(citations.begin(),citations.end());
        int index;
        for(int i=0;i<n;i++){
            if(n-i>=citations[i]){
                index=citations[i];
            }
        }
        return index;
    }
};
```
{%endfold%}
考虑到H指数的指标是固定不变的，因而排序后我们采用**从大到小的遍历方式**。
```cpp
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n=citations.size();
        sort(citations.begin(),citations.end());
        int h=0,i=citations.size()-1;
        while(i>=0 && citations[i]>h){
            i--;
            h++;
        }
        return h;
    }
};
```

## Leetcode 283.移动零

[原题链接](https://leetcode.cn/problems/move-zeroes)

{%fold into @ Error Version :x:%}
未考虑**连续0**的情况
```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n=nums.size();
        int count=0;
        for(int i=0;i<n-1;i++){
            if(nums[i]==0){
                count++;
                for(int j=i;j<n-1;j++){
                    nums[j]=nums[j+1];
                }
            }
        }
        for(int i=n-count;i<n;i++){
            nums[i]=0;
        }
    }
};
```
{%endfold%}
### 双指针
- **左指针**：指向**已处理好的序列的末端**
- **右指针**：指向**未处理的序列的起始位置**

1. 右指针左边到左指针之间均为0
2. 左指针**左侧均为非零数**

左右指针均从`index`索引**0位置出发**：
- 若为0，右指针右移，左指针不动（相当于标记0位置）
- 若不为0，**交换**左右指针指向的元素，均向右移动

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int right=0,left=0;
        int n=nums.size();
        while(right<n){
            if(nums[right]){
                swap(nums[left],nums[right]);
                left++;
            }
            right++;
        }
    }
};
```

## Leetcode 118.杨辉三角 + 119.杨辉三角ii
[原题链接](https://leetcode.cn/problems/pascals-triangle)
[原题链接](https://leetcode.cn/problems/pascals-triangle-ii)

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> vec(numRows);//初始化杨辉三角行数
        for(int i=0;i<numRows;i++){
            vec[i].resize(i+1);//初始化杨辉三角每列元素个数 第i行有i+1个元素
            vec[i][0]=1;//初始化杨辉三角每列第一个元素为1
            vec[i][i]=1;//初始化杨辉三角每列最后一个元素为1
            for(int j=1;j<i;j++){
                vec[i][j]=vec[i-1][j-1]+vec[i-1][j];
            }
        }
        return vec;
    }
};
```
### 优化一：滚动数组
当我们只需要求杨辉三角的`rowIndex`行时，发现对第`i+1`行的计算仅用到了第`i`行的数据,因而可以通过**滚动数组只保留当前行和上一行**的元素，而不需要**更早的元素**。
```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector <int> pre,cur;
        for(int i=0;i<=rowIndex;i++){
            cur.resize(i+1);
            cur[0]=cur[i]=1;
            for(int j=1;j<i;j++){
                cur[j]=pre[j-1]+pre[j];
            }
            pre=cur;
        }
        return pre;
    }
};
```
### 优化二：单一数组逆序更新
滚动数组的优化，将**当前行**的元素**逆序更新**到**上一行**，从而**不需要保存当前行**，原本的**两数相加**在**同一数组**中每次仅需要加一次即可！。
```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector <int> row(rowIndex+1);
        for(int i=0;i<=rowIndex;i++){
            row[0]=1;
            for(int j=i;j>0;j--){
                row[j]+=row[j-1];
            }
        }
        return row;
    }
};
```

## Leetcode 14.最长公共前缀
[原题链接](https://leetcode.cn/problems/longest-common-prefix)

### 方法一：横向扫描
1. 获取数组中第一个字符串作为**最长公共前缀**
2. **遍历数组**中的每个字符串，与**最长公共前缀**进行**公共前缀**的比较，更新**最长公共前缀**
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        int n=strs.size();
        string prefix=strs[0];
        for(int i=1;i<n;i++){
            prefix=CommonPrefix(prefix,strs[i]);
        }
        return prefix;
    }
    string CommonPrefix(string s1,string s2){
        int index=0;
        for(int i=0;i<min(s1.size(),s2.size());i++){
            if(s1[i]!=s2[i]){
                break;
            }
            index++;
        }
        return s1.substr(0,index);
    }
};
```

### 方法二：纵向扫描
1. **遍历数组**，**依次比较**每个字符串第1、2、3...个字符
2. 返回最长前缀：
- 若有字符串的第`i`个字符不同，则**最长公共前缀**的长度为`i-1`
- 若字符串的长度小于`i`，则**最长公共前缀**的长度为`i`
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        int n=strs.size();
        int count=strs[0].size();
        int i=0;
        for(i=0;i<count;i++){
            for(int j=1;j<n;j++){
                if( i==strs[j].size() || strs[j][i]!=strs[0][i]){
                    return strs[0].substr(0,i);
                }
            }
        }
        return strs[0];
    }
};
```
### 方法三：分治
### 方法四：二分查找


## Leetcode 470.用 Rand7() 实现 Rand10()
[原题链接](https://leetcode.cn/problems/implement-rand10-using-rand7)
{%fold into @ Error Version :x:%}
```cpp
class Solution {
public:
    int rand10() {
        int first,second;
        while(first=rand7()>6);
        while(second=rand7()>5);
        return (first&1)==1? second:5+second;
}
```
该答案第5、6行缺失括号而导致错误！
#### 第一种错误版本
```cpp
while(first=rand7()>6);
while(second=rand7()>5);
```
这里由于运算符优先级（比较运算符 > 高于赋值运算符 =），实际执行顺序是：
- 先计算`rand7()>6`的布尔结果（`true` 或 `false`，即 1 或 0）
- 将这个布尔结果赋值给 `first` 变量
- 循环条件永远是 1 或 0，导致逻辑错误
  
#### 第二种正确版本
```cpp
while((first=rand7())>6);
while((second=rand7())>5);
```
- 调用 `rand7 ()` 生成随机数
- 将结果赋值给 `first` 变量
- 检查该值是否大于 6
{%endfold%}

这道题的目的是使用`rand7()`产生10个不同但**概率相等**的数
### 方法一：拒绝采样
1. 首先**生成1-49的均匀随机数**：:exclamation:`(rand7()-1)*7 + rand7()`:exclamation:
2. 拒绝41-49的样本（保持1-40的**均匀分布**）
3. 对结果取模+1，得到1-10的**均匀随机数**
{%fold into @ 为什么这样生成采样样本？%}
1.`rand7()-1`生成 0~6
2.`(rand7()-1)*7`生成 0、7、14、21、28、35、42
3.`(rand7()-1)*7 + rand7()`生成 1~49
**无重复数字**，每个数字出现的**概率均相同**是1/49
{%endfold%}
```cpp
class Solution {
public:
    int rand10() {
        int num;
        while((num=(rand7()-1)*7+rand7())>40);
        return num%10+1;
    }
};
```

### 方法二：古典概型
1. 第一次`rand7`限定[1,6]，**判断奇偶性**，概率是1/2
2. 第二次`rand7`限定[1,5]，概率是1/5
3. 二者结合可以得出10种概率相同的结果
```cpp
class Solution {
public:
    int rand10() {
        int first,second;
        while((first=rand7())>6);
        while((second=rand7())>5);
        return (first&1)==1? second:5+second;
}
```

#### 位运算符
- **按位与（&）**：对两个操作数的每一位进行比较，只有当两位都为 1 时结果对应位才为 1，否则为 0。
示例：3 & 5（二进制 011 & 101）结果为 1（二进制 001）。
```cpp
bool isOdd(int x) {
    return x & 1; // 结果为1则奇数，0则偶数
}
```
- **按位或（|）**：对两个操作数的每一位进行比较，只要其中一位为 1，结果对应位就为 1，否则为 0。
示例：3 | 5（二进制 011 | 101）结果为 7（二进制 111）。
- **按位异或（^）**：对两个操作数的每一位进行比较，当两位不同时结果为 1，相同时为 0。
示例：3 ^ 5（二进制 011 ^ 101）结果为 6（二进制 110）。
```cpp
void swap(int& a, int& b) {
    if (a != b) { // 避免a和b同地址时出错
        a = a ^ b; // a现在是a^b
        b = a ^ b; // b = (a^b)^b = a
        a = a ^ b; // a = (a^b)^a = b
    }
}
```
- **按位非（~）**：对单个操作数的**每一位取反**（0 变 1，1 变 0），是一元运算符。
示例：~3（二进制 ~011）在 32 位系统中结果为 -4（二进制补码表示）。
- **左移（<<）**：将左操作数的**二进制位向左移动指定的位数**，右侧空位补 0。
示例：3 << 1（二进制 011 << 1）结果为 6（二进制 110），相当于**乘以 2 的 n 次方**（n 为移动位数）。
- **右移（>>）**：将左操作数的**二进制位向右移动指定的位数**，左侧空位补符号位（正数补 0，负数补 1）。
示例：6 >> 1（二进制 110 >> 1）结果为 3（二进制 011），相当于**除以 2 的 n 次方**（向下取整）。