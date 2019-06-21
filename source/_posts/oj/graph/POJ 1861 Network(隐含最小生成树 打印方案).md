---
title: POJ 1861 Network(隐含最小生成树 打印方案)
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2014-10-22 11:16:03
---
题意 求n个点m条边的图的连通子图中最长边的最小值

实际上就是求最小生成树中的最长边 因为最小生成树的最长边肯定是所有生成树中最长边最小的 那么就也变成了最小生成树了 不要被样例坑到了 样例并不是最佳方案 只是最长边与最小生成树的最长边相等 题目是特判 直接用最小生成树做就行

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 1005, M = 15005;
struct edge{int u, v, w;} e[M];
int par[N], ea[N], n, m, num, ans;
bool cmp(edge a, edge b){ return a.w < b.w; }

int Find(int x)
{
    int r = x, tmp;
    while(par[r] >= 0) r = par[r];
    while(x != r)
    {
        tmp = par[x];
        par[x] = r;
        x = tmp;
    }
    return r;
}

void Union(int u, int v)
{
    int ru = Find(u), rv = Find(v), tmp = par[ru] + par[rv];
    if(par[ru] > par[rv])
        par[ru] = rv, par[rv] = tmp;
    else
        par[rv] = ru, par[ru] = tmp;
}

void kruskal()
{
    memset(par, -1, sizeof(par));
    for(int i = 1; i <= m; ++i)
    {
        int u = e[i].u, v = e[i].v;
        if(Find(u) != Find(v))
        {
            ea[++num] = i;
            ans = max(ans, e[i].w);
            Union(u, v);
        }
        if(num >= n - 1) break;
    }
}

int main()
{
    int u, v, w;
    while(~scanf("%d%d", &n, &m))
    {
        for(int i = 1; i <= m; ++i)
        {
            scanf("%d%d%d", &u, &v, &w);
            e[i].u = u, e[i].v = v, e[i].w = w;
        }

        sort(e + 1, e + m + 1, cmp);
        ans = num = 0;
        kruskal();
        printf("%d\n%d\n", ans, num);
        for(int i = 1; i <= num; ++i)
        {
            int j = ea[i];
            printf("%d %d\n", e[j].u, e[j].v);
        }
    }
    return 0;
}
```

Network

Description
Andrew is working as system administrator and is planning to establish a new network in his company. There will be N hubs in the company, they can be connected to each other using cables. Since each worker of the company must have access to the whole network, each hub must be accessible by cables from any other hub (with possibly some intermediate hubs).
Since cables of different types are available and shorter ones are cheaper, it is necessary to make such a plan of hub connection, that the maximum length of a single cable is minimal. There is another problem — not each hub can be connected to any other one because of compatibility problems and building geometry limitations. Of course, Andrew will provide you all necessary information about possible hub connections.
You are to help Andrew to find the way to connect hubs so that all above conditions are satisfied.

Input

The first line of the input contains two integer numbers: N - the number of hubs in the network (2 <= N <= 1000) and M - the number of possible hub connections (1 <= M <= 15000). All hubs are numbered from 1 to N. The following M lines contain information about possible connections - the numbers of two hubs, which can be connected and the cable length required to connect them. Length is a positive integer number that does not exceed 106. There will be no more than one way to connect two hubs. A hub cannot be connected to itself. There will always be at least one way to connect all hubs.

Output

Output first the maximum length of a single cable in your hub connection plan (the value you should minimize). Then output your plan: first output P - the number of cables used, then output P pairs of integer numbers - numbers of hubs connected by the corresponding cable. Separate numbers by spaces and/or line breaks.

Sample Input

4 6 1 2 1 1 3 1 1 4 2 2 3 1 3 4 1 2 4 1

Sample Output

1 4 1 2 1 3 2 3 3 4