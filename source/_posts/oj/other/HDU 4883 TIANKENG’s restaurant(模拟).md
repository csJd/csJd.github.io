---
title: HDU 4883 TIANKENG’s restaurant(模拟)
author: Deng
tags: 
       - Algorithm

category: 
       - Other
       - OJ

date: 2014-08-27 10:37:07
---
题意 天坑开了个饭店 他知道所有客人的进来时间和出去的时间 求天坑至少准备多少张凳子

以分钟为单位 直接模拟就行了 peo[i]代表第i分钟的人 第i组人第si分钟进来 第so分钟出去 那么j从si到so peo[j]都加上这组的人数 最后看第几分钟人最多就是答案了

```js 
#include<cstdio>  
#include<cstring>  
using namespace std;  
const int N = 1441;  
int hi, ho, mi, mo, si, so, t, n, ans, p, peo[N];  
  
  
int main()  
{  
    scanf ("%d", &t);  
    while (t--)  
    {  
        scanf ("%d", &n);  
        memset (peo, 0, sizeof (peo));  
        for (int i = 1; i <= n; ++i)  
        {  
            scanf ("%d %d:%d %d:%d", &p, &hi, &mi, &ho, &mo);  
            si = hi * 60 + mi;  
            so = ho * 60 + mo;  
            for (int j = si; j < so; ++j)  
                peo[j] += p;  
        }  
        for (int i = ans = 1; i < N; ++i)  
            if (peo[i] > ans) ans = peo[i];  
        printf ("%d\n", ans);  
    }  
    return 0;  
}
```

# TIANKENG’s restaurant

Problem Description

TIANKENG manages a restaurant after graduating from ZCMU, and tens of thousands of customers come to have meal because of its delicious dishes. Today n groups of customers come to enjoy their meal, and there are Xi persons in the ith group in sum. Assuming that each customer can own only one chair. Now we know the arriving time STi and departure time EDi of each group. Could you help TIANKENG calculate the minimum chairs he needs to prepare so that every customer can take a seat when arriving the restaurant?
Input

The first line contains a positive integer T(T<=100), standing for T test cases in all. Each cases has a positive integer n(1<=n<=10000), which means n groups of customer. Then following n lines, each line there is a positive integer Xi(1<=Xi<=100), referring to the sum of the number of the ith group people, and the arriving time STi and departure time Edi(the time format is hh:mm, 0<=hh<24, 0<=mm<60), Given that the arriving time must be earlier than the departure time. Pay attention that when a group of people arrive at the restaurant as soon as a group of people leaves from the restaurant, then the arriving group can be arranged to take their seats if the seats are enough.
Output

For each test case, output the minimum number of chair that TIANKENG needs to prepare.
Sample Input

2 2 6 08:00 09:00 5 08:59 09:59 2 6 08:00 09:00 5 09:00 10:00
Sample Output

11 6

﻿﻿