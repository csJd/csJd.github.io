---
title: UVa 572 Oil Deposits(DFS求8连通块)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2014-10-10 14:38:24
---
题意 求n/*m矩阵中'@'连通块的个数 两个‘@’在一个九宫格内就属于一个连通块

最基础的DFS 遇到@就递归扫描周围8个并标记当前格子已访问 然后就得到答案了

```js 
#include<cstdio>
using namespace std;
const int N = 110;
char mat[N][N];

int dfs(int r, int c)
{
    if(mat[r][c] != '@') return 0;
    else
    {
        mat[r][c] = '*';
        for(int i = -1; i <= 1; ++i)
            for(int j = -1; j <= 1; ++j)
                if(mat[r + i][c + j] == '@')
                    dfs(r + i, c + j);
    }
    return 1;
}

int main()
{
    int n, m;
    while(scanf("%d%d", &n, &m) , m)
    {
        for(int i = 1; i <= n; ++i)
            scanf("%s", mat[i] + 1);

        int ans = 0;
        for(int i = 1; i <= n; ++i)
            for(int j = 1; j <= m; ++j)
                ans += dfs(i, j);
        printf("%d\n", ans);
    }
    return 0;
}
```

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