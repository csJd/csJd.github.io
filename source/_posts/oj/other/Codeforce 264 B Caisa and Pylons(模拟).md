---
title: Codeforce 264 B Caisa and Pylons(模拟)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Other

date: 2014-08-30 18:36:26
---
﻿﻿

题意 Caisa走台阶 有n个台阶 第i个台阶的高度为h[i] 从第i个台阶包括地面到下一个台阶得到的能量为h[i]-h[i+1] 能量不足以跳到下一个台阶就要补充能量 求Caisa跳完所有台阶最少要补充多少能量

水题 直接模拟

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 100005;
int h[N];
int main()
{
    int n, e, ans;
    scanf("%d",&n);
    for (int i = 1; i <= n; ++i)
        scanf ("%d", &h[i]);
    ans = 0;
    for (int i = e = 0; i < n; ++i)
    {
        if (h[i] + e < h[i + 1])
        {
            int t = (h[i + 1] - e - h[i]);
            h[i] += t;
            ans += t;
        }
        e += (h[i] - h[i + 1]);
    }
    printf ("%d\n", ans);
    return 0;
}
```

Caisa solved the problem with the sugar and now he is on the way back to home.

Caisa is playing a mobile game during his path. There are(*n* + 1)pylons numbered from0to*n*in this game. The pylon with number0has zero height, the pylon with number*i*(*i* > 0)has height*h**i*. The goal of the game is to reach*n*-th pylon, and the only move the player can do is to jump from the current pylon (let's denote its number as*k*) to the next one (its number will be*k* + 1). When the player have made such a move, its energy increases by*h**k* - *h**k* + 1(if this value is negative the player loses energy). The player must have non-negative amount of energy at any moment of the time.

Initially Caisa stand at0pylon and has0energy. The game provides a special opportunity: one can pay a single dollar and increase the height of anyone pylon by one. Caisa may use that opportunity several times, but he doesn't want to spend too much money. What is the minimal amount of money he must paid to reach the goal of the game?
Input

The first line contains integer*n*(1 ≤ *n* ≤ 105). The next line contains*n*integers*h*1,*h*2, ...,*h**n*(1  ≤  *h**i*  ≤  105)representing the heights of the pylons.

Output

Print a single number representing the minimum number of dollars paid by Caisa.
Sample test(s)

input 5 3 4 3 2 4

output 4
input 3 4 4 4

output 4

Note

In the first sample he can pay4dollars and increase the height of pylon with number0by4units. Then he can safely pass to the last pylon.