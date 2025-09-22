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
