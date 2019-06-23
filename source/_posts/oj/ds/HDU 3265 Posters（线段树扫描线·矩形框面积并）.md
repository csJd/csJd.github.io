---
title: HDU 3265 Posters（线段树扫描线·矩形框面积并）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2015-08-15 17:56:37
---
题意 把一些矩形海报挖去一部分小矩形贴在指定位置 问最后海报覆盖的面积

一个矩形框可以分割成4个独立的小矩形 然后就能用扫描线求面积并了

```js 
#include <cstdio>
#include <algorithm>
using namespace std;
const int N = 100005, M = N << 2;
typedef long long ll;

struct SLine
{
    int x, y1, y2, flag;
    SLine() {};
    SLine(int xx, int a, int b, int f) :
        x(xx), y1(a), y2(b), flag(f) {}
    bool operator< (const SLine &s) const{
        return x < s.x;
    }
} line[N << 3];

int len[M], cnt[M];

void pushup(int p, int s, int e)
{
    if(cnt[p]) len[p] = e - s + 1;
    else if(s == e) len[p] = 0;
    else len[p] = len[p << 1] + len[p << 1 | 1];
}

void update(int p, int s, int e, int l, int r, int v)
{
    if(r < l) return; //分割后矩形有y1 = y2 的情况
    if(l <= s && e <= r)
    {
        cnt[p] += v;
        pushup(p, s, e);
        return;
    }
    int mid = (s + e) >> 1;
    if(l <= mid) update(p << 1, s, mid, l, r, v);
    if(r > mid) update(p << 1 | 1, mid + 1, e, l, r, v);
    pushup(p, s, e);
}

int main()
{
    int n, m, x1, y1, x2, y2, x3, y3, x4, y4, f;
    while(scanf("%d", &n), n)
    {
        m = 0;
        for(int i = 0; i < n; ++i)
        {
            scanf("%d%d%d%d", &x1, &y1, &x2, &y2);
            scanf("%d%d%d%d", &x3, &y3, &x4, &y4);
            //将矩形框分成4个矩形
            line[m++] = SLine(x1, y1, y2, 1);
            line[m++] = SLine(x3, y1, y2, -1);
            line[m++] = SLine(x4, y1, y2, 1);
            line[m++] = SLine(x2, y1, y2, -1);

            line[m++] = SLine(x1, y1, y3, 1);
            line[m++] = SLine(x2, y1, y3, -1);
            line[m++] = SLine(x1, y4, y2, 1);
            line[m++] = SLine(x2, y4, y2, -1);
        }
        sort(line, line + m);

        ll ans = 0;
        for(int i = 0; i < m; ++i)
        {
            update(1, 1, N, line[i].y1 + 1, line[i].y2, line[i].flag);
            ans += ll(len[1]) * (line[i + 1].x - line[i].x);
        }
        printf("%lld\n", ans);
    }

    return 0;
}
//Last modified :  2015-08-14 14:29 CST
```

# Posters

Problem Description

Ted has a new house with a huge window. In this big summer, Ted decides to decorate the window with some posters to prevent the glare outside. All things that Ted can find are rectangle posters.
However, Ted is such a picky guy that in every poster he finds something ugly. So before he pastes a poster on the window, he cuts a rectangular hole on that poster to remove the ugly part. Ted is also a careless guy so that some of the pasted posters may overlap when he pastes them on the window.
Ted wants to know the total area of the window covered by posters. Now it is your job to figure it out.
To make your job easier, we assume that the window is a rectangle located in a rectangular coordinate system. The window’s bottom-left corner is at position (0, 0) and top-right corner is at position (50000, 50000). The edges of the window, the edges of the posters and the edges of the holes on the posters are all parallel with the coordinate axes.
Input

The input contains several test cases. For each test case, the first line contains a single integer N (0<N<=50000), representing the total number of posters. Each of the following N lines contains 8 integers x1, y1, x2, y2, x3, y3, x4, y4, showing details about one poster. (x1, y1) is the coordinates of the poster’s bottom-left corner, and (x2, y2) is the coordinates of the poster’s top-right corner. (x3, y3) is the coordinates of the hole’s bottom-left corner, while (x4, y4) is the coordinates of the hole’s top-right corner. It is guaranteed that 0<=xi, yi<=50000(i=1…4) and x1<=x3<x4<=x2, y1<=y3<y4<=y2.
The input ends with a line of single zero.
Output

For each test case, output a single line with the total area of window covered by posters.
Sample Input

2 0 0 10 10 1 1 9 9 2 2 8 8 3 3 7 7 0
Sample Output

56