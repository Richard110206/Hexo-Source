---
title: Leetcode-Mistake-Collection-3
date: 2025-11-26 21:45:00
tags: [Leetcode,updating]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/Leetcode-Mistake-Collection.png?raw=true
category: Leetcode Mistake Collection
category_bar: true
description: The article is intended to systematically document the errors I encountered while solving LeetCode problems, along with the corresponding corrective strategies.
math: true
---

### 同余定理
核心公式：$$(a \times b + c) \mod m = [(a \mod m) \times (b \mod m) + (c \mod m)] \mod m$$


[11.24每日一题原题链接：Leetcode 1018. 可被 5 整除的二进制前缀](https://leetcode.cn/problems/binary-prefix-divisible-by-5?envType=daily-question&envId=2025-11-26)
[11.25每日一题原题链接：Leetcode 1015. 可被 K 整除的最小整数](https://leetcode.cn/problems/smallest-integer-divisible-by-k?envType=daily-question&envId=2025-11-26)

以上两道每日一题都有一个相同的特点，成指数级增长，暴力解法必然超时或溢出，我们利用余数相同的数具有相同的整除性质来解决这道题目！

#### 1. 前缀数值的递推关系
对于二进制数组 `nums`，第 `i` 个前缀的二进制值 `num[i]` 与第 `i-1` 个前缀的关系为：`num[i] = num[i-1]*2 + nums[i]`

#### 2. 转化为余数的递推
我们需要判断 `num[i]` 是否能被 $5$ 整除，根据同余定理：
`num[i] = num[i-1]*2 + nums[i] = [num[i-1] mod 5) * (2 mod 5) + (nums[i] mod 5)] mod 5`
简化得：`num[i] = (num[i-1]*2 + nums[i]) mod 5`

#### 3. 用余数代替前缀和
又因为 `num[i-1] mod 5 = prefix[i-1] mod 5`，因而有余数的递推式子  `num[i] = (num[i-1]*2 + nums[i]) mod 5`

```cpp
class Solution {
public:
    vector<bool> prefixesDivBy5(vector<int>& nums) {
        vector <bool> ans;
        int prefix = 0;
        int length = nums.size();
        for (int i = 0; i < length; i++) {
            prefix = ((prefix*2) + nums[i]) % 5;
            ans.push_back(prefix == 0);
        }
        return ans;
    }
};
```

### 应用 2
与上一问相同，找到每次计算数字的递推关系式，用同余定理进行余数代换，不同的是需要使用**哈希表**对每次的余数进行存储，若是遇到相同的余数，说明重新进入一个循环，则永远无法被整除。
```cpp
class Solution {
public:
    int smallestRepunitDivByK(int k) {
        int left=1%k;
        int len=1;
        unordered_set<int> st;
        st.insert(left);
        while(left!=0){
            left=(left*10+1)%k;
            len++;
            if(st.find(left)!=st.end()) return -1;
            st.insert(left);
        }
        return len;
    }
};
```
### Leetcode 3381.长度可被 K 整除的子数组的最大元素和
[11.25每日一题原题链接：Leetcode 1015. 可被 K 整除的最小整数](https://leetcode.cn/problems/maximum-subarray-sum-with-length-divisible-by-k?envType=daily-question&envId=2025-11-27)

看到子数组和整除自然想到使用**前缀和**与**同余定理**：

1. **同余定理的应用**
   子数组长度 `j - i` 是 k 的整数倍等价于 `(j - i) mod k = 0`，根据同余定理可推导得 `j mod k = i mod k`。即：**若这两个索引之间的子数组长度为 k 的整数倍，则两个前缀和的索引对 k 取余结果相同**。

2. **最大化子数组和的逻辑**
   要使 `prefix[j] - prefix[i]` 最大（且 `j mod k = i mod k`），需为每个余数 `r`（`r = j mod k`）维护此前出现过的最小前缀和 `min[r]`。遍历到 `j` 时，用 `prefix[j] - min[r]` 更新最大和，再将 `prefix[j]` 纳入 `min[r]` 的最小值更新。

```cpp
class Solution {
public:
    long long maxSubarraySum(vector<int>& nums, int k) {
        int n=nums.size();
        vector<long long> prefix_sum(n+1);
        for (int i = 0; i < n; i++) {
            prefix_sum[i + 1] = prefix_sum[i] + nums[i];
        }
        vector<long long> min_s(k, LLONG_MAX / 2); 
        long long ans = LLONG_MIN;
        for(int j=0;j<prefix_sum.size();j++){
            int i=j%k;
            ans=max(ans,prefix_sum[j]-min_s[i]);
            min_s[i]=min(min_s[i],prefix_sum[j]);
        }
        return ans;
    }
};
```

### Leetcoe 50.Pow(x, n)
[Leetcode 50. Pow(x, n)](https://leetcode.cn/problems/powx-n)

#### 快速幂
简单地说就是如果计算$x^n$，`pow(x,n)`计算的时多个 $x$ 相乘耗时较长，我们可以将$n$分解为二进制数，我们只需要计算$x^1, x^2, x^4, x^8, \dots$，然后根据二进制数的每一位是否为1，来判断是否需要乘到结果中。
{%note info%}
当$n=13$时，二进制数为$1101$，$n=13=1+4+8$，有$ x^{13} = x^{1+4+8} = x^{1} · x^{4} · x^{8} $，对应的就是二进制每一位的权重。
{%endnote%}
```cpp
class Solution {
public:
    double myPow(double x, int N) {
        double ans=1;  
        long long n=N;  
        if(n<0){
            // 当n<0时，转换为x^(-n) = 1/(x^n)
            n=-n;      
            x=1/x;  
        }
        while(n){
            if(n&1){// 判断当前二进制位是否为1
                ans*=x;  // 如果是1，将当前x的幂乘到结果中
            }
            x*=x;    // x翻倍，准备下一轮(x^1 -> x^2 -> x^4 -> x^8 ...)
            n>>=1;   // n右移一位，处理下一个二进制位（n = n / 2）
        }
        return ans;
    }
};
```
{%note danger%}
事实上，当$n<0$时，由于有$ x^{-n} = \frac{1}{x^n} $，只需简单转换上述方法仍然适用！
{%endnote%}