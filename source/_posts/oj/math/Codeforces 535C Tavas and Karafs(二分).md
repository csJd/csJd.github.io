---
title: Codeforces 535C Tavas and Karafs(二分)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Mathematics

date: 2015-04-15 16:26:19
---
题意 有一个等差数列 从A开始 公差为B然后n个询问 每个询问给定l,t,m然后要求如果每次可以最多选择m个数 使这m个数-1 那么在t次操作中可以使l为左端点的最长序列中使所有数为0 输出这个最长序列的右端序号

定理 序列h1,h2,...,hn 可以在t次时间内(每次至多让m个元素减少1) 全部减小为0 当且仅当

**max(h1, h2, ..., hn) <= t && h1 + h2 + ... + hn <= m/*t**

那么就可以二分右端点来解决了 下限为l 上限为hi不超过t的最大i

```js 
#include <bits/stdc++.h>
typedef long long LL;
LL a, b, n, l, t, m;


LL getv(LL p)
{
    return a + (p - 1) * b;
}


LL getsum(LL r)
{
    return (getv(r) + getv(l)) * (r - l + 1) / 2;
}


int main()
{
    scanf("%lld%lld%lld", &a, &b, &n);
    while(n--)
    {
        scanf("%lld%lld%lld", &l, &t, &m);
        if(getv(l) > t) puts("-1");
        else
        {
            LL le = l, ri = (t - a) / b + 1, mid;
            while(le <= ri)
            {
                mid = (ri + le) / 2;
                if(getsum(mid) <= t * m) le = mid + 1;
                else ri = mid - 1;
            }
            printf("%lld\n", le - 1);
        }
    }
    return 0;
}
```

C. Tavas and Karafs

Karafs is some kind of vegetable in shape of an 1 × *h* rectangle. Tavaspolis people love Karafs and they use Karafs in almost any kind of food. Tavas, himself, is crazy about Karafs.
![](../images/eforces.com-ea42fba30e24759438ea5bbfb0a4e6db9fa0050f-.png)

Each Karafs has a positive integer height. Tavas has an infinite 1-based sequence of Karafses. The height of the *i*-th Karafs is *s**i* = *A* + (*i* - 1) × *B*.

For a given *m*, let's define an *m*-bite operation as decreasing the height of at most *m* distinct not eaten Karafses by 1. Karafs is considered as eaten when its height becomes zero.

Now SaDDas asks you *n* queries. In each query he gives you numbers *l*, *t* and *m* and you should find the largest number *r* such that *l* ≤ *r*and sequence *s**l*, *s**l* + 1, ..., *s**r* can be eaten by performing *m*-bite no more than *t* times or print -1 if there is no such number *r*.
Input

The first line of input contains three integers *A*, *B* and *n* (1 ≤ *A*, *B* ≤ 106, 1 ≤ *n* ≤ 105).

Next *n* lines contain information about queries. *i*-th line contains integers *l*, *t*, *m* (1 ≤ *l*, *t*, *m* ≤ 106) for *i*-th query.

Output

For each query, print its answer in a single line.
Sample test(s)

input 2 1 4 1 5 3 3 3 10 7 10 2 6 4 8

output 4 -1 8 -1
input 1 5 2 1 5 10 2 7 4

output 1 2