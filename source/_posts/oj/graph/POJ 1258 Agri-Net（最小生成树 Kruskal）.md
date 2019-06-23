---
title: POJ 1258 Agri-Net（最小生成树 Kruskal）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-10-22 10:37:18
---
题意 给你农场的邻接矩阵 求连通所有农场的最小消耗

和上一题一样裸的最小生成树

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;

const int N = 105, M = 10050;
int par[N], ans, n, m, t;
struct edge { int u, v, w;} e[M];
bool cmp(edge a, edge b){ return a.w < b.w;}

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
    if(par[ru] < par[rv])
        par[rv] = ru, par[ru] = tmp;
    else
        par[ru] = rv, par[rv] = tmp;
}

void kruskal()
{
    int cur = 0, u, v;
    memset(par, -1, sizeof(par));
    for(int i = 1; i <= m; ++i)
    {
        u = e[i].u, v = e[i].v;
        if(Find(u) != Find(v))
        {
            ans += e[i].w;
            Union(u, v);
            ++cur;
        }
        if(cur >= n - 1) break;
    }
}

int main()
{
    while(~scanf("%d", &n))
    {
        ans = m = 0;
        for(int i = 1; i <= n; ++i)
        {
            for(int j = 1; j <= n; ++j)
            {
                scanf("%d", &t);
                if(j < i) e[++m].u = i, e[m].v = j, e[m].w = t;
            }
        }
        sort(e + 1, e + m + 1, cmp);
        kruskal();
        printf("%d\n", ans);
    }
    return 0;
}
```

Agri-Net

Description
Farmer John has been elected mayor of his town! One of his campaign promises was to bring internet connectivity to all farms in the area. He needs your help, of course.
Farmer John ordered a high speed connection for his farm and is going to share his connectivity with the other farmers. To minimize cost, he wants to lay the minimum amount of optical fiber to connect his farm to all the other farms.
Given a list of how much fiber it takes to connect each pair of farms, you must find the minimum amount of fiber needed to connect them all together. Each farm must connect to some other farm such that a packet can flow from any one farm to any other farm.
The distance between any two farms will not exceed 100,000.

Input

The input includes several cases. For each case, the first line contains the number of farms, N (3 <= N <= 100). The following lines contain the N x N conectivity matrix, where each element shows the distance from on farm to another. Logically, they are N lines of N space-separated integers. Physically, they are limited in length to 80 characters, so some lines continue onto others. Of course, the diagonal will be 0, since the distance from farm i to itself is not interesting for this problem.

Output

For each case, output a single integer length that is the sum of the minimum length of fiber required to connect the entire set of farms.

Sample Input

4 0 4 9 21 4 0 8 17 9 8 0 16 21 17 16 0

Sample Output

28