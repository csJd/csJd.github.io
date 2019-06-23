---
title: UVa 10465 Homer Simpson(DP 完全背包)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-23 09:52:12
---
﻿﻿

题意 霍默辛普森吃汉堡 有两种汉堡 一中吃一个需要m分钟 另一种吃一个需要n分钟 他共有t分钟时间要我们输出他在尽量用掉所有时间的前提下最多能吃多少个汉堡 如果时间无法用完 输出他吃的汉堡数和剩余喝酒的时间

很明显的完全背包问题 求两次 一次对个数 一次对时间就行了 时间用不完的情况下就输出时间的

d1为个数的 d2为时间的 dt保存时间

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
#define maxn 10005
using namespace std;
int w[2],t,d1[maxn],d2[maxn],dt[maxn];
int main()
{
    while (scanf("%d%d%d",&w[0],&w[1],&t)!=EOF)
    {
        memset(d1,0x8f,sizeof(d1));
        memset(dt,0,sizeof(dt));
        memset(d2,0,sizeof(d2));
        d1[0]=0;
        for(int i=0; i<2; ++i)
            for(int j=w[i]; j<=t; ++j)
            {
                d1[j]=max(d1[j],d1[j-w[i]]+1);
                if((dt[j]<dt[j-w[i]]+w[i])||((dt[j]==dt[j-w[i]]+w[i])&&(d2[j]<d2[j-w[i]]+1)))
                {
                    dt[j]=dt[j-w[i]]+w[i];
                    d2[j]=d2[j-w[i]]+1;
                }
            }
        if(dt[t]==t)
            printf("%d\n",d1[t]);
        else
            printf("%d %d\n",d2[t],t-dt[t]);
    }
    return 0;
}
```
 Homer Simpson   ![](../images/dge.org-external-104-p10465.jpg.png) Homer Simpson, a very smart guy, likes eating Krusty-burgers. It takes Homer m minutes to eat a Krusty- burger. However, there�s a new type of burger in Apu�s Kwik-e-Mart. Homer likes those too. It takes him n minutes to eat one of these burgers. Given t minutes, you have to find out the maximum number of burgers Homer can eat without wasting any time. If he must waste time, he can have beer.

Input
Input consists of several test cases. Each test case consists of three integers m, n, t (0 < m,n,t < 10000). Input is terminated by EOF.

Output
For each test case, print in a single line the maximum number of burgers Homer can eat without having beer. If homer must have beer, then also print the time he gets for drinking, separated by a single space. It is preferable that Homer drinks as little beer as possible.
Sample Input
3 5 54
3 5 55

Sample Output
18
17