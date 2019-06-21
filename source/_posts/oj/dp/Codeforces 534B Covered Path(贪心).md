---
title: Codeforces 534B Covered Path(贪心)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2015-04-13 17:29:27
---
题意 你在路上走 每秒钟的开始都可以改变自己的速度(改变速度都是瞬间完成的) 知道你开始的速度v1 结束时的速度v2 整个过程所用时间t 以及每秒最多改变的速度d 求这段时间内你最多走了多远

最优的肯定是先把速度从v1升到最大 然后从最大减到v2 使得用的时间不会超多t 因为肯定是足够从v1减为或升到v2的 那么我们只用从两端往中间靠 哪边的速度小 哪边就加上d 知道两边相邻 这样就能保证每次改变的速度都最大 而且最后两端的速度差也不会大于d 也就是最优答案了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 105;
int v[N];
int main()
{
    int v1, v2, t, d, k;
    while(~scanf("%d%d%d%d", &v1, &v2, &t, &d))
    {
        v[0] = v1, v[t - 1] = v2;
        int le = 0, ri = t - 1, ans = 0;
        while(le < ri - 1)
        {
            if(v[le] < v[ri]) v[le + 1] = v[le++] + d;
            else v[ri - 1] = v[ri--] + d;
        }
        for(int i = 0; i < t; ++i) ans += v[i];
        printf("%d\n", ans);
    }
    return 0;
}
//Last modified :   2015-04-13 00:27
```

B. Covered Path

The on-board computer on Polycarp's car measured that the car speed at the beginning of some section of the path equals *v*1 meters per second, and in the end it is *v*2 meters per second. We know that this section of the route took exactly *t* seconds to pass.

Assuming that at each of the seconds the speed is constant, and between seconds the speed can change at most by *d* meters per second in absolute value (i.e., the difference in the speed of any two adjacent seconds does not exceed *d* in absolute value), find the maximum possible length of the path section in meters.
Input

The first line contains two integers *v*1 and *v*2 (1 ≤ *v*1, *v*2 ≤ 100) — the speeds in meters per second at the beginning of the segment and at the end of the segment, respectively.

The second line contains two integers *t* (2 ≤ *t* ≤ 100) — the time when the car moves along the segment in seconds, *d* (0 ≤ *d* ≤ 10) — the maximum value of the speed change between adjacent seconds.

It is guaranteed that there is a way to complete the segment so that:

Output

Print the maximum possible length of the path segment in meters.
Sample test(s)

input 5 6 4 2

output 26
input 10 10 10 0

output 100

Note

In the first sample the sequence of speeds of Polycarpus' car can look as follows: 5, 7, 8, 6. Thus, the total path is 5 + 7 + 8 + 6 = 26meters.

In the second sample, as *d* = 0, the car covers the whole segment at constant speed *v* = 10. In *t* = 10 seconds it covers the distance of 100 meters.