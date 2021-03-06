---
title: POJ 3258 River Hopscotch（二分·最小距离最大)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Mathematics

date: 2015-03-09 11:33:17
---
题意 一条河两岸之间有n个石头 求取走m个石头后 使得两个石头间距离的最小值最大

感觉关键是理解题意 简单的二分 二分最小距离 看要取走多少个(cnt)石头 cnt<=m的话 说明最小距离还可以增大 这样就可以二分出答案了

```js 
#include <cstdio>
#include <algorithm>
using namespace std;

const int N = 50005;
int p[N], l, n, m;

bool ok(int k)
{
    int last = 0, cnt = 0;
    for(int i = 1; i < n; ++i)
    {
        if(p[i] - p[last] < k) ++cnt;
        else last = i;
    }
    return cnt <= m;
}

int main()
{
    int le, ri, mid;
    while(~scanf("%d%d%d", &l, &n, &m))
    {
        p[++n] = l;
        for (int i = 1; i < n; ++i)
            scanf("%d", &p[i]);

        sort(p, p + n);
        le = 0, ri = l;
        while(le <= ri)
        {
            mid = (le + ri) >> 1;
            if(ok(mid)) le = mid + 1;
            else ri = mid - 1;
        }
        printf("%d\n", le - 1);
    }
    return 0;
}
```

River Hopscotch

Description
Every year the cows hold an event featuring a peculiar version of hopscotch that involves carefully jumping from rock to rock in a river. The excitement takes place on a long, straight river with a rock at the start and another rock at the end, *L* units away from the start (1 ≤ *L* ≤ 1,000,000,000). Along the river between the starting and ending rocks, *N* (0 ≤*N* ≤ 50,000) more rocks appear, each at an integral distance *Di* from the start (0 < *Di* < *L*).

To play the game, each cow in turn starts at the starting rock and tries to reach the finish at the ending rock, jumping only from rock to rock. Of course, less agile cows never make it to the final rock, ending up instead in the river.

Farmer John is proud of his cows and watches this event each year. But as time goes by, he tires of watching the timid cows of the other farmers limp across the short distances between rocks placed too closely together. He plans to remove several rocks in order to increase the shortest distance a cow will have to jump to reach the end. He knows he cannot remove the starting and ending rocks, but he calculates that he has enough resources to remove up to *M*rocks (0 ≤ *M* ≤ *N*).

FJ wants to know exactly how much he can increase the shortest distance */*before/** he starts removing the rocks. Help Farmer John determine the greatest possible shortest distance a cow has to jump after removing the optimal set of *M* rocks.

Input

Line 1: Three space-separated integers: *L*, *N*, and *M*
Lines 2.. *N*+1: Each line contains a single integer indicating how far some rock is away from the starting rock. No two rocks share the same position.

Output

Line 1: A single integer that is the maximum of the shortest distance a cow has to jump after removing *M* rocks

Sample Input

25 5 2 2 14 11 21 17

Sample Output

4

Hint

Before removing any rocks, the shortest jump was a jump of 2 from 0 (the start) to 2. After removing the rocks at 2 and 14, the shortest required jump is a jump of 4 (from 17 to 21 or from 21 to 25).

Source

[USACO 2006 December Silver](http://poj.org/searchproblem?field=source&key=USACO+2006+December+Silver)