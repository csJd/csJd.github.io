---
title: UVa 1152 4Values whose Sum is 0
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Other

date: 2015-01-29 10:49:22
---
题意 从4个n元集中各挑出一个数 使它们的和为零有多少种方法

直接n^4枚举肯定会超时的 可以把两个集合的元素和放在数组里 然后排序 枚举另外两个集合中两元素和 看数组中是否有其相反数就行了 复杂度为n^2/*logn

```js 
#include <bits/stdc++.h>
#define l(i) lower_bound(s,s+m,i)
#define u(i) upper_bound(s,s+m,i)
using namespace std;
const int N = 4005;
int a[N], b[N], c[N], d[N], s[N * N];

int main()
{
    int cas, m, k, n;
    long long ans;
    scanf("%d", &cas);
    while(cas--)
    {
        ans = m = 0;
        scanf("%d", &n);
        for(int i = 0; i < n; ++i)
            scanf("%d%d%d%d", &a[i], &b[i], &c[i], &d[i]);
        for(int i = 0; i < n; ++i)
            for(int j = 0; j < n; ++j)
                s[m++] = a[i] + b[j];
        sort(s, s + m);
        for(int i = 0; i < n; ++i)
            for(int j = 0; j < n; ++j)
                k = -c[i] - d[j], ans += (u(k) - l(k));

        printf("%lld\n", ans);
        if(cas) puts("");
    }
    return 0;
}
```

The SUM problem can be formulated as follows: given four lists *A*, *B*, *C*, *D* of integer values, compute how many quadruplet (*a*, *b*, *c*, *d* ) ![$ \in$](../images/dge.org-external-11-3506img1-.png) *A* x *B* x *C* x *D* are such that *a* + *b* + *c* + *d* = 0 . In the following, we assume that all lists have the same size *n* .

##

**The input begins with a single positive integer on a line by itself indicating the number of the cases following, each of them as described below. This line is followed by a blank line, and there is also a blank line between two consecutive inputs.**

The first line of the input file contains the size of the lists *n* (this value can be as large as 4000). We then have *n* lines containing four integer values (with absolute value as large as 228 ) that belong respectively to *A*, *B*, *C* and *D* .

##

**For each test case, the output must follow the description below. The outputs of two consecutive cases will be separated by a blank line.**

For each input file, your program has to write the number quadruplets whose sum is zero.

##

1 6 -45 22 42 -16 -41 -27 56 30 -36 53 -37 77 -36 30 -75 -46 26 -38 -10 62 -32 -54 -6 45

##

5

Sample Explanation: Indeed, the sum of the five following quadruplets is zero: (-45, -27, 42, 30), (26, 30, -10, -46), (-32, 22, 56, -46),(-32, 30, -75, 77), (-32, -54, 56, 30).