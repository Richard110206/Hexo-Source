---
title: Dynamic Programming
date: 2025-09-20 20:51:04
tags:
archive: true
math: true
---
 

 将大问题分解为小问题，再从小问题开始自底向上的进行求解。

 - 将数组 dp 称为 dp 表，$dp[i]$ 表示状态 $i$ 对应子问题的解
 - 将最小子问题对应的状态（第1阶和第2阶楼梯）称为**初始状态**
 - 将递推公式$dp[i]=dp[i-1]+dp[i-2]$称为**状态转移方程**

### 最优子结构 

原问题的最优解是从子问题的最优解构建得来的

### 无后效性

当前状态的求解不会受到之前状态的影响，只与当前状态有关

[Leetcode 746.使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs)

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n=cost.size();
        vector <int> dp(n+1);
        dp[0]=dp[1]=0;
        for(int i=2;i<=n;i++){
        dp[i]=min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]);
        }
        return dp[n];
    }
};
```

{%fold into@0-1背包问题%}
给定\(n\)个物品，第\(i\)个物品的重量为\(wgt[i - 1]\)、价值为\(val[i - 1]\)，和一个容量为\(cap\)的背包。每个物品只能选择一次，问在限定背包容量下能放入物品的最大价值。
{%endfold%}

{%fold into@完全背包问题%}
给定\(n\)个物品，第\(i\)个物品的重量为\(wgt[i - 1]\)、价值为\(val[i - 1]\)，和一个容量为\(cap\)的背包。每个物品只能选择一次，问在限定背包容量下能放入物品的最大价值。
{%endfold%}