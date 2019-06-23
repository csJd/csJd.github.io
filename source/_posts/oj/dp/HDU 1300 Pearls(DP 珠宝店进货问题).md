---
title: HDU 1300 Pearls(DP 珠宝店进货问题)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-23 16:19:24
---
﻿﻿

题意 珠宝店到珍珠批发商进货 第i种价格为p[i]的珍珠需要n个 则珍珠的结算价格为∑(n+10)/*p[i] 由于没种珍珠的数量结算时都要加上10 所以有时候把便宜的珍珠换为贵的结算价格反而变少了 给你一张购买清单 珍珠价格是递增的 每种珍珠都可以替换为比它贵的 求最少总花费

和上篇博客描述的几乎是一样的 令d[i]表示前i种珍珠的最少花费 sum[i]表示第1种到第第i种的总数 那么有转移方程 d[i]=min{d[j-1]+(sum[i]-sum[j-1]+10)/*p[i]} (sum[i]-sum[j-1]+10)/*p[i]表示第j种到第i种珍珠全部替换为第i种

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 105;
int p[N], d[N], s[N],num, n, cas;
int main()
{
    scanf ("%d", &cas);
    while (cas--)
    {
        scanf ("%d", &n);
        for (int i = 1; i <= n; ++i)
        {
            scanf ("%d%d", &num, &p[i]);
            s[i] = s[i - 1] + num;
        }
        memset(d,0x3f,sizeof(d));
        d[0]=0;
        for (int i = 1; i <= n; ++i)
            for (int j = 1; j <= i; ++j)
                d[i] = min (d[i], d[j-1] + (s[i] - s[j - 1] + 10) * p[i]);
        printf ("%d\n", d[n]);
    }
    return 0;
}
```

# Pearls

Problem Description

In Pearlania everybody is fond of pearls. One company, called The Royal Pearl, produces a lot of jewelry with pearls in it. The Royal Pearl has its name because it delivers to the royal family of Pearlania. But it also produces bracelets and necklaces for ordinary people. Of course the quality of the pearls for these people is much lower then the quality of pearls for the royal family. In Pearlania pearls are separated into 100 different quality classes. A quality class is identified by the price for one single pearl in that quality class. This price is unique for that quality class and the price is always higher then the price for a pearl in a lower quality class.
Every month the stock manager of The Royal Pearl prepares a list with the number of pearls needed in each quality class. The pearls are bought on the local pearl market. Each quality class has its own price per pearl, but for every complete deal in a certain quality class one has to pay an extra amount of money equal to ten pearls in that class. This is to prevent tourists from buying just one pearl.
Also The Royal Pearl is suffering from the slow-down of the global economy. Therefore the company needs to be more efficient. The CFO (chief financial officer) has discovered that he can sometimes save money by buying pearls in a higher quality class than is actually needed. No customer will blame The Royal Pearl for putting better pearls in the bracelets, as long as the prices remain the same.
For example 5 pearls are needed in the 10 Euro category and 100 pearls are needed in the 20 Euro category. That will normally cost: (5+10)/*10 + (100+10)/*20 = 2350 Euro.
Buying all 105 pearls in the 20 Euro category only costs: (5+100+10)/*20 = 2300 Euro.
The problem is that it requires a lot of computing work before the CFO knows how many pearls can best be bought in a higher quality class. You are asked to help The Royal Pearl with a computer program.
Given a list with the number of pearls and the price per pearl in different quality classes, give the lowest possible price needed to buy everything on the list. Pearls can be bought in the requested, or in a higher quality class, but not in a lower one.
Input

The first line of the input contains the number of test cases. Each test case starts with a line containing the number of categories c (1 <= c <= 100). Then, c lines follow, each with two numbers ai and pi. The first of these numbers is the number of pearls ai needed in a class (1 <= ai <= 1000). The second number is the price per pearl pi in that class (1 <= pi <= 1000). The qualities of the classes (and so the prices) are given in ascending order. All numbers in the input are integers.
Output

For each test case a single line containing a single number: the lowest possible price needed to buy everything on the list.
Sample Input

2 2 100 1 100 2 3 1 10 1 11 100 12
Sample Output

330 1344