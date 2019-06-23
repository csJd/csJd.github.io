---
title: UVa 11134 Fabled Rooks（贪心）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2015-02-02 17:19:47
---
题意 在n/*n的棋盘上的n个指定区间上各放1个'车’ 使他们相互不攻击 输出一种可能的方法

行和列可以分开看 就变成了n个区间上选n个点的贪心问题 看行列是否都有解就行

基础的贪心问题 对每个点k选择包含它的最优未使用区间 由于在给k找最优区间时1~k-1的最优区间都已经找好了 所有右界最小的区间肯定是最优区间

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 5005;
int xl[N], yl[N], xr[N], yr[N], x[N], y[N], n;

bool solve(int a[], int l[], int r[])
{
    int cur, mr;
    //mr为包含k的区间最小右界，cur为放k的最优区间
    memset(a, -1, sizeof(int)*n);
    for(int k = 1; k <= n; ++k)
    {
        cur = -1, mr = N;
        for(int i = 0; i < n; ++i)
            if(a[i] < 0 && l[i] <= k && r[i] < mr)
                mr = r[cur = i];
        if(cur < 0 || k > mr)  return 0;
        a[cur] = k;
    }
    return 1;
}

int main()
{
    while(~scanf("%d", &n), n)
    {
        for(int i = 0; i < n; ++i)
            scanf("%d%d%d%d", &xl[i], &yl[i], &xr[i], &yr[i]);

        if(solve(x, xl, xr) && solve(y, yl, yr))
            for(int i = 0; i < n; ++i)
                printf("%d %d\n", x[i], y[i]);
        else puts("IMPOSSIBLE");
    }
    return 0;
}
```

## Fabled Rooks

![](../images/dge.org-external-111-p11134.jpg.png) We would like to place n rooks, 1 ≤ n ≤ 5000, on a n×n board subject to the following restrictions

The input consists of several test cases. The first line of each of them contains one integer number, *n*, the side of the board. *n* lines follow giving the rectangles where the rooks can be placed as described above. The *i*-th line among them gives*xli*, *yli*, *xri*, and *yri*. The input file is terminated with the integer `0' on a line by itself.

Your task is to find such a placing of rooks that the above conditions are satisfied and then output*n* lines each giving the position of a rook in order in which their rectangles appeared in the input. If there are multiple solutions, any one will do. Output IMPOSSIBLE if there is no such placing of the rooks.

8 1 1 2 2 5 7 8 8 2 2 5 5 2 2 5 5 6 3 8 6 6 3 8 5 6 3 8 8 3 6 7 8 8 1 1 2 2 5 7 8 8 2 2 5 5 2 2 5 5 6 3 8 6 6 3 8 5 6 3 8 8 3 6 7 8 0

1 1 5 8 2 4 4 2 7 3 8 5 6 6 3 7 1 1 5 8 2 4 4 2 7 3 8 5 6 6 3 7