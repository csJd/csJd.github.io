---
title: hihoCoder 1078 线段树的区间修改
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2014-12-11 00:39:49
---
还是最基础的线段树噢 这次是区间修改

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
#define lc p<<1,s,mid
#define rc p<<1|1,mid+1,e
#define mid (s+e)/2

using namespace std;
const int N = 100005;
int sum[4 * N], setv[4 * N];
void pushup(int p)
{
    sum[p] = (sum[p << 1] + sum[p << 1 | 1]);
}

void pushdown(int p, int s, int e)
{
    if(setv[p] == 0) return;
    sum[p << 1] = (mid - s + 1) * setv[p];
    sum[p << 1 | 1] = (e - mid) * setv[p];
    setv[p << 1] = setv[p];
    setv[p << 1 | 1] = setv[p];
    setv[p] = 0;
}

void build(int p, int s, int e)
{
    setv[p] = 0;
    if(s == e) scanf("%d", &sum[p]);
    else
    {
        build(lc);
        build(rc);
        pushup(p);
    }
}

void update(int p, int s, int e, int l, int r, int v)
{
    if(s >= l && e <= r)
    {
        setv[p] = v;
        sum[p] = (e - s + 1) * v;
        return;
    }
    pushdown(p, s, e);
    if(r <= mid) update(lc, l, r, v);
    else if(l > mid) update(rc, l, r, v);
    else update(lc, l, mid, v), update(rc, mid + 1, r, v);
    pushup(p);
}

int query(int p, int s, int e, int l, int r)
{
    if(s >= l && e <= r) return sum[p];
    pushdown(p, s, e);
    if(r <= mid) return query(lc, l, r);
    if(l > mid) return query(rc, l, r);
    return query(lc, l, mid) + query(rc, mid + 1, r);
}

int main()
{
    int n, m, a, b, c, op;
    while(~scanf("%d", &n))
    {
        build(1, 1, n);
        scanf("%d", &m);
        while(m--)
        {
            scanf("%d", &op);
            if(op)
            {
                scanf("%d%d%d", &a, &b, &c);
                update(1, 1, n, a, b, c);
            }
            else
            {
                scanf("%d%d", &a, &b);
                printf("%d\n", query(1, 1, n, a, b));
            }
        }
    }
    return 0;
}
```

时间限制: 10000ms

单点时限: 1000ms
内存限制: 256MB

#### 描述

对于小Ho表现出的对线段树的理解，小Hi表示挺满意的，但是满意就够了么？于是小Hi将问题改了改，又出给了小Ho：

假设货架上从左到右摆放了N种商品，并且依次标号为1到N，其中标号为i的商品的价格为Pi。小Hi的每次操作分为两种可能，第一种是修改价格——小Hi给出一段区间[L, R]和一个新的价格NewP，所有标号在这段区间中的商品的价格都变成NewP。第二种操作是询问——小Hi给出一段区间[L, R]，而小Ho要做的便是计算出所有标号在这段区间中的商品的总价格，然后告诉小Hi。

那么这样的一个问题，小Ho该如何解决呢？

[提示：推动科学发展的除了人的好奇心之外还有人的懒惰心！](http://hihocoder.com/problemset/problem/1078#)

#### 输入

每个测试点（输入文件）有且仅有一组测试数据。

每组测试数据的第1行为一个整数N，意义如前文所述。

每组测试数据的第2行为N个整数，分别描述每种商品的重量，其中第i个整数表示标号为i的商品的重量Pi。

每组测试数据的第3行为一个整数Q，表示小Hi进行的操作数。

每组测试数据的第N+4~N+Q+3行，每行分别描述一次操作，每行的开头均为一个属于0或1的数字，分别表示该行描述一个询问和一次商品的价格的更改两种情况。对于第N+i+3行，如果该行描述一个询问，则接下来为两个整数Li, Ri，表示小Hi询问的一个区间[Li, Ri]；如果该行描述一次商品的价格的更改，则接下来为三个整数Li，Ri，NewP，表示标号在区间[Li, Ri]的商品的价格全部修改为NewP。

对于100%的数据，满足N<=10^5，Q<=10^5, 1<=Li<=Ri<=N，1<=Pi<=N, 0<Pi, NewP<=10^4。

#### 输出

对于每组测试数据，对于每个小Hi的询问，按照在输入中出现的顺序，各输出一行，表示查询的结果：标号在区间[Li, Ri]中的所有商品的价格之和。
样例输入 10 4733 6570 8363 7391 4511 1433 2281 187 5166 378 6 1 5 10 1577 1 1 7 3649 0 8 10 0 1 4 1 6 8 157 1 3 4 1557 样例输出 4731 14596