---
title: CF 505B Mr. Kitayuta's Colorful Graph(最短路)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2015-01-19 09:06:39
---
题意 求两点之间有多少不同颜色的路径

范围比较小 可以直接floyd

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 105;
int d[N][N][N], ans;

int main()
{
    int a, b, c, n, m, q;
    while(~scanf("%d%d", &n, &m))
    {
        memset(d, 0, sizeof(d));
        for(int i=1;i<=m;++i)
        {
            scanf("%d%d%d", &a, &b, &c);
            d[c][a][b] = d[c][b][a] = 1;
        }

        for(c = 1; c <= m; ++c)
        for(int k = 1; k <= n; ++k)
        for(int i = 1; i <= n; ++i)
        for(int j = 1; j <= n; ++j)
            if(!d[c][i][j]) d[c][i][j] = d[c][j][i] = (d[c][i][k] && d[c][k][j]);

        scanf("%d", &q);
        while(q--)
        {
            ans = 0;
            scanf("%d%d", &a, &b);
            for(int c = 1; c <= m; ++c)
                if(d[c][a][b]) ++ans;
            printf("%d\n", ans);
        }
    }
    return 0;
}
```

B. Mr. Kitayuta's Colorful Graph
Mr. Kitayuta has just bought an undirected graph consisting of *n* vertices and *m* edges. The vertices of the graph are numbered from 1 to *n*. Each edge, namely edge *i*, has a color *c**i*, connecting vertex *a**i* and *b**i*.

Mr. Kitayuta wants you to process the following *q* queries.

In the *i*-th query, he gives you two integers — *u**i* and *v**i*.

Find the number of the colors that satisfy the following condition: the edges of that color connect vertex *u**i* and vertex *v**i* directly or indirectly.

Input

The first line of the input contains space-separated two integers — *n* and *m* (2 ≤ *n* ≤ 100, 1 ≤ *m* ≤ 100), denoting the number of the vertices and the number of the edges, respectively.

The next *m* lines contain space-separated three integers — *a**i*, *b**i* (1 ≤ *a**i* < *b**i* ≤ *n*) and *c**i* (1 ≤ *c**i* ≤ *m*). Note that there can be multiple edges between two vertices. However, there are no multiple edges of the same color between two vertices, that is, if *i* ≠ *j*, (*a**i*, *b**i*, *c**i*) ≠ (*a**j*, *b**j*, *c**j*).

The next line contains a integer — *q* (1 ≤ *q* ≤ 100), denoting the number of the queries.

Then follows *q* lines, containing space-separated two integers — *u**i* and *v**i* (1 ≤ *u**i*, *v**i* ≤ *n*). It is guaranteed that *u**i* ≠ *v**i*.
Output

For each query, print the answer in a separate line.

Sample test(s)

input 4 5 1 2 1 1 2 2 2 3 1 2 3 3 2 4 3 3 1 2 3 4 1 4

output 2 1 0
input 5 7 1 5 1 2 5 1 3 5 1 4 5 1 1 2 2 2 3 2 3 4 2 5 1 5 5 1 2 5 1 5 1 4

output 1 1 1 1 2