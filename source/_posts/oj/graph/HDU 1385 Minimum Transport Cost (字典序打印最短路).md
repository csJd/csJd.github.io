---
title: HDU 1385 Minimum Transport Cost (字典序打印最短路)
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2014-10-30 11:34:11
---
题意 给你一个无向图的邻接矩阵 和途径每个点需要的额外花费首尾没有额外花费 求图中某两点之间的最短路并打印字典序最小路径

要求多组点之间的就用floyd咯 打印路径也比较方便 nex[i][j]表示从i点到j点最短路的第一个途经点 那么如果路径中加入一个节点k后 nex[i][j]应该更新为nex[i][k] 因为要途径k了

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 100,INF=0x3f3f3f;
int cost[N][N], tax[N], nex[N][N];
int s, t, n;

void floyd()
{
    int tmp;
    for(int k = 1; k <= n; ++k)
    for(int i = 1; i <= n; ++i)
    for(int j = 1; j <= n; ++j)
    {
        tmp = cost[i][k] + cost[k][j] + tax[k];
        if(cost[i][j] > tmp || (cost[i][j] == tmp && nex[i][j] > nex[i][k]))
        {
            nex[i][j] = nex[i][k];
            cost[i][j] = tmp;
        }
    }
}

int main()
{
    while(scanf("%d", &n), n)
    {
        memset(nex,0,sizeof(nex));
        for(int i = 1; i <= n; ++i)
        {
            for(int j = 1; j <= n; ++j)
            {
                scanf("%d", &cost[i][j]);
                if(cost[i][j] < 0) cost[i][j]=INF;
                else nex[i][j]=j;
            }
        }

        for(int i = 1; i <= n; ++i)  scanf("%d", &tax[i]);
        floyd();
        while(scanf("%d%d", &s, &t), s > 0)
        {
            int k=s;
            printf("From %d to %d :\nPath: %d", s, t, s);
            while(k!=t) printf("-->%d", k=nex[k][t]);
            printf("\nTotal cost : %d\n\n", cost[s][t]);
        }
    }
    return 0;
}
```

# Minimum Transport Cost

Problem Description

These are N cities in Spring country. Between each pair of cities there may be one transportation track or none. Now there is some cargo that should be delivered from one city to another. The transportation fee consists of two parts:
The cost of the transportation on the path between these cities, and
a certain tax which will be charged whenever any cargo passing through one city, except for the source and the destination cities.
You must write a program to find the route which has the minimum cost.
Input

First is N, number of cities. N = 0 indicates the end of input.
The data of path cost, city tax, source and destination cities are given in the input, which is of the form:
a11 a12 ... a1N
a21 a22 ... a2N
...............
aN1 aN2 ... aNN
b1 b2 ... bN
c d
e f
...
g h
where aij is the transport cost from city i to city j, aij = -1 indicates there is no direct path between city i and city j. bi represents the tax of passing through city i. And the cargo is to be delivered from city c to city d, city e to city f, ..., and g = h = -1. You must output the sequence of cities passed by and the total cost which is of the form:
Output

From c to d :
Path: c-->c1-->......-->ck-->d
Total cost : ......
......
From e to f :
Path: e-->e1-->..........-->ek-->f
Total cost : ......
Note: if there are more minimal paths, output the lexically smallest one. Print a blank line after each test case.
Sample Input

5 0 3 22 -1 4 3 0 5 -1 -1 22 5 0 9 20 -1 -1 9 0 4 4 -1 20 4 0 5 17 8 3 1 1 3 3 5 2 4 -1 -1 0
Sample Output

From 1 to 3 : Path: 1-->5-->4-->3 Total cost : 21 From 3 to 5 : Path: 3-->4-->5 Total cost : 16 From 2 to 4 : Path: 2-->1-->5-->4 Total cost : 17
Source

[Asia 1996, Shanghai (Mainland China)](http://acm.hdu.edu.cn/search.php?field=problem&key=Asia+1996%2C+Shanghai+%28Mainland+China%29&source=1&searchmode=source)