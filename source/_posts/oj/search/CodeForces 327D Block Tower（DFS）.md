---
title: CodeForces 327D Block Tower（DFS）
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2015-03-10 23:25:09
---
题意 给你一个城市的地图 你可以在地图上的 . 上建房子/#上不能建房子 红房子可以装200个人 蓝房子可以装100个人 只有相邻位置有蓝房子时才能建红房子 你也可以拆掉已经建成的房子 拆掉后该点又变成 .

这题想到了就很容易了 因为没有限制要步数最少 可以先把左右的地方都建成蓝房子 然后就变成求连通块的题了 每个蓝房子连通块内依次拆掉建红房子 最终就只剩下一个蓝房子了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 505;
char g[N][N];
stack<pair<int, int> > s;
int x[] = { -1, 1, 0, 0};
int y[] = {0, 0, -1, 1};
int n, m, a, b, cnt, is_first;

int dfs(int i, int j)
{
    int r, c;
    if(is_first) is_first = 0;
    else s.push(make_pair(i, j));
    g[i][j] = 'R', cnt += 3;
    for(int k = 0; k < 4; ++k)
    {
        r = i + x[k], c = j + y[k];
        if(g[r][c] == '.') dfs(r, c);
    }
}

int main()
{
    while(~scanf("%d%d", &n, &m))
    {
        cnt = 0;
        for(int i = 1; i <= n; ++i)
            scanf("%s", g[i] + 1);
        for(int i = 1; i <= n; ++i)
            for(int j = 1; j <= m; ++j)
                if(g[i][j] == '.')  is_first = 1, cnt -= 2, dfs(i, j);
        printf("%d\n", cnt);
        for(int i = 1; i <= n; ++i)
            for(int j = 1; j <= m; ++j)
                if(g[i][j] != '#') printf("B %d %d\n", i, j);

        while(!s.empty())
        {
            a = s.top().first, b = s.top().second;
            printf("D %d %d\nR %d %d\n", a, b, a, b);
            s.pop();
        }
    }
    return 0;
}
```
Block Tower

After too much playing on paper, Iahub has switched to computer games. The game he plays is called "Block Towers". It is played in a rectangular grid with *n* rows and *m* columns (it contains *n* × *m* cells). The goal of the game is to build your own city. Some cells in the grid are big holes, where Iahub can't build any building. The rest of cells are empty. In some empty cell Iahub can build exactly one tower of two following types:

Iahub is also allowed to destroy a building from any cell. He can do this operation as much as he wants. After destroying a building, the other buildings are not influenced, and the destroyed cell becomes empty (so Iahub can build a tower in this cell if needed, see the second example for such a case).

Iahub can convince as many population as he wants to come into his city. So he needs to configure his city to allow maximum population possible. Therefore he should find a sequence of operations that builds the city in an optimal way, so that total population limit is as large as possible.

He says he's the best at this game, but he doesn't have the optimal solution. Write a program that calculates the optimal one, to show him that he's not as good as he thinks.
Input

The first line of the input contains two integers *n* and *m* (1 ≤ *n*, *m* ≤ 500). Each of the next *n* lines contains *m* characters, describing the grid. The *j*-th character in the *i*-th line is '.' if you're allowed to build at the cell with coordinates (*i*, *j*) a tower (empty cell) or '/#' if there is a big hole there.

Output

Print an integer *k* in the first line (0 ≤ *k* ≤ 106) — the number of operations Iahub should perform to obtain optimal result.

Each of the following *k* lines must contain a single operation in the following format:

If there are multiple solutions you can output any of them. Note, that you shouldn't minimize the number of operations.
Sample test(s)

input 2 3 ../# ./#.

output 4 B 1 1 R 1 2 R 2 1 B 2 3
input 1 3 ...

output 5 B 1 1 B 1 2 R 1 3 D 1 2 R 1 2