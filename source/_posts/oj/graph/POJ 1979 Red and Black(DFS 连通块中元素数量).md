---
title: POJ 1979 Red and Black(DFS 连通块中元素数量)
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2014-10-13 11:36:27
---
题意 求矩阵中包含‘@’的'.'连通块中元素数量 '@'也看做'.'

最基础的dfs了

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 30;
char mat[N][N];
int dx[4] = {0, 0, -1, 1}, dy[4] = { -1, 1, 0, 0};
int ans;

void dfs(int r, int c)
{
    if(mat[r][c] != '.') return;
    mat[r][c] = '#';
    for(int i = 0; i < 4; ++i)
    {
        int x = r + dx[i], y = c + dy[i];
        if(mat[x][y] == '.')
        {
            dfs(x, y);
            ++ans;
        }
    }
}

int main()
{
    int m, n, sx, sy;
    while(scanf("%d%d", &m, &n), m)
    {
        memset(mat, 0, sizeof(mat));
        for(int i = 1; i <= n; ++i)
            scanf("%s", mat[i] + 1);
        for(int i = 1; i <= n; ++i)
            for(int j = 1; j <= m; ++j)
                if(mat[i][j] == '@') sx = i, sy = j;
        ans = 1;
        dfs(sx, sy);
        printf("%d\n", ans);
    }
}
```

Red and Black

Description
There is a rectangular room, covered with square tiles. Each tile is colored either red or black. A man is standing on a black tile. From a tile, he can move to one of four adjacent tiles. But he can't move on red tiles, he can move only on black tiles.
Write a program to count the number of black tiles which he can reach by repeating the moves described above.

Input

The input consists of multiple data sets. A data set starts with a line containing two positive integers W and H; W and H are the numbers of tiles in the x- and y- directions, respectively. W and H are not more than 20.
There are H more lines in the data set, each of which includes W characters. Each character represents the color of a tile as follows.
'.' - a black tile
'/#' - a red tile
'@' - a man on a black tile(appears exactly once in a data set)
The end of the input is indicated by a line consisting of two zeros.

Output

For each data set, your program should output a line which contains the number of tiles he can reach from the initial tile (including itself).

Sample Input

6 9 ..../#. ...../# ...... ...... ...... ...... ...... /#@.../# ./#../#. 11 9 ./#......... ./#./#/#/#/#/#/#/#. ./#./#...../#. ./#./#./#/#/#./#. ./#./#..@/#./#. ./#./#/#/#/#/#./#. ./#......./#. ./#/#/#/#/#/#/#/#/#. ........... 11 6 ../#../#../#.. ../#../#../#.. ../#../#../#/#/# ../#../#../#@. ../#../#../#.. ../#../#../#.. 7 7 ../#./#.. ../#./#.. /#/#/#./#/#/# ...@... /#/#/#./#/#/# ../#./#.. ../#./#.. 0 0

Sample Output

45 59 6 13