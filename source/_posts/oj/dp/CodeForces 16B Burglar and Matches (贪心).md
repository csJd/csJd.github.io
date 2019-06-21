---
title: CodeForces 16B Burglar and Matches (贪心)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-28 20:56:41
---
题意 一个窃贼到火柴仓库偷火柴 仓库有m个容器 第i个容器有a[i]个火柴盒 其中每个火柴盒中有b[i]根火柴 窃贼最大可以拿n个火柴盒 输入n m 然后m行a[i] b[i] 求窃贼最多能偷多少根火柴

很水的贪心 直接每次选当前火柴最多的盒子 n减去盒子数 直到n=0

```js 
#include<cstdio>  
#include<cstring>  
#include<algorithm>  
using namespace std;  
const int M = 25;  
int a[M], b[M], c[M], n, m, ans;  
  
bool cmp (int i, int j)  
{  
    return b[i] > b[j];  
}  
  
int main()  
{  
    scanf ("%d%d", &n, &m);  
    for (int i = 1; i <= m; ++i)  
    {  
        c[i] = i;  
        scanf ("%d%d", &a[i], &b[i]);  
    }  
    sort (c + 1, c + m + 1, cmp);  
    ans = 0;  
    for (int i = 1; i <= m; ++i)  
    {  
        if (n >= a[c[i]])  
        {  
            n -= a[c[i]];  
            ans += a[c[i]] * b[c[i]];  
        }  
        else  
        {  
            ans += n * b[c[i]];  
            break;  
        }  
    }  
    printf ("%d\n", ans);  
    return 0;  
}
```

A burglar got into a matches warehouseand wants to steal as many matches as possible. In the warehouse there are *m*containers, in the *i*-th container there are *a**i* matchboxes, and each matchbox contains *b**i* matches. All the matchboxes are of the same size. The burglar's rucksack can hold *n* matchboxes exactly. Your task is to find out the maximum amount of matches that a burglar can carry away. He has no time to rearrange matches in the matchboxes, that's why he just chooses not more than *n* matchboxes so that the total amount of matches in them is maximal.
Input

The first line of the input contains integer *n* (1 ≤ *n* ≤ 2·108) and integer *m* (1 ≤ *m* ≤ 20). The *i* + 1-th line contains a pair of numbers *a**i* and *b**i* (1 ≤ *a**i* ≤ 108, 1 ≤ *b**i* ≤ 10). All the input numbers are integer.

Output

Output the only number — answer to the problem.
Sample test(s)

input 7 3 5 10 2 5 3 6

output 62
input 3 3 1 3 2 2 3 1

output 7

﻿﻿