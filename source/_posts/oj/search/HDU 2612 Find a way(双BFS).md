---
title: HDU 2612 Find a way(双BFS)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Search

date: 2015-03-31 23:16:45
---
题意 在n/*m的地图中 ‘Y’和'M'两个人到某一家KFC(在地图上用"@'表示) 所需的最小时间和是多少 他们每走一步都要11分钟

可以分别以y和m为起点进行一遍bfs 把到每个KFC的时间都存起来 最后看哪家KFC的时间和最小就行了

```js 
#include <map>
#include <cstdio>
#include <algorithm>
using namespace std;

const int N = 205;
int x[] = {0, 0, -1, 1};
int y[] = { -1, 1, 0, 0};
pair<int, int> q[N * N];
int dy[N][N], dm[N][N], ans, n, m;
char g[N][N];

void bfs(int r, int c, char k)
{
    int (&d)[N][N] = (k == 'Y' ? dy : dm); //d为指向dy或dm的引用
    int v[N][N] = {0}, le = 0, ri = 0, cr, cc;
    q[ri++] = make_pair(r, c), v[r][c] = 1, d[r][c] = 0;

    while(le < ri)
    {
        r = q[le].first, c = q[le++].second;
        for(int i = 0; i < 4; ++i)
        {
            cr = r + x[i], cc = c + y[i];
            if(cr >= 0 && cr < n && cc >= 0 && cc < m
                    && v[cr][cc] == 0 && g[cr][cc] != '#')
                q[ri++] = make_pair(cr, cc), d[cr][cc] = d[r][c] + 1, v[cr][cc] = 1;
        }
    }
}

int main()
{
    while(~scanf("%d%d", &n , &m))
    {
        ans = N * N;
        for(int i = 0; i < n; ++i)
            scanf("%s", g[i]);
        for(int i = 0; i < n; ++i)
            for(int j = 0; j < m; ++j)
                if(g[i][j] == 'Y' || g[i][j] == 'M')  bfs(i, j, g[i][j]);

        for(int i = 0; i < n; ++i)
            for(int j = 0; j < m; ++j)
                if(g[i][j] == '@') ans = min(ans, dy[i][j] + dm[i][j]);

        printf("%d\n", ans * 11);
    }
    return 0;
}
```

# Find a way

Problem Description

Pass a year learning in Hangzhou, yifenfei arrival hometown Ningbo at finally. Leave Ningbo one year, yifenfei have many people to meet. Especially a good friend Merceki.
Yifenfei’s home is at the countryside, but Merceki’s home is in the center of city. So yifenfei made arrangements with Merceki to meet at a KFC. There are many KFC in Ningbo, they want to choose one that let the total time to it be most smallest.
Now give you a Ningbo map, Both yifenfei and Merceki can move up, down ,left, right to the adjacent road by cost 11 minutes.
Input

The input contains multiple test cases.
Each test case include, first two integers n, m. (2<=n,m<=200).
Next n lines, each line included m character.
‘Y’ express yifenfei initial position.
‘M’ express Merceki initial position.
‘/#’ forbid road;
‘.’ Road.
‘@’ KCF
Output

For each test case output the minimum total time that both yifenfei and Merceki to arrival one of KFC.You may sure there is always have a KFC that can let them meet.
Sample Input

4 4 Y./#@ .... ./#.. @..M 4 4 Y./#@ .... ./#.. @/#.M 5 5 Y..@. ./#... ./#... @..M. /#.../#
Sample Output

66 88 66