---
title: XTU1238 Segment Tree (线段树·区间最值更新)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-07-05 10:18:01
---
题意 对一个数组有四种操作

对每个操作4输出对应最小值和最大值

基础的线段树 在湘潭卡了好久没写出来
线段树维护三个值 区间最大值 maxv, 区间最小值minv, 区间增加的值add
操作1是很容易实现的 操作2和3比1要复杂些 初一想要对每个值单独进行操作 这样肯定会T的 毕竟那样用线段树就没意义了 后来发现每次更新时对应区间包含的所有最大区间节点的maxv 和 minv是可以确定的

然后知道了父区间的maxv,minv后 通过pashdown函数就可以在需要时再对子区间进行更新 因为子区间的所有值都是在父区间的minv和maxv之间 然后就看代码吧
```js 
#include <bits/stdc++.h>
#define lc p<<1, s, mid
#define rc p<<1|1, mid+1, e
#define mid ((s+e)>>1)
using namespace std;
const int N = 200005;
int maxv[N * 4], minv[N * 4], add[N * 4];

void pushup(int p)
{
    maxv[p] = max(maxv[p << 1], maxv[p << 1 | 1]);
    minv[p] = min(minv[p << 1], minv[p << 1 | 1]);
}

void pushdown(int p)
{
    int l = p << 1;
    int r = p << 1 | 1;

    if(add[p])
    {
        add[l] += add[p];
        add[r] += add[p];
        minv[l] += add[p];
        minv[r] += add[p];
        maxv[l] += add[p];
        maxv[r] += add[p];
        add[p] = 0;
    }

    //子区间的所有值肯定都父区间的minv~maxv之间
    minv[l] = max(minv[l], minv[p]);
    minv[l] = min(minv[l], maxv[p]);
    maxv[l] = max(maxv[l], minv[p]);
    maxv[l] = min(maxv[l], maxv[p]);

    minv[r] = max(minv[r], minv[p]);
    minv[r] = min(minv[r], maxv[p]);
    maxv[r] = max(maxv[r], minv[p]);
    maxv[r] = min(maxv[r], maxv[p]);
}

void build(int p, int s, int e)
{
    if(s == e)
    {
        scanf("%d", &maxv[p]);
        minv[p] = maxv[p];
        return;
    }
    build(lc);
    build(rc);
    pushup(p);
}

void update(int p, int s, int e, int l, int r, int v, int op)
{
    if(s >= l && e <= r)
    {
        if(op == 1) //将[l,r]区间的所有值都加上v
        {
            add[p] += v;
            minv[p] += v;
            maxv[p] += v;
        }
        else if (op == 2)  //将[l,r]区间中所有大于v的值都改为v
        {
            maxv[p] = min(maxv[p], v);
            minv[p] = min(minv[p], v);
        }
        else  //op = 3  将[l,r]区间中的所有小于v的值都改为v
        {
            maxv[p] = max(v, maxv[p]);
            minv[p] = max(v, minv[p]);
        }
        return;
    }
    pushdown(p);
    if(l <= mid) update(lc, l, r, v, op);
    if(r > mid) update(rc, l, r, v, op);
    pushup(p);
}

int query(int p, int s, int e, int l, int r, int op)
{
    if(s == l && e == r)//op = 0 时查询最小值  op = 1 时查询最大值
        return op ? maxv[p] : minv[p];
    pushdown(p);
    if(r <= mid) return query(lc, l, r, op);
    if(l > mid) return query(rc, l, r, op);
    if(op) return max(query(lc, l, mid, op), query(rc, mid + 1, r, op));
    return min(query(lc, l, mid, op), query(rc, mid + 1, r, op));
}

int main()
{
    int T, n, q, l, r, op, c, ax, in;
    scanf("%d", &T);
    while(T--)
    {
        memset(add, 0, sizeof(add));
        scanf("%d%d", &n, &q);
        build(1, 1, n);
        while(q--)
        {
            scanf("%d%d%d", &op, &l, &r);
            if(op == 4)
            {
                ax = query(1, 1, n, l, r, 1);
                in = query(1, 1, n, l, r, 0);
                printf("%d %d\n", in, ax);
            }
            else
            {
                scanf("%d", &c);
                update(1, 1, n, l, r, c, op);
            }
        }
    }
    return 0;
}
```

Segment Tree

Problem Description:

A contest is not integrity without problems about data structure.
There is an array a[1],a[2],…,a[n]. And q questions of the following 4 types: • 1 l r c - Update a[k] with a[k]+c for all l≤k≤r
• 2 l r c - Update a[k] with min{a[k],c} for all l≤k≤r;
• 3 l r c - Update a[k] with max{a[k],c} for all l≤k≤r;
• 4 l r - Ask for min{a[k]:l≤k≤r} and max{a[k]:l≤k≤r}.

Input

The first line contains a integer T(no more than 5) which represents the number of test cases.

For each test case, the first line contains 2 integers n,q (1≤n,q≤200000).

The second line contains n integers a1,a2,…,an which indicates the initial values of the array (|ai|≤).

Each of the following q lines contains an integer t which denotes the type of i-th question. If t=1,2,3, 3 integers l,r,c follows. If t=4, 2 integers l,r follows. (1≤ti≤4,1≤li≤ri≤n)

If t=1, |ci|≤2000;

If t=2,3, |ci|≤10^9.

Output

For each question of type 4, output two integers denote the minimum and the maximum.

Sample Input

1
1 1
1
4 1 1

Sample Output

1 1

Source
XTU OnlineJudge