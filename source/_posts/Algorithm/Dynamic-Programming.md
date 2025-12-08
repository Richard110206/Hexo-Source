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


### Leetcode 122.买卖股票的最佳时机 II

[Leetcode 122.买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii?envType=study-plan-v2&envId=top-interview-150)

#### 方法一：动态规划
每天手上只有持有股票和不持有股票**两种状态**，所以可以用二维数组 $dp$ 来表示，其中$dp[i][0]$表示第$i$天股票的最大收益，$dp[i][1]$表示第$i$天不持有股票的最大收益，那么我们可以写出状态转移方程为：$dp[i][0]=max{dp[i−1][0],dp[i−1][1]+prices[i]}$，$dp[i][1]=max{dp[i−1][1],dp[i−1][0]−prices[i]}$
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
          int n=prices.size();
          int dp[n][2];
          dp[0][0]=0,dp[0][1]=-prices[0];
          for(int i=1;i<n;i++){
            dp[i][0]=max(dp[i-1][0],dp[i-1][1]+prices[i]);
            dp[i][1]=max(dp[i-1][1],dp[i-1][0]-prices[i]);
          }
          return dp[n-1][0];
    }
};
```
由于每次的状态只与前一天的状态有关，所以可以用滚动数组来优化空间复杂度，将二维数组优化为一维数组。
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        int dp0=0,dp1=-prices[0];
        for(int i=0;i<n;i++){
            int newDp0 = max(dp0, dp1 + prices[i]);
            int newDp1 = max(dp1, dp0 - prices[i]);
            dp0 = newDp0;
            dp1 = newDp1;
        }
        return dp0;
    }
};
```
#### 方法二：贪心算法

由于股票的购买没有限制，因此整个问题等价于寻找 $x$ 个不相交的区间 $(l_i, r_i]$ 使得如下的等式最大化
$$\sum_{i=1}^{x} a[r_i] - a[l_i]$$
其中 $l_i$ 表示在第 $l_i$ 天买入，$r_i$ 表示在第 $r_i$ 天卖出。

同时我们注意到对于 $(l_i, r_i]$ 这一个区间贡献的价值 $a[r_i] - a[l_i]$，其实等价于 $(l_i, l_i+1], (l_i+1, l_i+2], \dots, (r_i-1, r_i]$ 这若干个区间长度为 $1$ 的区间的价值和，即$a[r_i] - a[l_i] = (a[r_i] - a[r_i-1]) + (a[r_i-1] - a[r_i-2]) + \dots + (a[l_i+1] - a[l_i])$。因此问题可以简化为找 $x$ 个长度为 $1$ 的区间 $(l_i, l_i+1]$ 使得$\sum_{i=1}^{x} a[l_i+1] - a[l_i]$价值最大化。

贪心的角度考虑，我们每次选择贡献大于 $0$ 的区间即能使得答案最大化，因此最后答案为
$$\text{ans} = \sum_{i=1}^{n-1} \max\{0, a[i] - a[i-1]\}$$
其中 $n$ 为数组的长度。

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        int profit=0;
        for(int i=1;i<n;i++){
            profit+=max(0,prices[i]-prices[i-1]);
        }
        return profit;
    }
};
```