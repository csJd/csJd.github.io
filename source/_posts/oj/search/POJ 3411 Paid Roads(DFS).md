---
title: POJ 3411 Paid Roads(DFS)
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2015-08-03 16:04:47
---
题意 你要从第1个城市到第N个城市去 有m条路 每条路用a, b, c, p, r 表示 你从第a个城市到第b个城市时 若之前经过或现在位于第c个城市 过路费就是p元 否则就是r元 求你到达第N个城市最少用多少过路费

由于最多只有10个城市 10条路 这个题就变得很简单了 直接暴力dfs就行 可以用状态压缩来存储已经走过了哪些城市 由于最多只有10条路 从某个城市出发要一条 回这个城市也要一条 所以一个城市最多经过5次 这个可以作为dfs的结束条件

```js 
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;

const int N = 11, INF = 2333333;
int n, m, ans, vis[N];
struct road{
    int a, b, c, p, r;
} rd[N];

//当前所在城市, 到过哪些城市, 当前已经用了多少过路费
void dfs(int p, int s, int v)
{
    if(vis[p] > 5) return;
    s = s | 1 << (p - 1);
    if(p == n)
    {
        ans = min(ans, v);
        return;
    }

    for(int i = 0; i < m; ++i)
    {
        if(rd[i].a != p) continue;
        ++vis[rd[i].b];
        if(s & 1 << (rd[i].c - 1)) //到过城市c
            dfs(rd[i].b, s, v + rd[i].p);
        else dfs(rd[i].b, s, v + rd[i].r);
        --vis[rd[i].b]; //回溯
    }
}

int main()
{
    while(~scanf("%d%d", &n, &m))
    {
        for(int i = 0; i < m; ++i)
            scanf("%d%d%d%d%d", &rd[i].a, &rd[i].b, &rd[i].c, &rd[i].p, &rd[i].r);

        ans = INF;
        memset(vis, 0, sizeof(vis));
        dfs(1, 0, 0);
        if(ans == INF) puts("impossible");
        else printf("%d\n", ans);
    }

    return 0;
}
```
 Paid Roads

Description

A network of **m** roads connects **N** cities (numbered from 1 to **N**). There may be more than one road connecting one city with another. Some of the roads are paid. There are two ways to pay for travel on a paid road **i** from city **ai** to city **bi**:

The payment is **Pi** in the first case and **Ri** in the second case.

Write a program to find a minimal-cost route from the city 1 to the city **N**.

Input

The first line of the input contains the values of **N** and **m**. Each of the following **m** lines describes one road by specifying the values of **ai**, **bi**, **ci**, **Pi**, **Ri** (1 ≤ **i**≤ **m**). Adjacent values on the same line are separated by one or more spaces. All values are integers, 1 ≤ **m, N** ≤ 10, 0 ≤ **Pi** , **Ri** ≤ 100, **P**i ≤ **Ri** (1 ≤ **i**≤ **m**).

Output

The first and only line of the file must contain the minimal possible cost of a trip from the city 1 to the city **N**. If the trip is not possible for any reason, the line must contain the word ‘**impossible**’.

Sample Input

4 5 1 2 1 10 10 2 3 1 30 50 3 4 3 80 80 2 1 2 10 10 1 3 2 10 50

Sample Output

110

Source

[Northeastern Europe 2002](http://poj.org/searchproblem?field=source&key=Northeastern+Europe+2002), Western Subregion