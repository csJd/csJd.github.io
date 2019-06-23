---
title: UVa 11426 GCD - Extreme (II) (欧拉函数应用·O(N*logN))
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Mathematics

date: 2015-08-09 17:52:29
---
题意 令**G(n) = sum{gcd(i, j) | 0 < i < n, i < j <= n}**给你一个n 输出G(n)

令**F(n) = sum{gcd(i, n) | 0 < i < n}**

那么有递推式**G(n) = G(n-1) + F(n) , G(0) = 0**

也就是说只用求出F(n) 就能递推求出 G(n）了 而求F(n)就比较容易了

对于i 设**x < i , gcd(x,i) = 1**即x, n 互质

则**gcd(2/*x, 2/*i) = 2, gcd(3/*x, 3/*i) = 3, ..., gcd(k/*x, k/*i) = k**

这样的x的个数就是i的欧拉函数值 那么我们求出i的欧拉函数后 所有的F(k/*i) 都加上k 这样筛一下就能求出一定范围内所有的F函数值了 然后累加上去就是G的函数值了

```js 
#include<cstdio>
const int N = 4000005;
typedef long long ll;
int phi[N];
ll g[N];

void init() //O(N*logN) 筛欧拉函数
{
    for(int i = 2; i < N; ++i) phi[i] = i;
    for(int i = 2; i < N; i ++)
    {
        if(phi[i] == i) //i是素数
        {
            for(int j = i; j < N; j += i)
                phi[j] = phi[j] / i * (i - 1);
            //需要从j中删除一个素因子i  因为phi(p^k) = (p-1) * p^(k-1)
        }
        for(int j = 1; j * i < N; j ++)
            g[j * i] += j * phi[i];
        //phi[i]是小于i且与i互质的数的个数 j * i < N 时 j * x 肯定也小于n
    }
    for(int i = 1; i < N; i ++) g[i] += g[i - 1];
}

int main()
{
    init();
    int n;
    while(scanf("%d", &n), n)
        printf("%lld\n", g[n]);
    return 0;
}
```