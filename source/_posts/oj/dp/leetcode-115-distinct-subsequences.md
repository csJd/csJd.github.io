---
title: LeetCode 115 Distinct Subsequences (DP)
tags:
  - Algorithm
categories:
  - OJ
  - DP
date: 2019-06-03 09:13:33
---

这里通过 LeetCode 中的一道动态规划题目（[115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences)）来复习一下动态规划中的正反向递推和滚动数组的使用。

<!--more-->

题意：输出字符串 S 中等于字符串 T 的子序列的个数。

分析：这是一道比较典型的动态规划题目，考虑状态：

 `dp[i][j]` 表示字符串 `s[:i]` 中等于子序列 `t[:j]` 的个数

其中 `s[:i]` 表示字符串 S 的前 i 个字符构成的子串，`t[:j]` 表示字符串 T 的前 j 个字符构成的子串。那么有初始状态 `dp[0][0] = 1`

首先  `dp[i][j]`  肯定可以由 `dp[i-1][j]` 转移，因为 `s[:i-1]` 的子序列肯定是 `s[i]` 的子序列；特别地，若 `s[i-1] == t[j-1]`，`dp[i][j]` 还可以由 `dp[i-1][j-1]` 转移。

那么不难写出逆向递推的方案：

```python
def numDistinct(self, s: str, t: str) -> int:
    ls = len(s)
    lt = len(t)
    # dp[i][j] for numDistinct(s[:i], t[:j])
    dp = [[0] * (lt+1) for i in range(ls+1)]
    dp[0][0] = 1
    for i in range(1, ls+1):
        dp[i][0] = 1
        ri = min(lt, i)
        for j in range(1, ri+1):
            dp[i][j] = dp[i-1][j]
            if(s[i-1] == t[j-1]):
                dp[i][j] += dp[i-1][j-1]

    return dp[ls][lt]
```

还有正向递推的方案：

```python
def numDistinct(self, s: str, t: str) -> int:
    ls = len(s)
    lt = len(t)
    # dp[i][j] for numDistinct(s[:i], t[:j])
    dp = [[0] * (lt+1) for i in range(ls+1)]
    dp[0][0] = 1
    for i in range(ls):
        for j in range(lt+1):
            if j > i:
                break
            dp[i+1][j] += dp[i][j]
            if j < lt and s[i] == t[j]:
                dp[i+1][j+1] += dp[i][j]

    return dp[ls][lt]
```

考虑到 `dp[i][j]` 只依赖 `dp[i-1][j-1]` 和 `dp[i-1]][j]`，实际上没必要使用二维数组来存储状态。利用滚动数组的性质，只使用一维数组就可以。 j 从后往前递推时，可以让 `dp[j]` 在第 i 次循环中表示 `dp[i][j]`，而 `dp[j-1]` 则表示 `dp[i-1][j-1]`，如下：

```python
def numDistinct(self, s: str, t: str) -> int:
    ls = len(s)
    lt = len(t)
    # dp[i][j] for numDistinct(s[:i], t[:j])
    dp = [1] + [0] * lt  # dp[0][0] = 1

    for i in range(1, ls+1):
        # if j > i, dp[i][j] must be zero
        ri = min(lt, i)
        for j in range(ri, 0, -1):
            # dp[j] is dp[i-1][j] here
            if(s[i-1] == t[j-1]):
                # dp[j-1] here is dp[i-1][j-1] in fact
                # following equation represents dp[i][j] += dp[i-1][j-1]
                dp[j] += dp[j-1]

    return dp[lt]  # the value is dp[ls][lt] now
```

# Leetcode 115. Distinct Subsequences

Given a string **S** and a string **T**, count the number of distinct subsequences of **S** which equals **T**.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ACE"` is a subsequence of `"ABCDE"` while `"AEC"` is not).

**Example 1:**

```
Input: S = "rabbbit", T = "rabbit"
Output: 3
Explanation:

As shown below, there are 3 ways you can generate "rabbit" from S.
(The caret symbol ^ means the chosen letters)

rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^
```

**Example 2:**

```
Input: S = "babgbag", T = "bag"
Output: 5
Explanation:

As shown below, there are 5 ways you can generate "bag" from S.
(The caret symbol ^ means the chosen letters)

babgbag
^^ ^
babgbag
^^    ^
babgbag
^    ^^
babgbag
  ^  ^^
babgbag
    ^^^
```



