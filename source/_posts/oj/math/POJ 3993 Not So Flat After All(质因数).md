---
title: POJ 3993 Not So Flat After All(质因数)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Mathematics

date: 2014-09-02 09:51:01
---
Not So Flat After All

Description
Any positive integer v can be written as p1a1/*p2a2/*.../*pnan where pi is a prime number and ai ≥ 0. For example: 24 = 23/*31.
Pick any two prime numbers p1 and p2 where p1 = p2. Imagine a two dimensional plane where the powers of p1 are plotted on the x-axis and the powers of p2 on the y-axis. Now any number that can be written as p1a1/*p2a2 can be plotted on this plane at location (x, y) = (a1, a2). The figure on the right shows few examples where p1 = 3 and p2 = 2.
  ![](../images/es-3993_1-.png)  This idea can be extended for any N-Dimensional space where each of the N axes is assigned a unique prime number. Each N-Dimensional space has a unique set of primes.
We call such set the Space Identification Set or S for short. |S| (the ordinal of S) is N.
Any number that can be expressed as a multiplication of pi ∈ S (each raised to a power (ai ≥ 0) can be plotted in this |S|-Dimensional space. The figure at the bottom illustrates this idea for N = 3 and S = {2, 3, 7}. Needless to say, any number that can be plotted on space A can also be plotted on space B as long as SA ![\subset](../images/ula-tex=%5Csubset.png) SB.
We define the distance between any two points in a given N-Dimensional space to be the sum of units traveled to get from one point to the other while following the grid lines (i.e. movement is always parallel to one of the axes.) For example, in the figure below, the distance between 168 and 882 is 4.
Given two positive integers, write a program that determines the minimum ordinal of a space where both numbers can be plotted in. The program also determines the distance between these two integers in that space.
  ![](../images/es-3993_2-.png)

Input

Your program will be tested on one or more test cases. Each test case is specified on a line with two positive integers (0 < A,B < 1, 000, 000) where A /* B > 1.
The last line is made of two zeros.

Output

For each test case, print the following line:
k. X:D
Where k is the test case number (starting at one,) X is the minimum ordinal needed in a space that both A and B can be plotted in. D is the distance between these two points.

Sample Input

168 882 770 792 0 0

Sample Output

1. 3:4 2. 5:6

题意 给你两个数a,b 求a,b所有的质因数个数 和每个质因数个数的差的绝对值的和 被描述得好复杂 理解了就是个水题；

素数问题就先打个素数表吧 然后能被整除的就是质因数了 然后统计a，b分别能被这个数整除多少次

```js 
#include<cstdio>
#include<cmath>
#include<cstring>
using namespace std;
typedef long long ll;
const int N = 1000001;
int p[N], ans[N], vis[N], a, b;
void initPrime()
{
    int num = 0, m = sqrt (N + 0.5);
    for (int i = 2; i <= m; ++i)
        if (vis[i] == 0)
            for (int j = i * i; j <= N; j += i) vis[j] = 1;
    for (int i = 2; i <= N; ++i)
        if (vis[i] == 0) p[++num] = i;
}

int main()
{
    initPrime();
    int cas=0;
    while (scanf ("%d%d", &a, &b),a)
    {
        int cnt = 0, ans = 0, ca, cb;
        for (int i = 1; a > 1 || b > 1; ++i)
            if (a % p[i] == 0 || b % p[i] == 0)
            {
                ++cnt, ca = cb = 0;
                while (a % p[i] == 0) a /= p[i], ++ca;
                while (b % p[i] == 0) b /= p[i], ++cb;
                ans += (ca > cb ? ca - cb : cb - ca);
            }
        printf ("%d. %d:%d\n", ++cas,cnt, ans);
    }
    return 0;
}
```