---
title: UVa 11054 Wine trading in Gergovia（扫描）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Other

date: 2015-02-03 10:01:33
---
题意 有n个村庄 第i个村庄需要买a[i]的酒 a[i]为负时该村庄可卖掉-a[i]的酒 保证所有a[i]的和为0 一个单位的酒从一个村庄运输到相邻村庄的消耗为1 求运输完所有酒需要的最小消耗

总消耗最少时 每个需要买的村庄都会找离他最近的可以卖的村庄 容易发现 这种状况下 从第一个村和第二个村庄之间的运输量为abs(a[1]) 第二个村庄和第三个村庄之间的运输量为abs(a[1]+a[2]) 第k个村庄到第k+1个村庄的运输量为abs(a[1]+a[2]+...+a[k])

```js 
#include <bits/stdc++.h>
using namespace std;
int main()
{
    int n, t;
    long long ans, last;
    while(~scanf("%d", &n), n)
    {
        ans = last = 0;
        //last记录需要从i-1运输到i的量
        for(int i = 1; i <= n; ++i)
        {
            scanf("%d", &t);
            ans += abs(last), last += t;
        }
        printf("%lld\n", ans);
    }
    return 0;
}
```

# Wine trading in Gergovia

As you may know from the comic "Asterix and the Chieftain's Shield", Gergovia consists of one street, and every inhabitant of the city is a wine salesman. You wonder how this economy works? Simple enough: everyone buys wine from other inhabitants of the city. Every day each inhabitant decides how much wine he wants to buy or sell. Interestingly, demand and supply is always the same, so that each inhabitant gets what he wants.

There is one problem, however: Transporting wine from one house to another results in work. Since all wines are equally good, the inhabitants of Gergovia don't care which persons they are doing trade with, they are only interested in selling or buying a specific amount of wine. They are clever enough to figure out a way of trading so that the overall amount of work needed for transports is minimized.

In this problem you are asked to reconstruct the trading during one day in Gergovia. For simplicity we will assume that the houses are built along a straight line with equal distance between adjacent houses. Transporting one bottle of wine from one house to an adjacent house results in one unit of work.

#### Input Specification

The input consists of several test cases. Each test case starts with the number of inhabitants *n* (2 ≤ *n* ≤ 100000). The following line contains n integers ai (-1000 ≤ ai ≤ 1000). If ai ≥ 0, it means that the inhabitant living in the ith house wants to buy ai bottles of wine, otherwise if ai < 0, he wants to sell -ai bottles of wine. You may assume that the numbers ai sum up to 0.
The last test case is followed by a line containing 0.

#### Output Specification

For each test case print the minimum amount of work units needed so that every inhabitant has his demand fulfilled. You may assume that this number fits into a signed 64-bit integer (in C/C++ you can use the data type "long long", in JAVA the data type "long").

#### Sample Input

5 5 -4 1 -3 1 6 -1000 -1000 -1000 1000 1000 1000 0

#### Sample Output

9 9000