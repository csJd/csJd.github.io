---
title: UVa 12563 Jin Ge Jin Qu hao(01背包)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2015-02-07 13:54:46
---
题意 你在KTV还剩t秒钟的时间 你需要在n首歌中选择尽量多的歌使得歌的数量最多的前提下剩下的时间最小

至少要留一秒给劲歌金曲 所以是一个容量为t-1的01背包 d[i][j]表示恰用j秒时间在前i首歌中最多唱多少首 每个状态有两种选择 唱或不唱第i首歌

所以有转移方程**d[i][j]=max(d[i-1][j],d[i-1][j-c[i]]+1)**

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 55, M = 180;
int c[N], d[N * M], t, n;

int main()
{
    int cas, ans;
    scanf("%d", &cas);
    for(int k = 1; k <= cas; ++k)
    {
        memset(d, 0x8f, sizeof(d));
        scanf("%d%d", &n, &t);
        for(int i = 0; i < n; ++i) scanf("%d", &c[i]);
        d[0] = 0;
        for(int i = 0; i < n; ++i)
            for(int j = t - 1; j >= c[i]; --j)
                d[j] = max(d[j], d[j - c[i]] + 1);
        for(int j = ans = t - 1; j >= 0; --j) if(d[j] > d[ans]) ans = j;
        printf("Case %d: %d %d\n", k, d[ans] + 1, ans + 678);
    }
    return 0;
}
```