---
title: ZOJ 1649 Rescue(有敌人迷宫BFS)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-10-16 22:37:02
---
题意 求迷宫中从a的位置到r的位置需要的最少时间 经过'.'方格需要1s 经过‘x’方格需要两秒 '/#'表示墙

由于有1s和2s两种情况 需要在基础迷宫bfs上加些判断

令到达每个点的时间初始为无穷大 当从一个点到达该点用的时间比他本来的时间小时 更新这个点的时间并将这个点入队 扫描完全图就得到答案咯

```js 
#include<cstdio>
#include<cstring>
#include<queue>
using namespace std;

const int N = 205;
char mat[N][N];
int time[N][N], sx, sy;
int dx[4] = {0, 0, -1, 1};
int dy[4] = { -1, 1, 0, 0};
struct grid
{
    int x, y;
    grid(int xx = 0, int yy = 0): x(xx), y(yy) {}
};

void bfs()
{
    memset(time, 0x3f, sizeof(time));
    time[sx][sy] = 0;
    queue<grid> g;
    g.push(grid(sx, sy));

    while(!g.empty())
    {
        grid cur = g.front();
        g.pop();
        int cx = cur.x, cy = cur.y, ct = time[cx][cy];
        for(int i = 0; i < 4; ++i)
        {
            int nx = cx + dx[i], ny = cy + dy[i];
            if(mat[nx][ny] && mat[nx][ny] != '#')
            {
                int tt = ct + 1;
                if(mat[cx][cy] == 'x') ++tt;
                if(tt < time[nx][ny])
                {
                    time[nx][ny] = tt;
                    g.push(grid(nx, ny));
                }
            }
        }
    }
}

int main()
{
    int n, m, ex, ey;
    while (~scanf("%d%d", &n, &m))
    {
        memset(mat, 0, sizeof(mat));
        for(int i = 1; i <= n; ++i)
            scanf("%s", mat[i] + 1);
        for(int i = 1; i <= n; ++i)
            for(int j = 1; j <= m; ++j)
                if(mat[i][j] == 'a') sx = i, sy = j;
                else if(mat[i][j] == 'r') ex = i, ey = j;
        bfs();
        if(time[ex][ey] != time[0][0])
            printf("%d\n", time[ex][ey]);
        else
            printf("Poor ANGEL has to stay in the prison all his life.\n");
    }

    return 0;
}
```