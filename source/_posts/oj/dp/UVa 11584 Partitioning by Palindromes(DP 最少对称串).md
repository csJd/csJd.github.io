---
title: UVa 11584 Partitioning by Palindromes(DP 最少对称串)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-25 15:07:18
---
题意 判断一个串最少可以分解为多少个对称串 一个串从左往后和从右往左是一样的 这个串就称为对沉串

令d[i]表示给定串的前i个字母至少可以分解为多少个对称串 那么对于j=1~i 若(i,j)是一个对称串 那么有 d[i]=min(d[i],d[j-1]+1) 然后就得到答案了

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 1005;

int d[N]; char s[N];
bool isPal (int a, int b)
{
    while (a <= b && s[a] == s[b])
        ++a, --b;
    return a >= b;
}

int main()
{
    int cas, l;
    scanf ("%d", &cas);
    for (int ca = 1; ca <= cas; ++ca)
    {
        scanf ("%s", s + 1);
        l = strlen (s + 1);
        memset (d, 0x3f, sizeof (d));
        d[0] = 0;

        for (int i = 1; i <= l; ++i)
        {
            for (int j = 1; j <= i; ++j)
                if (isPal (j, i))  d[i] = min (d[i], d[j - 1] + 1);
        }
        printf ("%d\n", d[l]);
    }
    return 0;
}
```

## Partitioning by Palindromes

![Can you read upside-down?](../images/dge.org-external-115-p11584-.png)

We say a sequence of characters is a *palindrome*if it is the same written forwards and backwards. For example, 'racecar' is a palindrome, but 'fastcar' is not.

A *partition* of a sequence of characters is a list of one or more disjoint non-empty groups of consecutive characters whose concatenation yields the initial sequence. For example, ('race', 'car') is a partition of 'racecar' into two groups.

Given a sequence of characters, we can always create a partition of these characters such that each group in the partition is a palindrome! Given this observation it is natural to ask: what is the minimum number of groups needed for a given string such that every group is a palindrome?

For example:

Input begins with the number *n* of test cases. Each test case consists of a single line of between 1 and 1000 lowercase letters, with no whitespace within.

For each test case, output a line containing the minimum number of groups required to partition the input into groups of palindromes.

3 racecar fastcar aaadbccb

1 7 3  Kevin Waugh