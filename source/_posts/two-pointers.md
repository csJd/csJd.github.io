---
title: 双指针技巧的应用
tags:
  - Algorithm
categories:
  - Review
date: 2019-07-18 16:25:01
---

这里对笔试题中常用到的「双指针技巧」进行简单的归纳，后续遇到会继续补充。
<!--more-->

## LeetCode 文章 「[常用的双指针技巧](https://leetcode-cn.com/circle/article/GMopsy)」

* 判定链表中是否含有环
* 已知链表中含有环，返回这个环的起始位置
* 寻找链表的中点
* 寻找链表的倒数第 k 个元素
* [滑动窗口解决子串问题](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/solution/hua-dong-chuang-kou-tong-yong-si-xiang-jie-jue-zi-/)
* [LeetCode 相关题目](https://leetcode.com/tag/two-pointers/)

## 数组相关问题

* 调整数组顺序使得满足某种条件的数（如奇数）位于不满足条件的前面
* 排序数组中和为 s 的两个数字
* 排序数组中和为 s 的所有子数组
* 和为 s 的连续正数序列

## 笔试题记录

* 网易游戏笔试题 (190811）：

  > 你可以最多改变两个大写字母，使得字符串所包含的连续的 N 串的长度最长，求最长长度

  ```python
  """
  5
  NNTN
  NNNNGGNNNN
  NGNNNNGNNNNNNNNSNNNN
  TNT
  T

  4
  10
  18
  3
  1
  """


  def main():
      # two pointers solution
      n_cases = int(input())
      while n_cases > 0:
          n_cases -= 1
          s = input().strip()
          length = len(s)

          le = ri = 0
          max_length = 0
          cnt = 0  # the count of non-'N's in current [le, ri] window
          while ri < length:
              if cnt > 2:
                  if s[le] != 'N':
                      cnt -= 1
                  le += 1
              else:
                  if s[ri] != 'N':
                      cnt += 1
                  ri += 1
                  max_length = max(max_length, ri-le)

          print(max_length)


  if __name__ == "__main__":
      main()
  ```

* 网易云音乐面试题（190817）

  > 给定一个字符串，请你找出其中不含有重复字符的最长子串

  ```python
  """
  输入：原始字符串
  输出：最长子串

  input: abcabcaa
  output: abc
  input: ddddddd
  output: d
  input: skkwek
  output: kwe
  """


  s = input()
  length = len(s)

  le = ri = 0
  window = set()
  ans = [0, 0]
  while ri < length:
      while s[ri] in window:
          window.remove(s[le])
          le += 1
      window.add(s[ri])
      ri += 1
      if len(window) > ans[1] - ans[0] + 1:
          ans[0], ans[1] = le, ri

  print(s[ans[0]:ans[1]+1])
  ```

* 字节跳动面试题（190818）

  > 求 arr[j] - arr[i] 的最大值， j >= i

  ```python
  """
  1 3 2 4 5 0 2 3 7
  7
  """


  def main():
      arr = list(map(int, input().split()))
      i = j = 0
      ans = 0
      while j < len(arr):
          if arr[j] <= arr[i]:
              i = j
          ans = max(ans, arr[j] - arr[i])
          j += 1
      print(ans)


  if __name__ == "__main__":
      main()
  ```
