---
title: UVa 140 Bandwidth(DFS 回溯 剪枝)
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2014-11-26 18:48:16
---
题意 有一个无向图 对于其所有顶点的一个排列 称一顶点与所有与其有边的其它顶点在排列中下标差的最大值为这个顶点的带宽 而所有顶点带宽的最大值为这个排列的带宽 求这个图带宽最小的排列

枚举排列 ans记录当前找到的最小带宽 枚举过程中一旦出现带宽大于ans的也就不用再扩展了 这样枚举完就得到了答案

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 50;
int n, ans, h[N], vis[N], a[N], d[N][N], p[N];

void dfs(int cur, int band)
{
    if(cur >= n && band < ans)
    {
        ans = band;
        for(int i = 0; i < cur; ++i)
            p[i] = a[i];//p数组记录最优解路径
    }

    for(int i = 0; cur < n && i < 26; ++i)
    {
        if(vis[i] || !h[i]) continue;

        a[cur] = i , vis[i] = 1;
        for(int j = 0; j < cur; ++j)
            if(d[i][a[j]] && band < cur - j) band = cur - j;

        if(band > ans)
        {
            vis[i] = 0;
            return;
        }
        dfs(cur + 1, band);
        vis[i] = 0;
    }
}

int main()
{
    char s[N];
    while(scanf("%s", s), strcmp(s, "#"))
    {
        memset(d, 0, sizeof(d));
        memset(h, 0, sizeof(h));
        //h数组记录字母'A'+i是否在图中出现  n为顶点个数
        int i = n = 0, u, v;
        while(s[i])
        {
            u = s[i] - 'A';
            if(!h[u]) h[u] = ++n;
            i += 2;
            while(s[i] && s[i] != ';')
            {
                v = s[i++] - 'A';
                if(!h[v]) h[v] = ++n;
                d[u][v] = d[v][u] =  1;
            }
            if(!s[i++]) break;
        }

        ans = N;
        memset(vis, 0, sizeof(vis));
        dfs(0, 0);
        for(int i = 0; i < n; ++i)
            printf("%c ", 'A' + p[i]);
        printf("-> %d\n", ans);
    }
    return 0;
}
```

#

****

Given a graph (V,E) where V is a set of nodes and E is a set of arcs in VxV, and an *ordering* on the elements in V, then the *bandwidth* of a node *v* is defined as the maximum distance in the ordering between *v* and any node to which it is connected in the graph. The bandwidth of the ordering is then defined as the maximum of the individual bandwidths. For example, consider the following graph:

![picture25](../images/dge.org-external-1-140img1.gif.png)

This can be ordered in many ways, two of which are illustrated below:

![picture47](../images/dge.org-external-1-140img2.gif.png)

For these orderings, the bandwidths of the nodes (in order) are 6, 6, 1, 4, 1, 1, 6, 6 giving an ordering bandwidth of 6, and 5, 3, 1, 4, 3, 5, 1, 4 giving an ordering bandwidth of 5.

Write a program that will find the ordering of a graph that minimises the bandwidth.

## Input

Input will consist of a series of graphs. Each graph will appear on a line by itself. The entire file will be terminated by a line consisting of a single /#. For each graph, the input will consist of a series of records separated by `;'. Each record will consist of a node name (a single upper case character in the the range `A' to `Z'), followed by a `:' and at least one of its neighbours. The graph will contain no more than 8 nodes.

## Output

Output will consist of one line for each graph, listing the ordering of the nodes followed by an arrow (->) and the bandwidth for that ordering. All items must be separated from their neighbours by exactly one space. If more than one ordering produces the same bandwidth, then choose the smallest in lexicographic ordering, that is the one that would appear first in an alphabetic listing.

## Sample input

A:FB;B:GC;D:GC;F:AGH;E:HD /#

## Sample output

A B C F G D H E -> 3