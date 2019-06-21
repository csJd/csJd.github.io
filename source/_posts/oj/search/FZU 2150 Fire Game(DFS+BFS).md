---
title: FZU 2150 Fire Game(DFS+BFS)
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2015-04-03 10:16:31
---
题意 在n/*m个格子组成的草地上 你可以选择两个是草('/#')的格子点燃 每个点燃的格子在下一秒其四个相邻的是草的格子也会被点燃 问点燃所有的草至少需要多少秒

DFS和BFS的综合 如果'/#‘连通块的数量大于2个是肯定不能点燃所有的 先dfs判断连通块个数 再bfs找出选哪两个格子可以最快把草烧完

```js 
#include <map>
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 12;
int x[] = {0, 0, -1, 1};
int y[] = { -1, 1, 0, 0};
char g[N][N] , v[N][N];
int  n, m, nu, cnt, ret;
pair<int, int> q[N * N], p[N * N];

void dfs(int r, int c)
{
    if(g[r][c] != '#') return;
    p[nu++] = make_pair(r, c), g[r][c] = cnt;
    for(int i = 0; i < 4; ++i)
        if(r + x[i] >= 0 && c + y[i] >= 0)
            dfs(r + x[i], c + y[i]);
}

void bfs(int r, int c, int cr, int cc)
{
    int le = 0, ri = 0;
    q[ri++] = make_pair(r, c);
    q[ri++] = make_pair(cr, cc);
    memset(v, 0, sizeof(v)), v[r][c] = v[cr][cc] = 1;
    while(le < ri)
    {
        cr = q[le].first, cc = q[le++].second, ret = v[cr][cc];
        for(int i = 0; i < 4; ++i)
        {
            r = cr + x[i], c = cc + y[i];
            if (r >= 0 && r < n && c >= 0 && c < m && g[r][c] != '.' && !v[r][c])
                q[ri++] = make_pair(r, c), v[r][c] = v[cr][cc] + 1;
        }
    }
}

int main()
{
    int cas, ans, r1, c1, r2, c2;
    scanf("%d", &cas);
    for(int k = 1; k <= cas; ++k)
    {
        scanf("%d%d", &n, &m);
        for(int i = 0; i < n; ++i)
            scanf("%s", g[i]);
        printf("Case %d: ", k);

        nu = cnt = ans = 0;
        for(int i = 0; i < n; ++i)
            for(int j = 0; j < m; ++j)
                if(g[i][j] == '#') ++cnt, dfs(i, j);

        if(cnt < 3)
        {
            ans = n * m;
            for(int i = 0; i < nu; ++i)
                for(int j = 0; j < nu; ++j)
                {
                    r1 = p[i].first, c1 = p[i].second;
                    r2 = p[j].first, c2 = p[j].second;
                    if(cnt == 2 && g[r1][c1] == g[r2][c2]) continue;
                    bfs(r1, c1, r2, c2);
                    if(ret < ans) ans = ret;
                }
        }
        printf("%d\n", ans - 1);
    }
    return 0;
}
```

**Problem 2150 Fire Game**
## ![](../images/cn-image-prodesc.gif.png) Problem Description

Fat brother and Maze are playing a kind of special (hentai) game on an N/*M board (N rows, M columns). At the beginning, each grid of this board is consisting of grass or just empty and then they start to fire all the grass. Firstly they choose two grids which are consisting of grass and set fire. As we all know, the fire can spread among the grass. If the grid (x, y) is firing at time t, the grid which is adjacent to this grid will fire at time t+1 which refers to the grid (x+1, y), (x-1, y), (x, y+1), (x, y-1). This process ends when no new grid get fire. If then all the grid which are consisting of grass is get fired, Fat brother and Maze will stand in the middle of the grid and playing a MORE special (hentai) game. (Maybe it’s the OOXX game which decrypted in the last problem, who knows.)

You can assume that the grass in the board would never burn out and the empty grid would never get fire.

Note that the two grids they choose can be the same.

## ![](../images/cn-image-prodesc.gif.png) Input

The first line of the date is an integer T, which is the number of the text cases.

Then T cases follow, each case contains two integers N and M indicate the size of the board. Then goes N line, each line with M character shows the board. “/#” Indicates the grass. You can assume that there is at least one grid which is consisting of grass in the board.

1 <= T <=100, 1 <= n <=10, 1 <= m <=10

## ![](../images/cn-image-prodesc.gif.png) Output

For each case, output the case number first, if they can play the MORE special (hentai) game (fire all the grass), output the minimal time they need to wait after they set fire, otherwise just output -1. See the sample input and output for more details.

## ![](../images/cn-image-prodesc.gif.png) Sample Input

4

3 3
./#.

/#/#/#
./#.

3 3
./#.

/#./#
./#.

3 3
...

/#./#
...

3 3
/#/#/#

../#
/#./#

## ![](../images/cn-image-prodesc.gif.png) Sample Output

Case 1: 1

Case 2: -1
Case 3: 0

Case 4: 2