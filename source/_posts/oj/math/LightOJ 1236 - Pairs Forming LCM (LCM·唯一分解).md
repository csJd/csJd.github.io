---
title: LightOJ 1236 - Pairs Forming LCM (LCM·唯一分解)
author: Deng
tags: 
       - Algorithm

category: 
       - Mathematics
       - OJ

date: 2015-08-07 09:27:32
---
题意 给你一个数n 求满足lcm(a, b) == n， a <= b 的 (a,b) 的个数

容易知道 n 是a, b的所有素因子取在a, b中较大指数的积

先将n分解为素数指数积的形式 n = π(pi^ei) 那么对于每个素因子pi pi在a,b中的指数ai, bi 至少有一个等于pi， 另一个小于等于pi

先不考虑a, b的大小 对于每个素因子pi

1. 在a中的指数 ai == ei 那么 pi 在 b 中的指数可取 [0, ei] 中的所有数 有 ei + 1 种情况

2. 在a中的指数 ai < ei 即 ai 在 [0, ei) 中 那么 pi 在 b 中的指数只能取 ei 有 ei 种情况

那么对与每个素因子都有 2/*ei + 1种情况 也就是满足条件的 (a, b) 有 π(2/*ei + 1)个 考虑大小时 除了 (n, n) 所有的情况都出现了两次 那么满足a<=b的有**(****π(2/*ei + 1)) / 2 + 1** 个

```js 
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e7 + 5;
int pme[N / 10], m;
bool vis[N];

void initPrime() //线性筛
{
    m = 0;
    for(int i = 2; i < N; ++i)
    {
        if(!vis[i]) pme[m++] = i;
        for(int j = 0; j < m && pme[j] * i < N; ++j)
        {
            vis[pme[j] * i] = 1;
            if(i % pme[j] == 0) break;
        }
    }
}

int main()
{
    initPrime();
    ll n, ans, c;
    int T, cas = 0;
    scanf("%d", &T);
    while(T--)
    {
        scanf("%lld", &n);
        ans = 1;
        for(int i = 0; i < m; ++i)
        {
            if(ll(pme[i])*pme[i] > n) break;
            c = 0;
            while( n % pme[i] == 0)
            {
                n /= pme[i];
                ++c;
            }
            if(c) ans *= c * 2 + 1;
        }
        if(n > 1) ans *= 3;

        printf("Case %d: %lld\n", ++cas, ans / 2 + 1);
    }

    return 0;
}
```

1236 - Pairs Forming LCM
Find the result of the following code:
long long pairsFormLCM( int n ) {
long long res = 0;
for( int i = 1; i <= n; i++ )
for( int j = i; j <= n; j++ )
if( lcm(i, j) ==n ) res++; *// lcm means least common multiple*
return res;
}

A straight forward implementation of the code may time out. If you analyze the code, you will find that the code actually counts the number of pairs **(i, j)** for which **lcm(i, j) = n** and **(i ≤ j)**.

# Input

Input starts with an integer **T (≤ 200)**, denoting the number of test cases.

Each case starts with a line containing an integer **n (1 ≤ n ≤ 1014)**.

# Output

For each case, print the case number and the value returned by the function **'pairsFormLCM(n)'**.
# Sample Input

 
# Output for Sample Input 15

2

3

4

6

8

10

12

15

18

20

21

24

25

27

29
 
Case 1: 2

Case 2: 2

Case 3: 3

Case 4: 5

Case 5: 4

Case 6: 5

Case 7: 8

Case 8: 5

Case 9: 8

Case 10: 8

Case 11: 5

Case 12: 11

Case 13: 3

Case 14: 4

Case 15: 2