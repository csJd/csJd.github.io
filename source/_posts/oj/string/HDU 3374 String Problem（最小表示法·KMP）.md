---
title: HDU 3374 String Problem（最小表示法·KMP）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2015-10-06 10:43:25
---
题意 给你一个环形串 输出其最小表示法的首字母位置 最大表示法的首字母位置 以及和对应位置等价位置的个数

最小表示法指一个循环串以某一位开始时对应的串的字典序最小 这个串就是该循环串的最小表示法

先看一下求字符串最小表示法的过程 可以看2003年国家集训队论文集中周源的论文

令 p 表示字符串 s 的最小表示法的下标, l = strlen(s) s = s + s

当**i <= p && j <= p && i ！= j**时 令 k (0 <= k < l) 为最小使得 s[i + k] != s[j + k] 的整数

k 存在时 若 s[i + k] > s[j + k]

那么对于任意的**x (i <= x <= i + k)**都是不可能等于p的因为对于这些 x 都有对应的 y (j <= y <= j + k) 使得**s[x, i + k) == s[y, j + k) && s[i + k] > s[j + k]**那么以 y 为起始位置的串肯定是小于以 x 为起始位置的串的 那么就可以令 i = i + k + 1继续这个过程了 s[i + k] < s[j + k] 时同理可令 j = j + k + 1 注意若前进后i == j 需要令 j = j + 1 因为要保证i != j

重复上面的过程直到 i >= l || j >= l || k不存在 此时 i, j 中小的那个数就是答案了 因为小的那个数前后所有都已经被确定不可能为 p

开始时不妨令i = 0, j = 1 p > 0 时是符合上面的条件的 p == 0 时可以发现 i 会保持0不变 j 一直前进直到 j > l 或 k 不存在

那么令 i = 0, j = 1 然后通过上面的过程就能得到最小表示法对应的位置了 同理也可以得到最大表示法对应的位置

至于求等价位置的个数 只要知道这个串是某个串循环了多少次就行了 通过kmp的l - next[l]表示最小循环节的长度就可以求出来

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 2000005;
int nex[N], l;
char s[N];

void getNext()
{
    int i = 0, j = nex[0] = -1;
    while(i < l)
    {
        if(j == -1 || s[i] == s[j])
            nex[++i] = ++j;
        else j = nex[j];
    }
}

int getPos(bool op) //op = 0最小表示法  op = 1 最大表示法
{
    strncpy(s + l, s, l);
    int i = 0, j = 1;
    while(i < l && j < l)
    {
        int k = 0;
        while(k < l && s[i + k] == s[j + k]) ++k;
        if(k >= l) break;
        if((s[i + k] > s[j + k]) ^ op) i += k + 1;
        else j += k + 1;
        if(i == j) ++j;  //保证i != j
    }
    return i < j ? i : j;
}

int main()
{
    while(~scanf("%s", s))
    {
        l = strlen(s);
        getNext();
        int rl = l - nex[l], t = l % rl ? 1 : l / rl;
        int p0 = getPos(0) + 1, p1 = getPos(1) + 1;
        printf("%d %d %d %d\n", p0, t, p1, t);
    }
    return 0;
}
//Last modified :  2015-10-06 09:30 CST
```

# String Problem

Problem Description

Give you a string with length N, you can generate N strings by left shifts. For example let consider the string “SKYLONG”, we can generate seven strings:
String Rank
SKYLONG 1
KYLONGS 2
YLONGSK 3
LONGSKY 4
ONGSKYL 5
NGSKYLO 6
GSKYLON 7
and lexicographically first of them is GSKYLON, lexicographically last is YLONGSK, both of them appear only once.
Your task is easy, calculate the lexicographically fisrt string’s Rank (if there are multiple answers, choose the smallest one), its times, lexicographically last string’s Rank (if there are multiple answers, choose the smallest one), and its times also.
Input

Each line contains one line the string S with length N (N <= 1000000) formed by lower case letters.
Output

Output four integers separated by one space, lexicographically fisrt string’s Rank (if there are multiple answers, choose the smallest one), the string’s times in the N generated strings, lexicographically last string’s Rank (if there are multiple answers, choose the smallest one), and its times also.
Sample Input

abcder aaaaaa ababab
Sample Output

1 1 6 1 1 6 1 6 1 3 2 3
Author

WhereIsHeroFrom