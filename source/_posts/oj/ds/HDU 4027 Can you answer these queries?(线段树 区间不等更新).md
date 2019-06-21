---
title: HDU 4027 Can you answer these queries?(线段树 区间不等更新)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-04-19 09:34:08
---
题意 输入n个数 然后有两种操作 输入0时将给定区间所有数都变为自己的开方 输入1输出给定区间所有数的和

虽然是区间更新 但每个点更新的不一样 因此只能对单点进行更新 其实一个点最多被更新7次 2^64开平方7次后就变为1了 如果某个区间的数都变为了1 那么对这个区间的开方就不用考虑了 另外要注意给你的区间可能是反的

```js 
#include <bits/stdc++.h>
#define lc p<<1,s,mid
#define rc p<<1|1,mid+1,e
#define mid ((s+e)>>1)
using namespace std;
typedef long long LL;
const int N = 100005;
LL sum[N * 4];

void pushup(int p)
{
    sum[p] = sum[p << 1] + sum[p << 1 | 1];
}

void build(int p, int s, int e)
{
    if(s == e)
    {
        scanf("%lld", &sum[p]);
        return;
    }
    build(lc);
    build(rc);
    pushup(p);
}

void update(int p, int s, int e, int l, int r)
{
    if(sum[p] == e - s + 1) return;  //[e,s]区间所有数都为1
    if(s == e)
    {
        sum[p] = sqrt(sum[p]);
        return;
    }
    if(l <= mid) update(lc, l, r);
    if(r > mid) update(rc, l, r);
    pushup(p);
}

LL query(int p, int s, int e, int l, int r)
{
    if(s == l && e == r) return sum[p];
    if(r <= mid) return query(lc, l, r);
    if(l > mid) return query(rc, l, r);
    return query(lc, l, mid) + query(rc, mid + 1, r);
}

int main()
{
    int n, m, a, b, c, k = 0;
    while(~scanf("%d", &n))
    {
        printf("Case #%d:\n", ++k);
        build(1, 1, n);
        scanf("%d", &m);
        while(m--)
        {
            scanf("%d%d%d", &c, &a, &b);
            if(b < a) swap(a, b);
            if(c) printf("%lld\n", query(1, 1, n, a, b));
            else update(1, 1, n, a, b);
        }
        puts("");
    }
    return 0;
}
```

# Can you answer these queries?

Problem Description

A lot of battleships of evil are arranged in a line before the battle. Our commander decides to use our secret weapon to eliminate the battleships. Each of the battleships can be marked a value of endurance. For every attack of our secret weapon, it could decrease the endurance of a consecutive part of battleships by make their endurance to the square root of it original value of endurance. During the series of attack of our secret weapon, the commander wants to evaluate the effect of the weapon, so he asks you for help.
You are asked to answer the queries that the sum of the endurance of a consecutive part of the battleship line.
Notice that the square root operation should be rounded down to integer.
Input

The input contains several test cases, terminated by EOF.
For each test case, the first line contains a single integer N, denoting there are N battleships of evil in a line. (1 <= N <= 100000)
The second line contains N integers Ei, indicating the endurance value of each battleship from the beginning of the line to the end. You can assume that the sum of all endurance value is less than 2 63.
The next line contains an integer M, denoting the number of actions and queries. (1 <= M <= 100000)
For the following M lines, each line contains three integers T, X and Y. The T=0 denoting the action of the secret weapon, which will decrease the endurance value of the battleships between the X-th and Y-th battleship, inclusive. The T=1 denoting the query of the commander which ask for the sum of the endurance value of the battleship between X-th and Y-th, inclusive.
Output

For each test case, print the case number at the first line. Then print one line for each query. And remember follow a blank line after each test case.
Sample Input

10 1 2 3 4 5 6 7 8 9 10 5 0 1 10 1 1 10 1 1 5 0 5 8 1 4 8
Sample Output

Case /#1: 19 7 6
Source

[The 36th ACM/ICPC Asia Regional Shanghai Site —— Online Contest](http://acm.hdu.edu.cn/search.php?field=problem&key=The+36th+ACM%2FICPC+Asia+Regional+Shanghai+Site+%A1%AA%A1%AA+Online+Contest&source=1&searchmode=source)