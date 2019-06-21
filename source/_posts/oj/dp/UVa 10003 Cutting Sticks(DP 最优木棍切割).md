---
title: UVa 10003 Cutting Sticks(DP 最优木棍切割)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-25 21:29:38
---
题意 要把一根长为l的木棍切成n+1段 切割的位置分别为c[1],c[2],...,c[n] 每次切割的成本为当前切割的那一段的长度 如长度100的切成两段成本为100 求完成切割的最小成本

和最优矩阵链乘那题很像 不用打印路径更简单了 在木棍上加两个点c[0]=0 c[n+1]=l d[i][j]表示把这段木棍的第i到j个点完成切割的最小成本 如果j=i+1 不需要切割 成本为0 这是边界条件 j>i+1时 可以在i和j间取一点k 那么有d[i][j]=min{d[i][k]+d[k][j]+c[j]-c[i]} 即在k处切断 这个切割的成本为c[j]-c[i]

所以有转移方程d[i][j]=min{d[i][k]+d[k][j]+c[j]-c[i]} i<k<j

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 55, INF = 0x3f3f3f3f;
int d[N][N], c[N];

int dp (int i, int j)
{
    if (d[i][j] < INF)  return d[i][j];
    for (int k = i+1; k < j; ++k)
        d[i][j] = min (d[i][j], dp (i, k) + dp (k , j) + c[j] - c[i]);
    return d[i][j];
}

int main()
{
    int l, n;
    while (scanf ("%d", &l), l)
    {
        scanf ("%d", &n);
        for (int i = 1; i <= n; ++i)
            scanf ("%d", &c[i]);
        c[++n] = l;
        memset (d, 0x3f, sizeof (d));
        for (int i = 0; i <= n-1; ++i) d[i][i+1]=0;
        printf ("The minimum cutting is %d.\n", dp (0, n));
    }
    return 0;
}
```

#

****

You have to cut a wood stick into pieces. The most affordable company, The Analog Cutting Machinery, Inc. (ACM), charges money according to the length of the stick being cut. Their procedure of work requires that they only make one cut at a time.

It is easy to notice that different selections in the order of cutting can led to different prices. For example, consider a stick of length 10 meters that has to be cut at 2, 4 and 7 meters from one end. There are several choices. One can be cutting first at 2, then at 4, then at 7. This leads to a price of 10 + 8 + 6 = 24 because the first stick was of 10 meters, the resulting of 8 and the last one of 6. Another choice could be cutting at 4, then at 2, then at 7. This would lead to a price of 10 + 4 + 6 = 20, which is a better price.

Your boss trusts your computer abilities to find out the minimum cost for cutting a given stick.

##

The input will consist of several input cases. The first line of each test case will contain a positive number*l*that represents the length of the stick to be cut. You can assume*l*< 1000. The next line will contain the number*n*(*n*< 50) of cuts to be made.

The next line consists of*n*positive numbers*c**i*(0 <*c**i*<*l*) representing the places where the cuts have to be done, given in strictly increasing order.

An input case with*l*= 0 will represent the end of the input.

##

You have to print the cost of the optimal solution of the cutting problem, that is the minimum cost of cutting the given stick. Format the output as shown below.

##

100 3 25 50 75 10 4 4 5 7 8 0

##

The minimum cutting is 200. The minimum cutting is 22.

﻿﻿