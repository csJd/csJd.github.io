---
title: HDU 1540 Tunnel Warfare(线段树 区间合并 最大连续区间)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-04-22 14:25:48
---
题意 有n个连在一起的地道 接下来有m个操作 D x 炸掉x号地道 炸掉后x所在的区间就不连续了 Q x 查询输出包括x的最大连续区间长度 R修复最后一个被炸的地道 注意输入R时可能并没有需要修复的地道

线段树的区间合并问题 线段树要维护3个信息

len 对应区间的最大连续长度

ll 对应区间最左端的一段连续长度

lr 对应区间最右端的一段连续长度

那么双亲节点的这些信息和孩子节点有什么关系呢容易发现

双亲的len 是 左孩子的len 右孩子的len 左孩子的右端长度+右孩子的左端长度 这三个的最大值

双亲的ll 如果左孩子整个区间都是连续的话 ll就等于左孩子的len加上右孩子的左端长度了

双亲的lr 如果右孩子整个区间都是连续的话 lr就等于右孩子的len加上左孩子的右端长度了

于是就知道pushup函数怎么写了 请看代码

知道这些信息又怎么查询包括x点的最大区间长度呢

肯定是在包含x的区间里面查的

如果根区间整个区间都是连续的 那么根区间长度就是答案了

如果x在左孩子的右端连续段或者右孩子的左端连续段里 那么这两段的和就是答案了

否则就继续查询x所属的孩子区间咯

```js 
#include <bits/stdc++.h>
#define lc lp, s, mid
#define rc rp, mid+1, e
#define lp p<<1
#define rp p<<1|1
#define mid ((s+e)>>1)
using namespace std;
const int N = 50005;
int len[N * 4], lr[N * 4], ll[N * 4];
int n, m;

void pushup(int p, int s, int e)
{
    len[p] = max(len[lp], len[rp]);
    len[p] = max(len[p], lr[lp] + ll[rp]);
    ll[p] = ll[lp], lr[p] = lr[rp];
    if(ll[p] == mid - s + 1) ll[p] += ll[rp];
    if(lr[p] == e - mid) lr[p] += lr[lp];
}

void build(int p, int s, int e)
{
    if(s == e)
    {
        len[p] = ll[p] = lr[p] = 1;
        return;
    }
    build(lc);
    build(rc);
    pushup(p, s, e);
}

void update(int p, int s, int e, int x, int v)
{
    if(s == e && e == x)
    {
        len[p] = ll[p] = lr[p] = v;
        return;
    }
    if(x <= mid) update(lc, x, v);
    else update(rc, x, v);
    pushup(p, s, e);
}

int query(int p, int s, int e, int x)
{
    if(len[p] == 0 || len[p] == e - s + 1)	return len[p];
    if(x <= mid)
        return x > mid - lr[lp] ? lr[lp] + ll[rp] : query(lc, x);
    return x <= ll[rp] + mid ? ll[rp] + lr[lp] : query(rc, x);
}

int main()
{
    char op[10];
    int p;
    while(~scanf("%d%d", &n, &m))
    {
        stack<int> s;
        build(1, 1, n);
        while(m--)
        {
            scanf("%s", op);
            if(op[0] == 'R' && !s.empty())
            {
                p = s.top(), s.pop();
                update(1, 1, n, p, 1);
            }
            else if(op[0] == 'D')
            {
                scanf("%d", &p);
                update(1, 1, n, p, 0);
                s.push(p);
            }
            else
            {
                scanf("%d", &p);
                printf("%d\n", query(1, 1, n, p));
            }
        }
    }
    return 0;
}
```