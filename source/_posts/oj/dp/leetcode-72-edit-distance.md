---
title: LeetCode 72 Edit Distance (DP)
tags:
  - Algorithm
categories:
  - OJ
  - DP
date: 2019-06-13 09:13:33
---

编辑距离 ([LeetCode 72: Edit Distance](https://leetcode.com/problems/edit-distance/)) 指的是将一个字符串 A 转变为另外一个字符串 B 所需的最小操作次数。

<!--more-->

一次操作可以是以下三者之一：

* 删除一个字符
* 插入一个字符
* 替换一个字符

这道题就是求两个字符串的编辑距离，是一个比较经典的动态规划问题。容易想到用 `dp[i][j]` 用来表示 A 字符串的
前 `i` 个字符（`A[:i]`）到 B 字符串的前 `j` 个字符（`B[:j]`）的编辑距离，那么有以下两种情况：

* `A[i] == B[j]`，
  此时 `dp[i][j] = dp[i-1][j-1]`
* `A[i] != B[j]`，
  此时有三种可能操作可以使得 A 当前的子串（`A[:i]`）转换为 B 当前的子串（`B[:j]`）：

  * 删除 `A[i]`，总共所需操作次数为 `dp[i-1][j] + 1`
  * 在 `A[i]` 后插入 `B[j]`, 总共所需操作次数为 `dp[i][j-1] + 1`
  * 替换 `A[i]` 为 `B[j]`，总共所需操作次数为 `dp[i-1][j-1] + 1`

  所以此时有 `dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1`

也容易想到初始状态：一个空串到任意字符串的编辑距离为该串的长度，即 `dp[0][i] = i`

结合上面的状态转移情况不难写出代码:

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        # dp[i][j] -> minDistance(word1[:i], word2[:j])
        len1, len2 = len(word1), len(word2)
        dp = [[0] * (len2+1) for i in range(2)]  # 2 * (len2+1) array

        for j in range(len2+1):
            dp[0][j] = j

        for i in range(1, len1+1):
            cur = i % 2
            dp[cur][0] = i
            for j in range(1, len2+1):
                if word1[i-1] == word2[j-1]:
                    dp[cur][j] = dp[cur-1][j-1]
                else:
                    # min(remove, insert, replace)
                    dp[cur][j] = min(dp[cur-1][j], dp[cur]
                                     [j-1], dp[cur-1][j-1]) + 1

        return dp[len1 % 2][len2]


def main():
    solu = Solution()
    print(solu.minDistance('intention', 'execution'))
    print(solu.minDistance('horse', 'ros'))
    print(solu.minDistance('', 'ros'))
    print(solu.minDistance('', ''))


if __name__ == "__main__":
    main()
```
