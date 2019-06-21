---
title: POJ 1436 Horizontally Visible Segments (线段树·区间染色)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-07-15 15:58:17
---
题意 在坐标系中有n条平行于y轴的线段 当一条线段与另一条线段之间可以连一条平行与x轴的线不与其它线段相交 就视为它们是可见的 问有多少组三条线段两两相互可见

先把所有线段存下来 并按x坐标排序 线段树记录对应区间从右往左当前可见的线段编号(1...n) 超过一条就为0 然后从左往右对每条线段 先查询左边哪些线段和它是可见的 把可见关系存到数组中 然后把这条线段对应区间的最右端可见编号更新为这条线段的编号 最后暴力统计有多少组就行了

```js 
#include <cstdio>
#include <algorithm>
#include <cstring>
#define lc p<<1, s, mid
#define rc p<<1|1, mid+1, e
#define mid ((s+e)>>1)
using namespace std;
const int N = 8005;
int top[N * 8];
bool g[N][N];

struct seg
{
    int y1, y2, x;
} line[N];

bool cmp(seg a, seg b)
{
    return a.x < b.x;
}

void build()
{
    memset(g, 0, sizeof(g));
    memset(top, 0, sizeof(top));
}

void pushup(int p)
{
    top[p] = (top[p << 1] == top[p << 1 | 1]) ? top[p << 1] : 0;
}

void pushdown(int p)
{
    if(top[p])
    {
        top[p << 1] = top[p << 1 | 1] = top[p];
        top[p] = 0;
    }
}

void update(int p, int s, int e, int l, int r, int v)
{
    if(l <= s && e <= r)
    {
        top[p] = v;
        return;
    }
    pushdown(p);
    if(l <= mid) update(lc, l, r, v);
    if(r > mid) update(rc, l, r, v);
    pushup(p);
}

void query(int p, int s, int e, int l, int r, int x)
{
    if(top[p]) //p对应的区间已经只可见一条线段
    {
        g[top[p]][x] = 1;
        return;
    }
    if(s == e) return;
    if(l <= mid) query(lc, l, r, x);
    if(r > mid) query(rc, l, r, x);
}

int main()
{
    int T, n, l, r;
    scanf("%d", &T);
    while(T--)
    {
        scanf("%d", &n);
        for(int i = 1; i <= n; ++i)
            scanf("%d%d%d", &line[i].y1, &line[i].y2, &line[i].x);
        sort(line + 1, line + n + 1, cmp);

        build();
        for(int i = 1; i <= n; ++i)
        {
            //点化为区间会丢失间隔为1的区间  所以要乘以2
            l = (line[i].y1) * 2;
            r = (line[i].y2) * 2;
            query(1, 0, N * 2, l, r, i);
            update(1, 0, N * 2, l, r, i);
        }

        int ans = 0;
        for(int i = 1; i <= n; ++i)
        {
            for(int j = i + 1; j <= n; ++j)
            {
                if(g[i][j])
                    for(int k = j + 1; k <= n; ++k)
                        if(g[j][k] && g[i][k]) ++ans;
            }
        }

        printf("%d\n", ans);
    }
    return 0;
}
//Last modified :   2015-07-15 15:33
```

Horizontally Visible Segments

Description
There is a number of disjoint vertical line segments in the plane. We say that two segments are horizontally visible if they can be connected by a horizontal line segment that does not have any common points with other vertical segments. Three different vertical segments are said to form a triangle of segments if each two of them are horizontally visible. How many triangles can be found in a given set of vertical segments?
Task
Write a program which for each data set:
reads the description of a set of vertical segments,
computes the number of triangles in this set,
writes the result.

Input

The first line of the input contains exactly one positive integer d equal to the number of data sets, 1 <= d <= 20. The data sets follow.
The first line of each data set contains exactly one integer n, 1 <= n <= 8 000, equal to the number of vertical line segments.
Each of the following n lines consists of exactly 3 nonnegative integers separated by single spaces:
yi', yi'', xi - y-coordinate of the beginning of a segment, y-coordinate of its end and its x-coordinate, respectively. The coordinates satisfy 0 <= yi' < yi'' <= 8 000, 0 <= xi <= 8 000. The segments are disjoint.

Output

The output should consist of exactly d lines, one line for each data set. Line i should contain exactly one integer equal to the number of triangles in the i-th data set.

Sample Input

1 5 0 4 4 0 3 1 3 4 2 0 2 2 0 2 3

Sample Output

1

Source

[Central Europe 2001](http://poj.org/searchproblem?field=source&key=Central+Europe+2001)