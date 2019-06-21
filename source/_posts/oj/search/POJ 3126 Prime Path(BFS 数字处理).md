---
title: POJ 3126 Prime Path(BFS 数字处理)
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2015-04-02 07:48:30
---
题意 给你两个4位素数a, b 你每次可以改变a的一位数但要求改变后仍为素数 求a至少改变多少次才能变成b

基础的bfs 注意数的处理就行了 出队一个数 然后入队所有可以由这个素数经过一次改变而来的素数 知道得到b

```js 
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 10000;
int p[N], v[N], d[N], q[N], a, b;

void initPrime()
{
    memset(v, 0 , sizeof(v));
    for(int i = 2; i * i < N; ++i)
        if(!v[i]) for(int j = i; i * j < N; ++j)  v[i * j] = 1;
    for(int i = 2; i < N ; ++i) p[i] = !v[i];
}

int bfs()
{
    int c, t, le = 0, ri = 0;
    memset(v, 0, sizeof(v));
    q[ri++] = a, v[a] = 1, d[a] = 0;
    while(le < ri)
    {
        c = q[le++];
        if( c == b) return d[c];
        for(int i = 1; i < N; i *= 10)
        {
            for(int j = 0; j < 10; ++j)   //把c第i数量级的数改为j
            {
                if(i == 1000 && j == 0) continue;
                t = c / (i * 10) * i * 10 + i * j + c % i;
                if(p[t] && !v[t])
                    v[t] = 1, d[t] = d[c] + 1, q[ri++] = t;
            }
        }
    }
    return -1;
}

int main()
{
    int cas;
    scanf("%d", &cas);
    initPrime();
    while(cas--)
    {
        scanf("%d%d", &a, &b);
        if((a = bfs()) != -1) printf("%d\n", a);
        else puts("Impossible");
    }
    return 0;
}
```

Prime Path

Description
![](../images/es-3126_1.jpg.png) The ministers of the cabinet were quite upset by the message from the Chief of Security stating that they would all have to change the four-digit room numbers on their offices.
— It is a matter of security to change such things every now and then, to keep the enemy in the dark.
— But look, I have chosen my number 1033 for good reasons. I am the Prime minister, you know!
— I know, so therefore your new number 8179 is also a prime. You will just have to paste four new digits over the four old ones on your office door.
— No, it’s not that simple. Suppose that I change the first digit to an 8, then the number will read 8033 which is not a prime!
— I see, being the prime minister you cannot stand having a non-prime number on your door even for a few seconds.
— Correct! So I must invent a scheme for going from 1033 to 8179 by a path of prime numbers where only one digit is changed from one prime to the next prime.
Now, the minister of finance, who had been eavesdropping, intervened.
— No unnecessary expenditure, please! I happen to know that the price of a digit is one pound.
— Hmm, in that case I need a computer program to minimize the cost. You don't know some very cheap software gurus, do you?
— In fact, I do. You see, there is this programming contest going on... Help the prime minister to find the cheapest prime path between any two given four-digit primes! The first digit must be nonzero, of course. Here is a solution in the case above.
 1033
1733
3733
3739
3779
8779
8179 The cost of this solution is 6 pounds. Note that the digit 1 which got pasted over in step 2 can not be reused in the last step – a new 1 must be purchased.

Input

One line with a positive number: the number of test cases (at most 100). Then for each test case, one line with two numbers separated by a blank. Both numbers are four-digit primes (without leading zeros).

Output

One line for each case, either with a number stating the minimal cost or containing the word Impossible.

Sample Input

3 1033 8179 1373 8017 1033 1033

Sample Output

6 7 0