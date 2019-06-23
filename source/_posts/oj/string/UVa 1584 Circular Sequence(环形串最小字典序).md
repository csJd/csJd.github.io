---
title: UVa 1584 Circular Sequence(环形串最小字典序)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2014-08-31 09:09:36
---
题意 给你一个环形串 输出它以某一位为起点顺时针得到串的最小字典序

直接模拟 每次后移一位比较字典序即可 注意不能用strcpy(s+1,s)这样后移 strcpy复制地址不能有重叠部分

```js 
#include<cstdio>  
#include<cstring>  
using namespace std;  
const int N = 150;  
char s[N], ans[N], c;  
int t, l;  
  
int main()  
{  
    scanf ("%d", &t);  
    while (t--)  
    {  
        scanf ("%s", s);  
        l = strlen (s);  
        strcpy (ans, s);  
        for (int i = 0; i < l; ++i)  
        {  
            c = s[l - 1];  
            for (int j = l - 1; j >= 1 ; --j)  
                s[j] = s[j - 1];  
            s[0] = c;  
            if (strcmp (s, ans) < 0)  
                strcpy (ans, s);  
        }  
        printf ("%s\n", ans);  
    }  
}
```

Some DNA sequences exist in circular forms as in the following figure, which shows a circular sequence ``CGAGTCAGCT", that is, the last symbol ``T" in ``CGAGTCAGCT" is connected to the first symbol ``C". We always read a circular sequence in the clockwise direction.

![\epsfbox{p3225.eps}](../images/dge.org-external-15-p3225.jpg.png)

Since it is not easy to store a circular sequence in a computer as it is, we decided to store it as a linear sequence. However, there can be many linear sequences that are obtained from a circular sequence by cutting any place of the circular sequence. Hence, we also decided to store the linear sequence that is lexicographically smallest among all linear sequences that can be obtained from a circular sequence.

Your task is to find the lexicographically smallest sequence from a given circular sequence. For the example in the figure, the lexicographically smallest sequence is ``AGCTCGAGTC". If there are two or more linear sequences that are lexicographically smallest, you are to find any one of them (in fact, they are the same).

##

The input consists of *T* test cases. The number of test cases *T* is given on the first line of the input file. Each test case takes one line containing a circular sequence that is written as an arbitrary linear sequence. Since the circular sequences are DNA sequences, only four symbols, A,C, G and T, are allowed. Each sequence has length at least 2 and at most 100.

##

Print exactly one line for each test case. The line is to contain the lexicographically smallest sequence for the test case.

The following shows sample input and output for two test cases.

##

2 CGAGTCAGCT CTCC

##

AGCTCGAGTC CCCT

﻿﻿