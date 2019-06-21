---
title: POJ 3356 AGTC（最长公共子序列）
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-09-02 09:51:05
---
AGTC

Description
Let *x* and *y* be two strings over some finite alphabet *A*. We would like to transform *x* into *y* allowing only operations given below:

Certainly, we would like to minimize the number of all possible operations.
**Illustration**

A G T A A G T /* A G G C | | | | | | | A G T /* C /* T G A C G C

**Deletion:** /* in the bottom line
**Insertion:** /* in the top line
**Change:** when the letters at the top and bottom are distinct

This tells us that to transform *x* = AGTCTGACGC into *y* = AGTAAGTAGGC we would be required to perform 5 operations (2 changes, 2 deletions and 1 insertion). If we want to minimize the number operations, we should do it like

A G T A A G T A G G C | | | | | | | A G T C T G /* A C G C

and 4 moves would be required (3 changes and 1 deletion).

In this problem we would always consider strings *x* and *y* to be fixed, such that the number of letters in *x* is *m* and the number of letters in *y* is *n* where *n* ≥ *m*.

Assign 1 as the cost of an operation performed. Otherwise, assign 0 if there is no operation performed.

Write a program that would minimize the number of possible operations to transform any string *x* into a string *y*.

Input

The input consists of the strings *x* and *y* prefixed by their respective lengths, which are within 1000.

Output

An integer representing the minimum number of possible operations to transform any string *x* into a string *y*.

Sample Input

10 AGTCTGACGC 11 AGTAAGTAGGC

Sample Output

4

题意 给你两个DNA序列 求第一个第一个序列至少经过多次删除 、替换 或添加碱基得到第二个序列 其实分析一下可以发现 只要求出两个序列的最长公共子序列 这部分就可以不动了 然后较长序列的长度减去最长公共子序列的长度就是答案了

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 1005;
int la, lb, d[N][N];
char a[N], b[N];

void lcs()
{
    memset (d, 0, sizeof (d));
    for (int i = 1; i <= la; ++i)
        for (int j = 1; j <= lb; ++j)
            if (a[i] == b[j]) d[i][j] = d[i - 1][j - 1] + 1;
            else d[i][j] = max (d[i - 1][j], d[i][j - 1]);
}

int main()
{
    while (~scanf ("%d%s%d%s", &la, a + 1, &lb, b + 1))
    {
        lcs();
        printf ("%d\n", max (la, lb) - d[la][lb]);
    }
    return 0;
}
```