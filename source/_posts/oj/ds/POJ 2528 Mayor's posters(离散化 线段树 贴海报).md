---
title: POJ 2528 Mayor's posters(离散化 线段树 贴海报)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-04-18 19:06:56
---
题意 在墙上贴n张海报 输入每张海报的的左右端点坐标 问最后可以看到多少张海报 能看到一点也是能看到

先把线段树初始化为0 输入一张海报 就把那个区间变成这张海报的序号 最后判断墙上有多少个不同的序号就行了

但是海报坐标的端点值高达10000000 直接用线段树会超时 但是注意到海报最多只有10000张 也就是最多有20000个不同的坐标 于是可以利用离散化的知识 把所有坐标排序 注意所有右端点坐标+1也要加入排序(注意1,10 ; 1,3; 7,10这种情况 如果右端点+1没加入排序的话可能使原来不相邻的变为相邻 这样就覆盖了本来没有覆盖的区间) 可以发现 把每个端点的坐标变为该点排序后的序号不会改变整个图形的结构 于是就可以用序号代替坐标了 大大减小了复杂度

```js 
#include <cstdio>
#include <cstring>
#include <algorithm>
#define lc p<<1,s,mid
#define rc p<<1|1,mid+1,e
#define mid ((s+e)>>1)
#define CLR(A) memset(A,0,sizeof(A))
using namespace std;

const int N = 20005;
int col[N * 4], vis[N];
int le[N], ri[N], c[N * 2], ans;

void pushdown(int p)
{
    if(col[p] == -1) return;
    col[p << 1] = col[p << 1 | 1]  = col[p];
    col[p] = -1;
}

void update(int p, int s, int e, int l, int r, int v)
{
    if(s >= l && e <= r)
    {
        col[p] = v;
        return;
    }
    pushdown(p);
    if(l <= mid) update(lc, l, r, v);
    if(r > mid) update(rc, l, r, v);
}

void query(int p, int s, int e)
{
    if(col[p] != -1)
    {
        if(!vis[col[p]])
            vis[col[p]] = 1, ++ans;
        return;
    }
    query(lc);
    query(rc);
}

int compress(int n) //离散化
{
    int k = 0;
    for(int i = 0; i < n; ++i)
    {
        c[k++] = le[i];
        c[k++] = ri[i];
        c[k++] = ri[i] + 1;
    }
    sort(c, c + k);
    return unique(c, c + k) - c - 1;
}

int main()
{
    int cas, m, n, l, r;
    scanf("%d", &cas);
    while(cas--)
    {
        scanf("%d", &m);
        for(int i = 0; i < m; ++i)
            scanf("%d%d", &le[i], &ri[i]);
        n = compress(m);

        ans = 0;
        CLR(col), CLR(vis);
        for(int i = 0; i < m; ++i)
        {
            l = lower_bound(c, c + n, le[i]) - c + 1;
            r = lower_bound(c, c + n, ri[i]) - c + 1;
            update(1, 1, n, l, r, i + 1);
        }
        vis[0] = 1;
        query(1, 1, n);
        printf("%d\n", ans);
    }
    return 0;
}
```

Mayor's posters

Description
The citizens of Bytetown, AB, could not stand that the candidates in the mayoral election campaign have been placing their electoral posters at all places at their whim. The city council has finally decided to build an electoral wall for placing the posters and introduce the following rules:

They have built a wall 10000000 bytes long (such that there is enough place for all candidates). When the electoral campaign was restarted, the candidates were placing their posters on the wall and their posters differed widely in width. Moreover, the candidates started placing their posters on wall segments already occupied by other posters. Everyone in Bytetown was curious whose posters will be visible (entirely or in part) on the last day before elections.
Your task is to find the number of visible posters when all the posters are placed given the information about posters' size, their place and order of placement on the electoral wall.

Input

The first line of input contains a number c giving the number of cases that follow. The first line of data for a single case contains number 1 <= n <= 10000. The subsequent n lines describe the posters in the order in which they were placed. The i-th line among the n lines contains two integer numbers l i and ri which are the number of the wall segment occupied by the left end and the right end of the i-th poster, respectively. We know that for each 1 <= i <= n, 1 <= l i <= ri <= 10000000. After the i-th poster is placed, it entirely covers all wall segments numbered l i, l i+1 ,... , ri.

Output

For each input data set print the number of visible posters after all the posters are placed.
The picture below illustrates the case of the sample input.
![](../images/es-2528_1.jpg.png)

Sample Input

1 5 1 4 2 6 8 10 3 4 7 10

Sample Output

4

Source

[Alberta Collegiate Programming Contest 2003.10.18](http://poj.org/searchproblem?field=source&key=Alberta+Collegiate+Programming+Contest+2003.10.18)