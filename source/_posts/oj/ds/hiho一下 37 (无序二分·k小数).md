---
title: hiho一下 37 (无序二分·k小数)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2015-03-16 00:49:06
---
题意 中文

可以先排序然后输出第k个 复杂度为O(N/*logN) 但有更快的方法 其实二分时只要能保证mid左边的数都比mid小 mid右边的数都比mid大就能进行划分了 对于k不在的区间就不用管了于是可以用到快排的思想

```js 
#include <cstdio>
using namespace std;
const int N = 1000005;
int a[N];
int main()
{
    int n, k, l, r, le, ri, m;
    while(~scanf("%d%d", &n, &k))
    {
        for(int i = 0; i < n; ++i) scanf("%d", &a[i]);
        if(n < k) {puts("-1"); continue;}
        l = 0, r = n - 1;
        while(l <= r)
        {
            m = a[le = l], ri = r;
            while(le < ri)
            {
                while(le < ri && a[ri] > m) --ri;
                a[le] = a[ri];
                while(le < ri && a[le] < m) ++le;
                a[ri] = a[le];
            }
            a[le] = m;
            if(le < k - 1) l = le + 1;
            else r = le - 1;
        }
        printf("%d\n", a[r + 1]);
    }
    return 0;
}
```

时间限制: 10000ms

单点时限: 1000ms
内存限制: 256MB

#### 描述

在上一回里我们知道Nettle在玩《艦これ》，Nettle的镇守府有很多船位，但船位再多也是有限的。Nettle通过捞船又出了一艘稀有的船，但是已有的N(1≤N≤1,000,000)个船位都已经有船了。所以Nettle不得不把其中一艘船拆掉来让位给新的船。Nettle思考了很久，决定随机选择一个k，然后拆掉稀有度第k小的船。 已知每一艘船都有自己的稀有度，Nettle现在把所有船的稀有度值告诉你，希望你能帮他找出目标船。

[提示：非有序数组的二分查找之二](http://hihocoder.com/contest/hiho37/problem/1#)

#### 输入

第1行：2个整数N,k。N表示数组长度，
第2行：N个整数，表示a[1..N]，保证不会出现重复的数，1≤a[i]≤2,000,000,000。

#### 输出

第1行：一个整数t，表示t在数组中是第k小的数，若K不在数组中，输出-1。
样例输入 10 4 1732 4176 2602 6176 1303 6207 3125 1 1011 6600 样例输出 1732