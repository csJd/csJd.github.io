---
title: POJ 3036 Honeycomb Walk(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-22 20:05:21
---
题意 求蜜蜂在蜂巢跑行n个格子回到原点的方法数

把每个格子抽象成一个点 最多走14步 把初始位置O的坐标设为(15,15) 保围着O的6个点如下图所示 他们到O的距离都是1

![](../images/null)

每个点周围都有这样的6个点 代表相邻的格子 这6个点到中间点的距离为1 令d[k][i][j]表示 从O点走k步到达(i,j)点的方法总数 那么很容易有转移方程 d[k][i][j]=d[k-1][i-1][j-1]+d[k-1][i][j-1]+d[k-1][i+1][j]+d[k-1][i+1][j+1]+d[k-1][i][j+1]+d[k-1][i-1][j] 边界条件为d[0][15][15]=0

```js 
#include<cstdio>
using namespace std;
const int L = 15, X = 30, Y = 30;
int main()
{
    int d[L][X][Y]={0}, n, t;
    d[0][15][15] = 1;
    for (int k = 1; k < L; ++k)
        for (int i = 1; i < X; ++i)
            for (int j = 1; j < Y; ++j)
                d[k][i][j] = d[k - 1][i - 1][j - 1] + d[k - 1][i][j - 1] + d[k - 1][i + 1][j] +
                             d[k - 1][i + 1][j + 1] + d[k - 1][i][j + 1] + d[k - 1][i - 1][j];
    scanf ("%d", &t);
    while (t--)
    {
        scanf ("%d", &n);
        printf ("%d\n", d[n][15][15]);
    }
    return 0;
}
```

Honeycomb Walk

Description

![](../images/es-3036_1.jpg.png)

A bee larva living in a hexagonal cell of a large honeycomb decides to creep for a walk. In each “step” the larva may move into any of the six adjacent cells and after*n*steps, it is to end up in its original cell.

Your program has to compute, for a given*n*, the number of different such larva walks.

Input

The first line contains an integer giving the number of test cases to follow. Each case consists of one line containing an integer*n*, where 1 ≤*n*≤ 14.

Output

For each test case, output one line containing the number of walks. Under the assumption 1 ≤*n*≤ 14, the answer will be less than 231.

Sample Input

2 2 4

Sample Output

6 90

﻿﻿