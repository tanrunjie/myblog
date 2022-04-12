---
title: 九章基础class_5_6
categories:
  - 九章算法
mathjax: true
date: 2022-04-08 15:01:35
tags: 九章算法
---

### 第五课 双序列DP
再次强调，可能为DP的问题： 记忆化搜索
- Max/Min问题
- True/False问题
- 计数问题
可能不是DP问题的
- 需要所有“具体”的方案 
- 输入是“集合”而非“序列”

例题：a,b两串的Longest Common Sequence的长度
state: dp[i][j], a前i子串匹配b前j子串的LCS长度
function: dp[i][j] = dp[i-1][j-1] + 1 if a[i]==b[j]
dp[i][j] = max(dp[i-1][j],dp[i][j-1]) if a[i]!=b[j]
init: dp[i][0]=0, dp[0][j] = 0;
ans:dp[a.size()][b.size()]

重点：序列a,b挑出i,j子序列的dp
- dp[i][j]定义
- dp[i][j]的转换以及从i,j为结尾的情况考虑

总结：
- 归纳可能使用或不使用DP的规律
- DP四要素：状态，转换方程，初始化和答案
- 三种常见面试DP：矩阵，单序列，双序列
- 奇技淫巧：初始化首行首列， N个数开N+1位置的数组

#### 背包问题
定义：n个整数，装m的背包
state: f[i][j]“前i”个数，能否组成和为j
function: f[i][j] = f[i-1][j-a[i]] or f[i-1][j]
init: f[x][0] = true; f[0][1-m] = false;
ans: f[n][x]为true 的最大值的x

当前行只跟上一行i-1有关：滚动数组优化，用模2; 如与i-1, i-2有关，则模3；如此类推。

TO DO：较难，待补充具体解法

### 第六课