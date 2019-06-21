---
title: POJ 2421 Constructing Roads（最小生成树）
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2014-10-23 11:16:40
---
题意 在n个村庄之间修路使所有村庄连通 其中有些路已经修好了 求至少还需要修多长路

还是裸的最小生成树 修好的边权值为0就行咯

```js 
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
using namespace std;
const int N = 105, M = 10050;
int par[N], n, m, mat[N][N];
int ans;
struct edge{int u, v, w;} e[M];
bool cmp(edge a, edge b){return a.w < b.w;}

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

void Union (int u, int v)
{
    int ru = Find(u), rv = Find(v), tmp = par[ru] + par[rv];
    if(par[ru] < par[rv])
        par[rv] = ru, par[ru] = tmp;
    else
        par[ru] = rv, par[rv] = tmp;
}

void kruskal()
{
    memset(par, -1, sizeof(par));
    int cnt = 0;
    for(int i = 1; i <= m; ++i)
    {
        int u = e[i].u, v = e[i].v;
        if(Find(u) != Find(v))
        {
            ++cnt;
            ans += e[i].w;
            Union(u, v);
        }
        if(cnt >= n - 1) break;
    }
}

int main()
{
    int t, u, v;
    while(~scanf("%d", &n))
    {
        m = 0;
        for(int i = 1; i <= n; ++i)
        {
            for(int j = 1; j <= n; ++j)
            {
                scanf("%d", &mat[i][j]);
                if(j < i) e[++m].u = i, e[m].v = j, e[m].w = mat[i][j];
            }
        }

        scanf("%d", &t);
        while(t--)
        {
            scanf("%d%d", &u, &v);
            e[++m].u = u, e[m].v = v, e[m].w = 0;
        }

        sort(e + 1, e + m + 1, cmp);
        ans = 0; kruskal();
        printf("%d\n", ans);
    }
    return 0;
}
```
Constructing Roads

**Time Limit:** 2000MS  **Memory Limit:** 65536K **Total Submissions:** 19755  **Accepted:** 8250

Description
There are N villages, which are numbered from 1 to N, and you should build some roads such that every two villages can connect to each other. We say two village A and B are connected, if and only if there is a road between A and B, or there exists a village C such that there is a road between A and C, and C and B are connected.
We know that there are already some roads between some villages and your job is the build some roads such that all the villages are connect and the length of all the roads built is minimum.

Input

The first line is an integer N (3 <= N <= 100), which is the number of villages. Then come N lines, the i-th of which contains N integers, and the j-th of these N integers is the distance (the distance should be an integer within [1, 1000]) between village i and village j.
Then there is an integer Q (0 <= Q <= N /* (N + 1) / 2). Then come Q lines, each line contains two integers a and b (1 <= a < b <= N), which means the road between village a and village b has been built.

Output

You should output a line contains an integer, which is the length of all the roads to be built such that all the villages are connected, and this value is minimum.

Sample Input

3 0 990 692 990 0 179 692 179 0 1 1 2

Sample Output

179