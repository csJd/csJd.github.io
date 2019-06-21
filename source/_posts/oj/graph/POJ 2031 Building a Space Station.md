---
title: POJ 2031 Building a Space Station
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2014-10-23 10:49:22
---
题意 有n个空间站 接下n行依次输入n个空间站的x,y,z坐标和半径 求连接所有空间站总共至少要修多长的桥

也是裸的最小生成树 注意距离不会小于0 就是两个空间站相交的时候

```js 
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
using namespace std;
const int N = 105, M = 10050;
int par[N], n, m;
double ans;
struct edge{int u, v; double w;} e[M];
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
    double x[N], y[N], z[N], r[N], dis;
    while(scanf("%d", &n), n)
    {
        m = 0;
        for(int i = 1; i <= n; ++i)
        {
            scanf("%lf%lf%lf%lf", &x[i], &y[i], &z[i], &r[i]);
            for(int j = 1; j < i; ++j)
            {
                double tx = x[i] - x[j], ty = y[i] - y[j], tz = z[i] - z[j];
                double dis = sqrt(tx * tx + ty * ty + tz * tz) - r[i] - r[j];
                e[++m].u = i, e[m].v = j, e[m].w = max(0.0, dis);
            }
        }
        sort(e + 1, e + m + 1, cmp);
        ans = 0;
        kruskal();
        printf("%.3f\n", ans);
    }
    return 0;
}
```

Building a Space Station

Description
You are a member of the space station engineering team, and are assigned a task in the construction process of the station. You are expected to write a computer program to complete the task.
The space station is made up with a number of units, called cells. All cells are sphere-shaped, but their sizes are not necessarily uniform. Each cell is fixed at its predetermined position shortly after the station is successfully put into its orbit. It is quite strange that two cells may be touching each other, or even may be overlapping. In an extreme case, a cell may be totally enclosing another one. I do not know how such arrangements are possible.
All the cells must be connected, since crew members should be able to walk from any cell to any other cell. They can walk from a cell A to another cell B, if, (1) A and B are touching each other or overlapping, (2) A and B are connected by a `corridor', or (3) there is a cell C such that walking from A to C, and also from B to C are both possible. Note that the condition (3) should be interpreted transitively.
You are expected to design a configuration, namely, which pairs of cells are to be connected with corridors. There is some freedom in the corridor configuration. For example, if there are three cells A, B and C, not touching nor overlapping each other, at least three plans are possible in order to connect all three cells. The first is to build corridors A-B and A-C, the second B-C and B-A, the third C-A and C-B. The cost of building a corridor is proportional to its length. Therefore, you should choose a plan with the shortest total length of the corridors.
You can ignore the width of a corridor. A corridor is built between points on two cells' surfaces. It can be made arbitrarily long, but of course the shortest one is chosen. Even if two corridors A-B and C-D intersect in space, they are not considered to form a connection path between (for example) A and C. In other words, you may consider that two corridors never intersect.

Input

The input consists of multiple data sets. Each data set is given in the following format.
n
x1 y1 z1 r1
x2 y2 z2 r2
...
xn yn zn rn
The first line of a data set contains an integer n, which is the number of cells. n is positive, and does not exceed 100.
The following n lines are descriptions of cells. Four values in a line are x-, y- and z-coordinates of the center, and radius (called r in the rest of the problem) of the sphere, in this order. Each value is given by a decimal fraction, with 3 digits after the decimal point. Values are separated by a space character.
Each of x, y, z and r is positive and is less than 100.0.
The end of the input is indicated by a line containing a zero.

Output

For each data set, the shortest total length of the corridors should be printed, each in a separate line. The printed values should have 3 digits after the decimal point. They may not have an error greater than 0.001.
Note that if no corridors are necessary, that is, if all the cells are connected without corridors, the shortest total length of the corridors is 0.000.

Sample Input

3 10.000 10.000 50.000 10.000 40.000 10.000 50.000 10.000 40.000 40.000 50.000 10.000 2 30.000 30.000 30.000 20.000 40.000 40.000 40.000 20.000 5 5.729 15.143 3.996 25.837 6.013 14.372 4.818 10.671 80.115 63.292 84.477 15.120 64.095 80.924 70.029 14.881 39.472 85.116 71.369 5.553 0

Sample Output

20.000 0.000 73.834