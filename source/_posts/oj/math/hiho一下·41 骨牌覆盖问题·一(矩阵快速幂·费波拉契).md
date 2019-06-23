---
title: hiho一下·41 骨牌覆盖问题·一(矩阵快速幂·费波拉契)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Mathematics

date: 2015-04-13 17:09:52
---
题意 求费波拉契数列第N项 1≤N≤100,000,000

通过矩阵的幂 可以把一维递推的时间复杂度减小到O(logN) 主要就是快速幂的思想

对于m^n 若**n=2^a1+2^a2+...+2^ak**那么**m^n = m^(2^a1) /*****m^(2^a2) /* ... /*****m^(2^ak)**那么只用看n转换为二进制后哪些位为1就可以快速求出m^n了

```js 
#include <bits/stdc++.h>  
using namespace std;  
typedef long long LL;  
const int N = 2;  
const LL MOD = 19999997;  
  
//将矩阵a*b的结果放入c  
void matMul(LL a[][N], LL b[][N], LL c[][N])  
{  
    LL ret[N][N] = {0};  
    for(int i = 0; i < N; ++i)  
        for(int j = 0; j < N; ++j)  
            for(int k = 0; k < N; ++k)  
                ret[i][j] = (ret[i][j] + a[i][k] * b[k][j]) % MOD;  
    memcpy(c, ret, sizeof(ret));  
}  
  
//将a^n放入a  
void matPow(LL a[][N], int n)  
{  
    LL ret[N][N] = {0};  
    for(int i = 0; i < N; ++i) 
        ret[i][i] = 1;  
    while(n)  
    {  
        if(n & 1) matMul(ret, a, ret);  
        matMul(a, a, a);  
        n >>= 1;  
    }  
    memcpy(a,ret,sizeof(ret));
}  
  
int main()  
{  
    int n;  
    while(~scanf("%d", &n))  
    {  
        LL a[N][N] = {0, 1, 1, 1};  
        matPow(a, n);  
        printf("%lld\n", a[1][1] % MOD);  
    }  
    return 0;  
}
```

时间限制: 10000ms

单点时限: 1000ms
内存限制: 256MB

#### 描述

骨牌，一种古老的玩具。今天我们要研究的是骨牌的覆盖问题：
我们有一个2xN的长条形棋盘，然后用1x2的骨牌去覆盖整个棋盘。对于这个棋盘，一共有多少种不同的覆盖方法呢？
举个例子，对于长度为1到3的棋盘，我们有下面几种覆盖方式：

![](../images/der.com-problem_images-20150411-1428731713269-.png "week41_1.PNG")

[提示：骨牌覆盖](http://hihocoder.com/contest/hiho41/problem/1#)

[提示：如何快速计算结果](http://hihocoder.com/contest/hiho41/problem/1#)

#### 输入

第1行：1个整数N。表示棋盘长度。1≤N≤100,000,000

#### 输出

第1行：1个整数，表示覆盖方案数 MOD 19999997
样例输入 62247088 样例输出 17748018