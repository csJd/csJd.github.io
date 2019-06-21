---
title: UVa 1347 Tour(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2015-02-06 20:12:54
---
题意 二维坐标系上有n个点 从第一个点出发经过部分点到达第n个点 再从第n个点回到第一个点 除了第一个点 每个点都经过且仅经过一次 求最短路径长度

还是基础的DP 想出状态转移方程就容易了 d[i][j]表示去的时候经过第i个点回来经过第j个点且1~max(i,j)间的点都已经走过 显然d[i][j]=d[j][i] 所以只用考虑i>j的部分 i<j的部分d[i][j]保存i,j两点之间的距离

第i+1个点要么去的时候走 要么回来的时候走

1. 去的时候走d[i+1][i] + d[i][i+1],d[i][i+1]保存的是距离

2. 回来的时候走**d[i+1][j] + d[j][i+1].****d[i][i+1]**保存的是距离

所以有状态转移方程**d[i][j] = min(d[i+1][i] + d[i][i+1],d[i+1][j] + d[j][j+1]).**

```js 
#include <bits/stdc++.h>
#define l(x) (x[i]-x[j])*(x[i]-x[j])
using namespace std;
const int N = 105;
int x[N], y[N];
double d[N][N];

int main()
{
    int n;
    while(~scanf("%d", &n))
    {
        for(int i = 1; i <= n; ++i)
        {
            scanf("%d%d", &x[i], &y[i]);
            for(int j = 1; j < i; ++j)
                d[j][i] = sqrt(l(x) + l(y));
        }

        for(int i = 1; i < n - 1; ++i) d[n - 1][i] = d[n - 1][n] + d[i][n];
        for(int i = n - 2; i > 1; --i)
        {
            for(int j = 1; j < i; ++j)
                d[i][j] = min(d[i + 1][j] + d[i][i + 1], d[i + 1][i] + d[j][i + 1]);
        }
        printf("%.2lf\n", d[1][2] + d[2][1]);
    }
    return 0;
}
```

John Doe, a skilled pilot, enjoys traveling. While on vacation, he rents a small plane and starts visiting beautiful places. To save money, John must determine the shortest closed tour that connects his destinations. Each destination is represented by a point in the plane *p*i = < *x*i, *y*i > . John uses the following strategy: he starts from the leftmost point, then he goes strictly left to right to the rightmost point, and then he goes strictly right back to the starting point. It is known that the points have distinct *x* -coordinates.

Write a program that, given a set of *n* points in the plane, computes the shortest closed tour that connects the points according to John's strategy.

##

The program input is from a text file. Each data set in the file stands for a particular set of points. For each set of points the data set contains the number of points, and the point coordinates in ascending order of the *x* coordinate. White spaces can occur freely in input. The input data are correct.

##

For each set of data, your program should print the result to the standard output from the beginning of a line. The tour length, a floating-point number with two fractional digits, represents the result.

**Note:** An input/output sample is in the table below. Here there are two data sets. The first one contains 3 points specified by their *x* and *y* coordinates. The second point, for example, has the *x*coordinate 2, and the *y* coordinate 3. The result for each data set is the tour length, (6.47 for the first data set in the given example).

##

3 1 1 2 3 3 1 4 1 1 2 3 3 1 4 2

##

6.47 7.89

****