---
title: Codeforce 264 A Caisa and Sugar(简单)
author: Deng
tags: 
       - Algorithm

category: 
       - Other
       - OJ

date: 2014-08-30 18:29:17
---
Caisa去商店买Sugar 商店找零的最低单位为元 低于1元的部分每分找一个糖果 商店有n种Suger Caisa有s元钱 她只买一种Sugar 输入n s 再输入n行 每行两个数a b 表示第i种Sugar的单价为a元b分 求Sugar最多能被找多少糖果

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
int main()
{
    int  n, s, cos;
    while (~scanf ("%d%d", &n, &s))
    {
        s *= 100;
        int flag = 0, a, b, ans = 0;
        while (n--)
        {
            scanf ("%d%d", &a, &b);
            cos = a * 100 + b;
            for (int j = 1; cos * j <= s; ++j)
            {
                flag = 1;
                ans = max (ans, (s - cos) % 100);
            }
        }
        printf ("%d\n", flag ? ans : -1);
    }
    return 0;
}
```

Caisais going to have a party and he needs to buy the ingredients for a big chocolate cake. For that he is going to the biggest supermarket in town.

Unfortunately, he has just*s*dollars for sugar. But that's not a reason to be sad, because there are*n*types of sugar in the supermarket, maybe he able to buy one. But that's not all. The supermarket has very unusual exchange politics: instead of cents the sellers give sweets to a buyer as a change. Of course, the number of given sweets always doesn't exceed99, because each seller maximizes the number of dollars in the change (100cents can be replaced with a dollar).

Caisa wants to buy only one type of sugar, also he wants to maximize the number of sweets in the change. What is the maximum number of sweets he can get? Note, that Caisa doesn't want to minimize the cost of the sugar, he only wants to get maximum number of sweets as change.
Input

The first line contains two space-separated integers*n*, *s*(1 ≤ *n*, *s* ≤ 100).

The*i*-th of the next*n*lines contains two integers*x**i*,*y**i*(1 ≤ *x**i* ≤ 100; 0 ≤ *y**i* < 100), where*x**i*represents the number of dollars and*y**i*the number of cents needed in order to buy the*i*-th type of sugar.

Output

Print a single integer representing the maximum number of sweetshe can buy, or-1if he can't buy any type of sugar.
Sample test(s)

input 5 10 3 90 12 0 9 70 5 50 7 0

output 50
input 5 5 10 10 20 20 30 30 40 40 50 50

output -1

Note

In the first test sample Caisa can buy the fourth type of sugar, in such a case he will take50sweets as a change.

﻿﻿