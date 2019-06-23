---
title: HDU 2844 Coins (组合背包)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:51:26
---
题意 给你n种面额不同的金币和每种金币的个数 求这些金币能组合成的面额在m内有多少种

还是明显的背包问题 d[i]表示这些金币在i内能组合成的最大面额 初始化d为负无穷 d[0]=0 这样就可以保证d[i]恰好为i时才能为正值

原因可以自己想想 然后就用背包背吧 直接多重背包也可以过 但是分成多重背包和完全背包要快一点

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 105, M = 100005;
int main()
{
    int n, m, val[N], num[N], d[M], ans;
    while (scanf ("%d%d", &n, &m), n && m)
    {
        memset (d, 0x8f, sizeof (d));
        d[0] = ans = 0;
        for (int i = 1; i <= n; ++i)
            scanf ("%d", &val[i]);
        for (int i = 1; i <= n; ++i)
            scanf ("%d", &num[i]);
        for (int i = 1; i <= n; ++i)
        {
            if (num[i]*val[i] >= m)
                for (int j = val[i]; j <= m; ++j)
                    d[j] = max (d[j], d[j - val[i]] + val[i]);
            else
            {
                for (int k = 1; k <= num[i]; k *= 2)
                {
                    for (int j = m; j >= k * val[i]; --j)
                        d[j] = max (d[j], d[j - k * val[i]] + k * val[i]);
                    num[i] -= k;
                }
                if (num[i] > 0)
                    for (int j = m; j >= num[i]*val[i]; --j)
                        d[j] = max (d[j], d[j - num[i] * val[i]] + num[i] * val[i]);
            }
        }
        for (int i = 1; i <= m; ++i)
            if (d[i] > 0) ++ans;
        printf ("%d\n", ans);
    }
    return 0;
}
```

# Coins

Problem Description

Whuacmers use coins.They have coins of value A1,A2,A3...An Silverland dollar. One day Hibix opened purse and found there were some coins. He decided to buy a very nice watch in a nearby shop. He wanted to pay the exact price(without change) and he known the price would not more than m.But he didn't know the exact price of the watch.
You are to write a program which reads n,m,A1,A2,A3...An and C1,C2,C3...Cn corresponding to the number of Tony's coins of value A1,A2,A3...An then calculate how many prices(form 1 to m) Tony can pay use these coins.
Input

The input contains several test cases. The first line of each test case contains two integers n(1 ≤ n ≤ 100),m(m ≤ 100000).The second line contains 2n integers, denoting A1,A2,A3...An,C1,C2,C3...Cn (1 ≤ Ai ≤ 100000,1 ≤ Ci ≤ 1000). The last test case is followed by two zeros.
Output

For each test case output the answer on a single line.
Sample Input

3 10 1 2 4 2 1 1 2 5 1 4 2 1 0 0
Sample Output

8 4