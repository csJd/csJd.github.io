---
title: ZOJ 1203 Swordfish(最小生成树 kruskal)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-10-22 09:39:13
---
题意 给你n个点的坐标 每个点都可与其它n-1个点相连 求这n个点的最小生成树的权重

裸的最小生成树 直接kruskal咯

```js 
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
using namespace std;
const int N = 105, M = 10050;
double x[N], y[N], ans;
int n, m , par[N];

struct edge {
    int u, v;
    double w;
} e[M];

bool cmp(edge a, edge b)  { return a.w < b.w;}

int Find(int x)
{
    int r = x;
    while(par[r] >= 0) r = par[r];
    while(x != r) //压缩
    {
        int tmp = par[x];
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
    int cur = 0, u, v;
    memset(par, -1, sizeof(par)); //初始化并查集
    for(int i = 1; i <= m; ++i)
    {
        u = e[i].u, v = e[i].v;
        if(Find(u) != Find(v))
        {
            ans += e[i].w;
            ++cur;
            Union(u, v);
        }
        if(cur >= n - 1) break;
    }
}

int main()
{
    int cas = 0;
    while(scanf("%d", &n), n)
    {
        if(cas) printf("\n");
        m = 0;

        for(int i = 1; i <= n; ++i)
        {
            scanf("%lf%lf", &x[i], &y[i]);
            for(int j = 1; j < i; ++j)
            {
                double dis = sqrt((x[i] - x[j]) * (x[i] - x[j]) + (y[i] - y[j]) * (y[i] - y[j]));
                e[++m].u = i, e[m].v = j, e[m].w = dis;
            }
        }

        sort(e + 1, e + 1 + m, cmp);
        ans = 0; kruskal();
        printf("Case #%d:\nThe minimal distance is: %.2f\n", ++cas, ans);
    }

    return 0;
}
```

Swordfish    Time Limit:2 Seconds Memory Limit:65536 KB    There exists a world within our world
A world beneath what we call cyberspace.
A world protected by firewalls,
passwords and the most advanced
security systems.
In this world we hide
our deepest secrets,
our most incriminating information,
and of course, a shole lot of money.
This is the world of Swordfish.
  We all remember that in the movie Swordfish, Gabriel broke into the World Bank Investors Group in West Los Angeles, to rob $9.5 billion. And he needed Stanley, the best hacker in the world, to help him break into the password protecting the bank system. Stanley's lovely daughter Holly was seized by Gabriel, so he had to work for him. But at the last moment, Stanley made some little trick in his hacker mission: he injected a trojan horse in the bank system, so the money would jump from one account to another account every 60 seconds, and would continue jumping in the next 10 years. Only Stanley knew when and where to get the money. If Gabriel killed Stanley, he would never get a single dollar. Stanley wanted Gabriel to release all these hostages and he would help him to find the money back.
You who has watched the movie know that Gabriel at last got the money by threatening to hang Ginger to death. Why not Gabriel go get the money himself? Because these money keep jumping, and these accounts are scattered in different cities. In order to gather up these money Gabriel would need to build money transfering tunnels to connect all these cities. Surely it will be really expensive to construct such a transfering tunnel, so Gabriel wants to find out the minimal total length of the tunnel required to connect all these cites. Now he asks you to write a computer program to find out the minimal length. Since Gabriel will get caught at the end of it anyway, so you can go ahead and write the program without feeling guilty about helping a criminal.
Input:
The input contains several test cases. Each test case begins with a line contains only one integer N (0 <= N <=100), which indicates the number of cities you have to connect. The next N lines each contains two real numbers X and Y(-10000 <= X,Y <= 10000), which are the citie's Cartesian coordinates (to make the problem simple, we can assume that we live in a flat world). The input is terminated by a case with N=0 and you must not print any output for this case.
Output:
You need to help Gabriel calculate the minimal length of tunnel needed to connect all these cites. You can saftly assume that such a tunnel can be built directly from one city to another. For each of the input cases, the output shall consist of two lines: the first line contains "Case /#n:", where n is the case number (starting from 1); and the next line contains "The minimal distance is: d", where d is the minimal distance, rounded to 2 decimal places. Output a blank line between two test cases.
Sample Input:
 5 0 0 0 1 1 1 1 0 0.5 0.5 0 Sample Output:
 Case /#1: The minimal distance is: 2.83