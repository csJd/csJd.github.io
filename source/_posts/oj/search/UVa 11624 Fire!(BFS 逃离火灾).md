---
title: UVa 11624 Fire!(BFS 逃离火灾)
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2015-04-03 00:58:31
---
题意 n/*m的迷宫中有一些着火点 每个着火点上下左右相邻的非墙点下一秒也将成为一个着火点 Joe每秒能向相邻的点移动一步 给你所有着火点的位置和Joe的位置 问Joe逃离这个迷宫所需的最小时间

可以先一遍bfs把每个点的最早着火时间存起来 只有Joe到达该点的时间小于这个时间Joe才能走这个点 只需要对Joe所在的点为起点再来一次bfs就行了 需要注意的是开始可能有多个着火点 我开始以为只有一个着火点被坑了好久

v[i][j]第一遍bfs记录点i,j的最早着火时间 第二遍bfs记录Joe到达该点的时间

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 1005;
int x[] = {0, 0, -1, 1};
int y[] = { -1, 1, 0, 0};
int v[N][N], n, m, ans, le, ri;
pair<int, int> q[N * N];
char g[N][N];

void bfs()
{
    int cr, cc, r, c;
    while(le < ri)
    {
        cr = q[le].first, cc = q[le++].second;
        for(int i = 0; i < 4; ++i)
        {
            r = cr + x[i], c = cc + y[i];
            if(r < 0 || r >= n || c < 0 || c >= m)
                ans = ans ? ans : v[cr][cc];
            else if(g[r][c] == '.' && (!v[r][c] || v[cr][cc] + 1 < v[r][c]))
                v[r][c] = v[cr][cc] + 1, q[ri++] = make_pair(r, c);
        }
    }
}

int main()
{
    int cas, jr, jc;
    scanf("%d", &cas);
    while(cas--)
    {
        scanf("%d%d", &n, &m);
        for(int i = 0; i < n; ++i)
            scanf("%s", g[i]);

        memset(v, 0, sizeof(v));
        le = ri = 0;
        for(int i = 0; i < n; ++i)
            for(int j = 0; j < m; ++j)
                if(g[i][j] == 'F')
                    q[ri++] = make_pair(i, j), v[i][j] = 1;
                else if(g[i][j] == 'J') jr = i, jc = j;
        bfs();
        le = ri = ans = 0, v[jr][jc] = 1;
        q[ri++] = make_pair(jr, jc);
        bfs();
        if(ans) printf("%d\n", ans);
        else puts("IMPOSSIBLE");
    }
    return 0;
}
```
题目链接 [http://uva.onlinejudge.org/external/116/11624.pdf](http://uva.onlinejudge.org/external/116/11624.pdf)