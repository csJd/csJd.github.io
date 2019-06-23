---
title: UVa 572 Oil Deposits(DFS)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Search

date: 2014-09-02 09:50:44
---
#

****

The GeoSurvComp geologic survey company is responsible for detecting underground oil deposits. GeoSurvComp works with one large rectangular region of land at a time, and creates a grid that divides the land into numerous square plots. It then analyzes each plot separately, using sensing equipment to determine whether or not the plot contains oil.

A plot containing oil is called a pocket. If two pockets are adjacent, then they are part of the same oil deposit. Oil deposits can be quite large and may contain numerous pockets. Your job is to determine how many different oil deposits are contained in a grid.

##

The input file contains one or more grids. Each grid begins with a line containing m and n , the number of rows and columns in the grid, separated by a single space. If m = 0 it signals the end of the input; otherwise  ![$1 \le m \le 100$](../images/dge.org-external-5-572img1.gif.png) and  ![$1 \le n \le 100$](../images/dge.org-external-5-572img2.gif.png) . Following this are m lines of n characters each (not counting the end-of-line characters). Each character corresponds to one plot, and is either ` /* ', representing the absence of oil, or ` @ ', representing an oil pocket.

##

For each grid, output the number of distinct oil deposits. Two different pockets are part of the same oil deposit if they are adjacent horizontally, vertically, or diagonally. An oil deposit will not contain more than 100 pockets.

##

1 1 /* 3 5 /*@/*@/* /*/*@/*/* /*@/*@/* 1 8 @@/*/*/*/*@/* 5 5 /*/*/*/*@ /*@@/*@ /*@/*/*@ @@@/*@ @@/*/*@ 0 0

##

0 1 2 2

题意 计算@连通块的数量 典型的dfs应用

```js 
#include<cstdio>
#include<cstring>
using namespace std;
#define r i+x[k]
#define c j+y[k]
const int N = 105;
char mat[N][N];
int n, x[8] = { -1, -1, -1, 0, 0, 1, 1, 1};
int m, y[8] = { -1, 0, 1, -1, 1, -1, 0, 1};

int dfs (int i, int j)
{
    if (mat[i][j] == '*') return 0;
    mat[i][j] = '*';
    for (int k = 0; k < 8; ++k)
        if (r > 0 && r <= n && c > 0 && c <= m && mat[r][c] == '@')
            dfs (r, c);
    return 1;
}

int main()
{
    while (scanf ("%d%d", &n, &m), n)
    {
        int ans = 0;
        for (int i = 1; i <= n; ++i)
            scanf ("%s", mat[i] + 1);
        for (int i = 1; i <= n; ++i)
            for (int j = 1; j <= m; ++j)
                ans += dfs (i, j);
        printf ("%d\n", ans);
    }
    return 0;
}
```