---
title: ZOJ 3622 Magic Number(数)
author: Deng
tags: 
       - Algorithm

category: 
       - Mathematics
       - OJ

date: 2014-11-18 15:42:36
---
题意 如果一个正整数y满足 将任意正整数x放到y的左边得到的数z满足 z%y==0 那么这个数就是个Magic Number 给你一个范围 求这个范围内Magic Number的个数

令 l表示y的位数 ly=10^l 那么z=x/*ly + y 要z%y==0 容易看出 只需 x/*ly%y==0

又因为x是任意的 所以一个Magic Number必须满足 ly%y==0

y<2^31 所以l最大为10 直接枚举l 找到所有符合的y就行了

当ly%y==0时y>=ly/10&&y<ly即ly是比y多一位数的令t=ly/y 那么肯定有1<t<=10对于每个ly 我们就只用枚举t了

```js 
#include<cstdio>
#include<algorithm>
using namespace std;
const int N = 50;
typedef long long ll;
ll p[N], n, m;

int main()
{
    int cnt = 0, ans;   //ly为10^l
    for(ll ly = 10; ly < 1e11; ly *= 10)
    {
        for(ll t = 10; t > 1; --t)  //若(ly/y==t) 必有1<t<=10
            if(ly % t == 0) p[cnt++] = ly / t;
    }

    while(~scanf("%lld%lld", &n, &m))
    {
        ans = upper_bound(p, p + cnt, m) - lower_bound(p, p + cnt, n);
        printf("%d\n", ans);
    }
    return 0;
}
```
  Magic Number    Time Limit:2 Seconds Memory Limit:32768 KB

A positive number *y* is called magic number if for every positive integer *x* it satisfies that put *y* to the right of *x*, which will form a new integer *z*, *z* mod *y* = 0.

#### Input

The input has multiple cases, each case contains two positve integers *m*, *n*(1 <= *m* <= *n* <= 2^31-1), proceed to the end of file.

#### Output

For each case, output the total number of magic numbers between *m* and *n*(*m*, *n* inclusively).

#### Sample Input

1 1 1 10

#### Sample Output

1 4