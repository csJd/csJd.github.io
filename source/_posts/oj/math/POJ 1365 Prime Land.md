---
title: POJ 1365 Prime Land
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Mathematics

date: 2014-09-02 09:50:27
---
觉得这题最难的就是理解题意了 应该怪我英语太差了

题意 一个大于1的数可以 表示成p1^e1/*p2^e2/*p3^e3/*p4^e4/*p5^e5/*.../*pn^en的形式 其中p是素数 且p是递减的 p< 32767

输入给你这个数的这种表达形式的所有p和e 第奇数个数为pi第偶数个数为ei 每行的所有数据表示一个数 你要求出这个数x然后输出x-1的这种表达形式；

理解了题意这个题就简单了 直接暴力可能会超时 所以还是打了个素数表 还要注意pow的前一个参数要乘以1.0 不然会精度丢失

```js 
#include<cstdio>
#include<cstring>
#include<cmath>
using namespace std;
const int N = 33000;
int x, a, b , m , last , ans[N], vis[N], prime[N];

void initPrime()
{
    int num = 0, m = sqrt (N + 0.5);
    for (int i = 2; i <= m; ++i)
    {
        if (vis[i] == 0)
            for (int j = i * i; j < N; j += i)
                vis[j] = 1;
    }
    for (int i = 2, t = 0; i < N; ++i)
        if (!vis[i]) prime[++num] = i;
}

int main()
{
    initPrime(); char c;
    while (scanf ("%d", &a), a)
    {
        scanf ("%d%c", &b, &c);
        x = pow (a*1.0, b);
        while (c == ' ')
        {
            scanf ("%d %d%c", &a, &b, &c);
            x = x * pow (a*1.0, b);
        }
        --x; last = 0;
        memset(ans,0,sizeof(ans));

        for (int i = 1; x > 1; m=i,++i)
            if (x % prime[i] == 0)
            {
                if (!last) last = i;
                while (x % prime[i] == 0)
                {
                    ++ans[i];
                    x /= prime[i];
                }
            }
        for (int i = m; i >= last; --i)
            if (ans[i]) printf (i == last ? "%d %d\n" : "%d %d ", prime[i], ans[i]);
    }
    return 0;
}
```
Prime Land

Description
Everybody in the Prime Land is using a prime base number system. In this system, each positive integer x is represented as follows: Let {pi}i=0,1,2,... denote the increasing sequence of all prime numbers. We know that x > 1 can be represented in only one way in the form of product of powers of prime factors. This implies that there is an integer kx and uniquely determined integers ekx, ekx-1, ..., e1, e0, (ekx > 0), that ![](../images/es-1365_1.jpg.png) The sequence
(ekx, ekx-1, ... ,e1, e0)
is considered to be the representation of x in prime base number system.
It is really true that all numerical calculations in prime base number system can seem to us a little bit unusual, or even hard. In fact, the children in Prime Land learn to add to subtract numbers several years. On the other hand, multiplication and division is very simple.
Recently, somebody has returned from a holiday in the Computer Land where small smart things called computers have been used. It has turned out that they could be used to make addition and subtraction in prime base number system much easier. It has been decided to make an experiment and let a computer to do the operation ``minus one''.
Help people in the Prime Land and write a corresponding program.
For practical reasons we will write here the prime base representation as a sequence of such pi and ei from the prime base representation above for which ei > 0. We will keep decreasing order with regard to pi.

Input

The input consists of lines (at least one) each of which except the last contains prime base representation of just one positive integer greater than 2 and less or equal 32767. All numbers in the line are separated by one space. The last line contains number 0.

Output

The output contains one line for each but the last line of the input. If x is a positive integer contained in a line of the input, the line in the output will contain x - 1 in prime base representation. All numbers in the line are separated by one space. There is no line in the output corresponding to the last ``null'' line of the input.

Sample Input

17 1 5 1 2 1 509 1 59 1 0

Sample Output

2 4 3 2 13 1 11 1 7 1 5 1 3 1 2 1

Prime Land

**Time Limit:** 1000MS  **Memory Limit:** 10000K **Total Submissions:** 2972  **Accepted:** 1362

Description
Everybody in the Prime Land is using a prime base number system. In this system, each positive integer x is represented as follows: Let {pi}i=0,1,2,... denote the increasing sequence of all prime numbers. We know that x > 1 can be represented in only one way in the form of product of powers of prime factors. This implies that there is an integer kx and uniquely determined integers ekx, ekx-1, ..., e1, e0, (ekx > 0), that ![](../images/es-1365_1.jpg.png) The sequence
(ekx, ekx-1, ... ,e1, e0)
is considered to be the representation of x in prime base number system.
It is really true that all numerical calculations in prime base number system can seem to us a little bit unusual, or even hard. In fact, the children in Prime Land learn to add to subtract numbers several years. On the other hand, multiplication and division is very simple.
Recently, somebody has returned from a holiday in the Computer Land where small smart things called computers have been used. It has turned out that they could be used to make addition and subtraction in prime base number system much easier. It has been decided to make an experiment and let a computer to do the operation ``minus one''.
Help people in the Prime Land and write a corresponding program.
For practical reasons we will write here the prime base representation as a sequence of such pi and ei from the prime base representation above for which ei > 0. We will keep decreasing order with regard to pi.

Input

The input consists of lines (at least one) each of which except the last contains prime base representation of just one positive integer greater than 2 and less or equal 32767. All numbers in the line are separated by one space. The last line contains number 0.

Output

The output contains one line for each but the last line of the input. If x is a positive integer contained in a line of the input, the line in the output will contain x - 1 in prime base representation. All numbers in the line are separated by one space. There is no line in the output corresponding to the last ``null'' line of the input.

Sample Input

17 1 5 1 2 1 509 1 59 1 0

Sample Output

2 4 3 2 13 1 11 1 7 1 5 1 3 1 2 1