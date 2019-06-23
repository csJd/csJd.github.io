---
title: LightOJ 1215 Finding LCM(数论)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Mathematics

date: 2015-07-12 10:46:06
---
题意 已知LCM(a, b, c) = L 和 a、b、L 求最小的满足等式的c.

把数展开为素因子积的形式后

GCD(a,b)就是a,b的公共素因子取在a、b中的较小指数

LCM(a,b)就是a,b的所有素因子取在a、b中的较大指数

令m = LCM(a,b) 那么问题转化为了求最小的c满足 LCM(m, c) = L

那么最小的c就是L中不在m中的素因子和L中指数大于m中指数的素因子取在L中的指数即积

```js 
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

ll gcd(ll a, ll b)
{
    return b ? gcd(b, a % b) : a;
}

int main()
{
    int T;
    ll a, b, l, c, d, m;
    scanf("%d", &T);
    for(int cas = 1; cas <= T; ++cas)
    {
        printf("Case %d: ", cas);
        scanf("%lld%lld%lld", &a, &b, &l);
        m = a * b / gcd(a, b); //m为a,b的最大公约数

        if(l % m) puts("impossible");
        else
        {
            //要使lcm(c,m) = l  c中至少要有l中不在m中的素因子和l中指数大于m中的素因子取在l中的指数
            c = l / m;   //现在c中包含了l中不在m中的素因子取l中指数 和 l中指数大于m中的素因子取指数差
            //那么现在c还需要乘上c和m的公共素因子取m中的指数
            while((d = gcd(c, m)) != 1) //gcd(c,m) 取c,m公共素因子的小指数积
            {
                c = c * d, m = m / d;
                d = gcd(c, m);
            }
            printf("%lld\n", c);
        }
    }
    return 0;
}
```

1215 - Finding LCM
**LCM** is an abbreviation used for **L**east **C**ommon **M**ultiple in Mathematics. We say **LCM (a, b, c) = L** if and only if **L** is the least integer which is divisible by **a, b**and **c**.

You will be given **a, b** and **L**. You have to find **c** such that **LCM (a, b, c) = L**. If there are several solutions, print the one where **c** is as small as possible. If there is no solution, report so.

# Input

Input starts with an integer **T (≤ 325)**, denoting the number of test cases.

Each case starts with a line containing three integers **a b L (1 ≤ a, b ≤ 106, 1 ≤ L ≤ 1012)**.

# Output

For each case, print the case number and the minimum possible value of **c**. If no solution is found, print **'impossible'**.
# Sample Input

 
# Output for Sample Input 3

3 5 30

209475 6992 77086800

2 6 10
 
Case 1: 2

Case 2: 1

Case 3: impossible