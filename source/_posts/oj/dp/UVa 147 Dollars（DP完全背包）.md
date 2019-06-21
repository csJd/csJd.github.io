---
title: UVa 147 Dollars（DP完全背包）
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-21 09:42:53
---
#

很水的完全背包题 大家都会的 只是要注意把小数化为整数 不然精度丢失很严重；

```js 
#include<cstdio>
#include<cstring>
using namespace std;
long long d[30005];
int val[12]={10000,5000,2000,1000,500,200,100,50,20,10,5},a;
int main()
{
    double fm;
    d[0]=1;
    for(int i=0;i<11;++i)
        for(int j=val[i];j<=30000;j+=5)
            d[j]+=d[j-val[i]];
    while (scanf("%lf",&fm),a=int(fm*100+0.5))
        printf("%6.2lf%17lld\n",fm,d[a]);
    return 0;
}
```

#

****

New Zealand currency consists of $100, $50, $20, $10, and $5 notes and $2, $1, 50c, 20c, 10c and 5c coins. Write a program that will determine, for any given amount, in how many ways that amount may be made up. Changing the order of listing does not increase the count. Thus 20c may be made up in 4 ways: 1 ![tex2html_wrap_inline25](../images/dge.org-external-1-147img1.gif.png) 20c, 2 ![tex2html_wrap_inline25](../images/dge.org-external-1-147img1.gif.png) 10c, 10c+2 ![tex2html_wrap_inline25](../images/dge.org-external-1-147img1.gif.png) 5c, and 4 ![tex2html_wrap_inline25](../images/dge.org-external-1-147img1.gif.png) 5c.

##

Input will consist of a series of real numbers no greater than $300.00 each on a separate line. Each amount will be valid, that is will be a multiple of 5c. The file will be terminated by a line containing zero (0.00).

##

Output will consist of a line for each of the amounts in the input, each line consisting of the amount of money (with two decimal places and right justified in a field of width 6), followed by the number of ways in which that amount may be made up, right justified in a field of width 17.

##

0.20 2.00 0.00

##

0.20 4 2.00 293

﻿﻿