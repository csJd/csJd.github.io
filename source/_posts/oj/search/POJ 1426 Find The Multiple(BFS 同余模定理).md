---
title: POJ 1426 Find The Multiple(BFS 同余模定理)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Search

date: 2015-04-01 21:34:24
---
题意 给你一个数n 输出一个仅由0，1组成的数m使得m是n的倍数

找到一个m 是m%n==0 就行了 初始让m=1 然后bfs扩展m的位数 只有两种情况m = m /* 10 或 m = m/*10 + 1;

同余模定理**(a+b) % c = (a%c + b%c) % c, (a/*b)%c = (a%c /* b%c) % c;**

运用同余模定理 可以只记录余数 这样就可以避免处理大数了 因为余数不会超过n 而n是小于两百的

对于已经得到过的余数 是可以跳过的 可以考虑下为什么

于是只用保存每一位选的是0还是1 只要保证最后余数为0就行了

**1. 选0 m = m/*10 % n;**

**2. 选1 m = (m/*10 + 1) % n;**

```js 
#include <cstdio>
using namespace std;
const int N = 205;
int q[N], p[N], d[N], n;

int bfs()
{
    int le = 0, ri = 0, r = 1, cr;
    int v[N] = {0};
    v[q[ri++] = r % n] = d[0] = 1;
    p[0] = -1;
    while(le < ri)
    {
        cr = q[le];
        if(cr == 0)  return le;
        if(!v[r = cr * 10 % n])
            v[r] = 1, p[ri] = le, d[ri] = 0, q[ri++] = r;
        if(!v[r = (cr * 10 + 1) % n])
            v[r] = 1, p[ri] = le, d[ri] = 1, q[ri++] = r;
        ++le;
    }
    return -1;
}

void print(int k)
{
    if(p[k] >= 0) print(p[k]);
    printf("%d", d[k]);
}

int main()
{
    while(scanf("%d", &n), n)
    {
        print(bfs());
        puts("");
    }
    return 0;
}
```

Find The Multiple

Description
Given a positive integer n, write a program to find out a nonzero multiple m of n whose decimal representation contains only the digits 0 and 1. You may assume that n is not greater than 200 and there is a corresponding m containing no more than 100 decimal digits.

Input

The input file may contain multiple test cases. Each line contains a value of n (1 <= n <= 200). A line containing a zero terminates the input.

Output

For each value of n in the input print a line containing the corresponding value of m. The decimal representation of m must not contain more than 100 digits. If there are multiple solutions for a given value of n, any one of them is acceptable.

Sample Input

2 6 19 0

Sample Output

10 100100100100100100 111111111111111111