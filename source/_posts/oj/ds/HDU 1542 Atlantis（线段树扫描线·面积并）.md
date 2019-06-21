---
title: HDU 1542 Atlantis（线段树扫描线·面积并）
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-08-12 21:10:56
---
题意 给你一些矩形的左下和右上的坐标 求这些矩形的面积并

最基础的扫描线 理解了就是个水题了 先看一些图吧

![](../images/dn.net-20150812201313561-watermark-2-text-aHR0cDovL2Jsb2cuY3Nkbi5uZXQv-font-5a6L5L2T-fontsize-400-fill-I0JBQkFCMA==-dissolve-70-gravity-SouthEast.png) ![](../images/dn.net-20150812201335946-watermark-2-text-aHR0cDovL2Jsb2cuY3Nkbi5uZXQv-font-5a6L5L2T-fontsize-400-fill-I0JBQkFCMA==-dissolve-70-gravity-SouthEast.png) ![](../images/dn.net-20150812201350012-watermark-2-text-aHR0cDovL2Jsb2cuY3Nkbi5uZXQv-font-5a6L5L2T-fontsize-400-fill-I0JBQkFCMA==-dissolve-70-gravity-SouthEast.png) ![](../images/dn.net-20150812201358280-watermark-2-text-aHR0cDovL2Jsb2cuY3Nkbi5uZXQv-font-5a6L5L2T-fontsize-400-fill-I0JBQkFCMA==-dissolve-70-gravity-SouthEast.png) ![](../images/dn.net-20150812201407487-watermark-2-text-aHR0cDovL2Jsb2cuY3Nkbi5uZXQv-font-5a6L5L2T-fontsize-400-fill-I0JBQkFCMA==-dissolve-70-gravity-SouthEast.png) ![](../images/dn.net-20150812201420062-watermark-2-text-aHR0cDovL2Jsb2cuY3Nkbi5uZXQv-font-5a6L5L2T-fontsize-400-fill-I0JBQkFCMA==-dissolve-70-gravity-SouthEast.png)

恩 看完了有什么感觉没有 那些红色的线就可以当作传说中的扫描线 就像从左到右扫描嘛 可以发现 矩形有竖直边的地方就有这些线 这些线把把拼在一起的矩形切成了一个个的小矩形 我们把这些小矩形的面积加起来不就是要求的面积吗

那么现在这些小矩形的面积怎么求呢 长乘宽嘛 长是什么 两条红线之间的距离 恩 我们要先把所有矩形的竖直边存起来 然后按横坐标排个序 那么相邻两个竖边的横坐标的差不就是长吗

那么宽呢 宽就是扫描线右边有矩形部分的长度呀 那么现在的问题就是怎么求这个长度了 我们是依次对矩形竖边进行扫描的 我们的扫描线就可以用来维护这个长度(len) 开始让扫描线上所有位置值(cnt)都为0 每来一条竖边 如果这个竖边是矩形左边的边(入边) 我们就让扫描线对应区间的值都加上1 如果这个竖边是矩形右边的边(出边) 我们就让扫描线对应区间的值都减去1 那么我们每次只用知道扫描线上非0部分有多长 这个长度不就是扫描线右边有矩形部分的长度了

噢 对区间进行加1减1操作 是不是很熟悉呀

你可以当作普通的区间更新来做 但是扫描线有个特殊的性质可以让你省掉lazy标记 因为减1的区间肯定是之前加过1的 我们更新时找到满足条件的区间后不用再往下推(pushdown)了 而且只有cnt = 0的地方才需要pushup

由于这道题的y坐标的范围比较大 而且不是整数 所以需要进行离散化操作 离散化只用把原来的值替换为排序后的下标就行了 因为这样不会改变他们y坐标的相对顺序 计算len的时候再把下标对应的y值取出来

```js 
#include <cstdio>
#include <cstring>
#include <algorithm>
#define lc p<<1, s, mid
#define rc p<<1|1, mid + 1, e
#define mid ((s+e)>>1)
using namespace std;
const int N = 205;
int cnt[N * 4];
double len[N * 4], y[N];

struct SLine
{
    double x, y1, y2;
    int flag;
    SLine() {}
    SLine(double a, double b, double c, int t):
        x(a), y1(b), y2(c), flag(t) {}
    bool operator< (const SLine &s) const
    {
        return x < s.x;
    }
} line[N];

void pushup(int p, int s, int e) //只有cnt = 0时才需要更新
{
    if(cnt[p]) len[p] = y[e] - y[s - 1];
    else if(s == e) len[p] = 0;
    else len[p] = len[p << 1] + len[p << 1 | 1];
}

void build()
{
    memset(len, 0, sizeof(len));
    memset(cnt, 0, sizeof(cnt));
}

void update(int p, int s, int e, int l, int r, int v)
{
    if(l <= s && e <= r)
    {
        cnt[p] += v;
        pushup(p, s, e);
        return;
    }
    if(l <= mid) update(lc, l, r, v);
    if(r > mid) update(rc, l, r, v);
    pushup(p, s, e);
}

int main()
{
    int n, m, cas = 0;
    double x1, y1, x2, y2;
    while(scanf("%d", &n), n)
    {
        build();
        m = 0;
        for(int i = 0; i < n; ++i)
        {
            scanf("%lf%lf%lf%lf", &x1, &y1, &x2, &y2);
            y[m] = y1, y[m + 1] = y2;
            line[m++] = SLine(x1, y1, y2, 1);
            line[m++] = SLine(x2, y1, y2, -1);
        }
        sort(line, line + m);
        sort(y, y + m); //离散化

        double sum = 0;
        for(int i = 0; i < m - 1; ++i)
        {
            int l = lower_bound(y, y + m, line[i].y1) - y;
            int r = lower_bound(y, y + m, line[i].y2) - y;
            update(1, 1, m, l + 1, r, line[i].flag);
            //l+1是点区间化段区间时长度需减1
            sum += len[1] * (line[i + 1].x - line[i].x);
        }
        printf("Test case #%d\nTotal explored area: %.2f\n\n", ++cas, sum);
    }
    return 0;
}
```

# Atlantis

Problem Description

There are several ancient Greek texts that contain descriptions of the fabled island Atlantis. Some of these texts even include maps of parts of the island. But unfortunately, these maps describe different regions of Atlantis. Your friend Bill has to know the total area for which maps exist. You (unwisely) volunteered to write a program that calculates this quantity.
Input

The input file consists of several test cases. Each test case starts with a line containing a single integer n (1<=n<=100) of available maps. The n following lines describe one map each. Each of these lines contains four numbers x1;y1;x2;y2 (0<=x1<x2<=100000;0<=y1<y2<=100000), not necessarily integers. The values (x1; y1) and (x2;y2) are the coordinates of the top-left resp. bottom-right corner of the mapped area.
The input file is terminated by a line containing a single 0. Don’t process it.
Output

For each test case, your program should output one section. The first line of each section must be “Test case /#k”, where k is the number of the test case (starting with 1). The second one must be “Total explored area: a”, where a is the total explored area (i.e. the area of the union of all rectangles in this test case), printed exact to two digits to the right of the decimal point.
Output a blank line after each test case.
Sample Input

2 10 10 20 20 15 15 25 25.5 0
Sample Output

Test case /#1 Total explored area: 180.00