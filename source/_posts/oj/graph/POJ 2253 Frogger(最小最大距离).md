---
title: POJ 2253 Frogger(最小最大距离)
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2014-10-30 23:02:03
---
题意 给你n个点的坐标 求第1个点到第2个点的所有路径中两点间最大距离的最小值

很水的floyd咯

```js 
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>
using namespace std;
const int N=205;
double d[N][N];
int x[N],y[N],n;

void floyd()
{
    for(int k=1;k<=n;++k)
    for(int i=1;i<=n;++i)
    for(int j=1;j<=n;++j)
        d[i][j]=min(d[i][j],max(d[i][k],d[k][j]));
}

int main()
{
    int cas=0;
    while(scanf("%d",&n),n)
    {
        memset(d,0x3f,sizeof(d));
        for(int i=1;i<=n;++i)
        {
            scanf("%d%d",&x[i],&y[i]);
            for(int j=1;j<i;++j)
            {
                int tx=x[i]-x[j],ty=y[i]-y[j];
                d[i][j]=d[j][i]=sqrt(tx*tx+ty*ty);
            }
        }
        floyd();
        printf("Scenario #%d\nFrog Distance = %.3f\n\n",++cas,d[1][2]);
    }
    return 0;
}
```

Frogger

Description
Freddy Frog is sitting on a stone in the middle of a lake. Suddenly he notices Fiona Frog who is sitting on another stone. He plans to visit her, but since the water is dirty and full of tourists' sunscreen, he wants to avoid swimming and instead reach her by jumping.
Unfortunately Fiona's stone is out of his jump range. Therefore Freddy considers to use other stones as intermediate stops and reach her by a sequence of several small jumps.
To execute a given sequence of jumps, a frog's jump range obviously must be at least as long as the longest jump occuring in the sequence.
The frog distance (humans also call it minimax distance) between two stones therefore is defined as the minimum necessary jump range over all possible paths between the two stones.
You are given the coordinates of Freddy's stone, Fiona's stone and all other stones in the lake. Your job is to compute the frog distance between Freddy's and Fiona's stone.

Input

The input will contain one or more test cases. The first line of each test case will contain the number of stones n (2<=n<=200). The next n lines each contain two integers xi,yi (0 <= xi,yi <= 1000) representing the coordinates of stone /#i. Stone /#1 is Freddy's stone, stone /#2 is Fiona's stone, the other n-2 stones are unoccupied. There's a blank line following each test case. Input is terminated by a value of zero (0) for n.

Output

For each test case, print a line saying "Scenario /#x" and a line saying "Frog Distance = y" where x is replaced by the test case number (they are numbered from 1) and y is replaced by the appropriate real number, printed to three decimals. Put a blank line after each test case, even after the last one.

Sample Input

2 0 0 3 4 3 17 4 19 4 18 5 0

Sample Output

Scenario /#1 Frog Distance = 5.000 Scenario /#2 Frog Distance = 1.414

Source

[Ulm Local 1997](http://poj.org/searchproblem?field=source&key=Ulm+Local+1997)