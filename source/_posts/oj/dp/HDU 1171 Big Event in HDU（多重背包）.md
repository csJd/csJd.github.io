---
title: HDU 1171 Big Event in HDU（多重背包）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:51:16
---
# Big Event in HDU

Problem Description

Nowadays, we all know that Computer College is the biggest department in HDU. But, maybe you don't know that Computer College had ever been split into Computer College and Software College in 2002.
The splitting is absolutely a big event in HDU! At the same time, it is a trouble thing too. All facilities must go halves. First, all facilities are assessed, and two facilities are thought to be same if they have the same value. It is assumed that there is N (0<N<1000) kinds of facilities (different value, different kinds).
Input

Input contains multiple test cases. Each test case starts with a number N (0 < N <= 50 -- the total number of different facilities). The next N lines contain an integer V (0<V<=50 --value of facility) and an integer M (0<M<=100 --corresponding number of the facilities) each. You can assume that all V are different.
A test case starting with a negative integer terminates input and this test case is not to be processed.
Output

For each case, print one line containing two integers A and B which denote the value of Computer College and Software College will get respectively. A and B should be as equal as possible. At the same time, you should guarantee that A is not less than B.
Sample Input

2 10 1 20 1 3 10 1 20 2 30 1 -1
Sample Output

20 10 40 40

题意 把一堆东西尽量分为两份 第一份不小于第二份

把所有东西的总价值s除以2 让它装尽量多的东西作为第二份 剩下的就是第一份了

题目有个小坑点 是以负数作为结束条件的 不是-1 还有不要开始把s/=2 后来第一份又用s/*2-d[s] 因为s/2/*2不一定等于s了

```js 
<span style="font-family:Arial Black;">#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 55,  V = 255000;
int d[V], val[N], num[N];
int main()
{
    int n;
    while (scanf ("%d", &n),n>0)
    {
        int s = 0;
        memset(d,0,sizeof(d));
        for (int i = 1; i <= n; ++i)
        {
            scanf ("%d%d", &val[i], &num[i]);
            s += val[i] * num[i];
        }
        
        for (int i = 1; i <= n; ++i)
        {
            for (int k = 1; k<=num[i]; k *=2)
            {
                num[i] -= k;
                for (int j = s / 2; j >= k * val[i]; --j)
                    d[j] = max (d[j], d[j - k * val[i]] + k * val[i]);
            }
            if (num[i] != 0)
                for (int j = s / 2; j >= num[i] * val[i]; --j)
                    d[j] = max (d[j], d[j - num[i] * val[i]] + num[i] * val[i]);
        }
        printf ("%d %d\n", s - d[s/2], d[s/2]);
    }
    return 0;
}
</span>
```