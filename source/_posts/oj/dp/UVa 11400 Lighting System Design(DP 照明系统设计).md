---
title: UVa 11400 Lighting System Design(DP 照明系统设计)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-23 16:03:26
---
题意 设计某个地方的照明系统 一共需要n种不同类型的灯泡 接着输入 每种灯泡的电压v 对应电压电源的价格k 每个灯泡的价格c 需要这种灯泡的数量l 电压低的灯泡可以用电压高的灯泡替换 每种灯泡只需要一个对应的电源 求完成这个照明系统的最少花费

比较简单的DP 容易知道 当要替换一种灯泡中的一个到令一种电压较高的灯泡时 只有全部替换这种灯泡为另一种时才可能使总花费变小 全部替换掉就省下了这种灯泡的电源花费 先把灯泡按照电压排序 那么每种灯泡都可以替换他前面的任意灯泡了 令s[i]表示前i种灯泡的总数 那么s[i]-s[j-1]表示的是第j种到第[i]种灯泡的总数

令d[i]表示前i种灯泡的最少花费 那么可以得到转移方程 d[i]=min{d[j-1]+(s[i]-s[j-1])/*c[i]+k[i]} j为1到i之间所有数

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 1005;

int d[N], s[N], v[N], k[N], c[N], l[N], o[N], n;

bool cmp (int i, int j)
{
    return v[i] < v[j];
}

int main()
{
    while (~scanf ("%d", &n), n)
    {
        for (int i = 1; i <= n; ++i)
        {
            scanf ("%d%d%d%d", &v[i], &k[i], &c[i], &l[i]);
            o[i] = i;
        }
        sort (o + 1, o + n + 1, cmp);
        memset (d, 0x3f, sizeof (d));
        d[0] = 0;
        for (int i = 1; i <= n; ++i)
        {
            s[i] = s[i - 1] + l[o[i]];
            for (int j = 1; j <= i; ++j)
                d[i] = min (d[i], d[j - 1] + (s[i] - s[j - 1]) * c[o[i]] + k[o[i]]);
        }
        printf ("%d\n", d[n]);
    }
    return 0;
}
```

You are given the task to design a lighting system for a huge conference hall. After doing a lot of calculation & sketching, you have figured out the requirements for an energy-efficient design that can properly illuminate the entire hall. According to your design, you need lamps of**n**different power ratings. For some strange current regulation method, all the lamps need to be fed with the same amount of current. So, each category of lamp has a corresponding voltage rating. Now, you know the number of lamps & cost of every single unit of lamp for each category. But the problem is,you are to buy equivalent voltage sources for all the lamp categories. You can buy a single voltage source for each category (Each source is capable of supplying to infinite number of lamps of its voltage rating.) & complete the design. But the accounts section of your company soon figures out that they might be able to reduce the total system cost by eliminating some of the voltage sources & replacing the lamps of that category with higher rating lamps. Certainly you can never replace a lamp by a lower rating lamp as some portion of the hall might not be illuminated then. You are more concerned about money-saving than energy-saving. Find the minimum possible cost to design the system.

**Input**

****

Each case in the input begins with n (1<=n<=1000), denoting the number of categories. Each of the following n lines describes a category. A category is described by 4 integers -**V**(1<=V<=132000), the voltage rating, K (1<=K<=1000), the cost of a voltage source of this rating, C (1<=C<=10), the cost of a lamp of this rating & L (1<=L<=100), the number of lamps required in this category. The input terminateswitha test case where n = 0. This case should not be processed.

**Output**

****

For each test case, print the minimum possible cost to design the system.

**Sample Input Output for Sample Input**
3

100 500 10 20

120 600 8 16

220 400 7 18

0
 
778

﻿﻿