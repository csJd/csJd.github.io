---
title: UVa 437 The Tower of Babylon(DP 最长条件子序列)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-23 10:50:52
---
﻿﻿

题意 给你n种长方体 每种都有无穷个 当一个长方体的长和宽都小于另一个时 这个长方体可以放在另一个上面 要求输出这样累积起来的最大高度

因为每个长方体都有3种放法 比较不好控制 可以把一个长宽高分成三个长方体 高度是固定的 这样就比较好控制了

```js 
#include<cstdio>  
#include<cstring>  
#include<algorithm>  
using namespace std;  
#define maxn 105  
int x[maxn], y[maxn], z[maxn], d[maxn], n;  
int dp(int i)  
{  
    if(d[i] > 0) return d[i];  
    d[i] = z[i];  
    for(int j = 1; j <= n; ++j)  
    {  
        if((x[i] > x[j] && y[i] > y[j]) || (x[i] > y[j] && y[i] > x[j]))  
            d[i] = max(d[i], dp(j) + z[i]);  
    }  
    return d[i];  
}  
  
int main()  
{  
    int a, b, c, cas = 1;  
    while (scanf("%d", &n), n)  
    {  
        n *= 3;  
        for(int i = 1; i <= n;)  
        {  
            scanf("%d%d%d", &a, &b, &c);  
            x[i] = a; y[i] = b; z[i++] = c;  
            x[i] = a; y[i] = c; z[i++] = b;  
            x[i] = b; y[i] = c; z[i++] = a;  
        }  
  
        int ans = 0;  
        memset(d, 0, sizeof(d));  
        for(int i = 1; i <= n; ++i)  
            ans = max(dp(i), ans);  
        printf("Case %d: maximum height = %d\n", cas, ans);  
  
        cas++;  
    }  
    return 0;  
}
```

#

****

Perhaps you have heard of the legend of the Tower of Babylon. Nowadays many details of this tale have been forgotten. So now, in line with the educational nature of this contest, we will tell you the whole story:

The babylonians had *n* types of blocks, and an unlimited supply of blocks of each type. Each type-*i* block was a rectangular solid with linear dimensions ![tex2html_wrap_inline32](../images/dge.org-external-4-437img1.gif.png) . A block could be reoriented so that any two of its three dimensions determined the dimensions of the base and the other dimension was the height. They wanted to construct the tallest tower possible by stacking blocks. The problem was that, in building a tower, one block could only be placed on top of another block as long as the two base dimensions of the upper block were both strictly smaller than the corresponding base dimensions of the lower block. This meant, for example, that blocks oriented to have equal-sized bases couldn't be stacked.

Your job is to write a program that determines the height of the tallest tower the babylonians can build with a given set of blocks.

##

The input file will contain one or more test cases. The first line of each test case contains an integer *n*, representing the number of different blocks in the following data set. The maximum value for *n* is 30. Each of the next *n* lines contains three integers representing the values ![tex2html_wrap_inline40](../images/dge.org-external-4-437img2.gif.png) , ![tex2html_wrap_inline42](../images/dge.org-external-4-437img3.gif.png) and ![tex2html_wrap_inline44](../images/dge.org-external-4-437img4.gif.png) .

Input is terminated by a value of zero (0) for *n*.

For each test case, print one line containing the case number (they are numbered sequentially starting from 1) and the height of the tallest possible tower in the format "Case *case*: maximum height = *height*"

##

1 10 20 30 2 6 8 10 5 5 5 7 1 1 1 2 2 2 3 3 3 4 4 4 5 5 5 6 6 6 7 7 7 5 31 41 59 26 53 58 97 93 23 84 62 64 33 83 27 0

##

Case 1: maximum height = 40 Case 2: maximum height = 21 Case 3: maximum height = 28 Case 4: maximum height = 342