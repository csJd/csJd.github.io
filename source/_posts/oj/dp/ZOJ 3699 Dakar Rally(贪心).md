---
title: ZOJ 3699 Dakar Rally(贪心)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2015-07-08 08:36:18
---
题意 路上有 n 个加油站 每个加油站的价格可能不同 你的油箱容积为 v 问从起点开车到终点加油至少用多少钱

贪心 每次都让油箱里面便宜的油最多就行了 在每个站点 i 有两种情况

1. i 点把油加满跑完都没有更便宜的 那么在 i 点肯定要加满 然后开到 i+1 点去

2. i 点把油加满能跑到第一个比 i 点更便宜的 j 点或者到了终点 j 那么只用把油加到能到 j 点 然后直接开到 j 点

然后把每个点加油用的钱加起来就行了

```js 
#include <cstdio>
using namespace std;
const int N = 100005;
typedef long long ll;
ll len[N], cost[N], pri[N], v;

int main()
{
    int T, n;
    scanf("%d", &T);

    while(T--)
    {
        scanf("%d%lld", &n, &v);
        int flag = 0;

        for(int i = 0; i < n; i++)
        {
            scanf("%lld%lld%lld", &len[i], &cost[i], &pri[i]);
            if(len[i] * cost[i] > v) flag = 1;
        }

        if(flag)
        {
            puts("Impossible");
            continue;
        }

        int i = 0, j;
        ll r = 0, ans = 0, t;                 //r记录到i点还剩多少油
        while(i < n)
        {
            j = i + 1, t = len[i] * cost[i];  //t记录到下一个便宜的点用了多少油

            //往前走直到找到一个更便宜的站j  或者油箱跑完
            while( j < n && pri[j] >= pri[i] && v - t >= cost[j] * len[j])
            {
                t += len[j] * cost[j];
                ++j;
            }


            if( j >= n || pri[j] < pri[i])  //j点油第一个比i点便宜  加到能到j
            {
                if(r > t)  r -= t;
                else
                {
                    ans += (t - r)  * pri[i];
                    r = 0;
                }
                i = j;
            }

            else  //油跑完都没更便宜的 加满 到下一站
            {
                ans +=  (v - r) * pri[i];
                r = v - len[i] * cost[i];
                i++;
            }
        }

        printf("%lld\n", ans);
    }

    return 0;
}
```
  Dakar Rally    Time Limit:2 Seconds Memory Limit:65536 KB

#### Description

The **Dakar Rally** is an annual Dakar Series rally raid type of off-road race, organized by the Amaury Sport Organization. The off-road endurance race consists of a series of routes. In different routes, the competitors cross dunes, mud, camel grass, rocks, erg and so on.

Because of the various circumstances, the mileages consume of the car and the prices of gas vary from each other. Please help the competitors to minimize their payment on gas.

Assume that the car begins with an empty tank and each gas station has infinite gas. The racers need to finish all the routes in order as the test case descripts.

#### Input

There are multiple test cases. The first line of input contains an integer *T* (*T* ≤ 50) indicating the number of test cases. Then *T* test cases follow.

The first line of each case contains two integers: *n* -- amount of routes in the race; *capacity* -- the capacity of the tank.

The following *n* lines contain three integers each: *mileagei* -- the mileage of the *ith* route; *consumei* -- the mileage consume of the car in the *ith* route , which means the number of gas unit the car consumes in 1 mile; pricei -- the price of unit gas in the gas station which locates at the beginning of the *ith* route.

All integers are positive and no more than 105.

#### Output

For each test case, print the minimal cost to finish all of the *n* routes. If it's impossible, print "Impossible" (without the quotes).

#### Sample Input

2 2 30 5 6 9 4 7 10 2 30 5 6 9 4 8 10

#### Sample Output

550 Impossible