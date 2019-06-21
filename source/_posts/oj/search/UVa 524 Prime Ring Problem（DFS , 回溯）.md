---
title: UVa 524 Prime Ring Problem（DFS , 回溯）
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2014-11-20 16:42:21
---
题意 把1到n这n个数以1为首位围成一圈 输出所有满足任意相邻两数之和均为素数的所有排列

直接枚举排列看是否符合肯定会超时的 n最大为16 利用回溯法 边生成边判断 就要快很多了

```js 
#include<cstdio>
using namespace std;
const int N = 50;
int p[N], vis[N], a[N], n;

int isPrime(int k)
{
    for(int i = 2; i * i <= k; ++i)
        if(k % i == 0) return 0;
    return 1;
}

void dfs(int cur)
{
    if(cur == n && p[a[n - 1] + 1])
    {
        printf("%d", a[0]);
        for(int i = 1; i < n; ++i)
            printf(" %d", a[i]);
        printf("\n");
    }

    for(int i = 2; cur < n && i <= n; ++i)
    {
        if(!vis[i] && p[a[cur - 1] + i])
        {
            vis[i] = a[cur] = i;
            dfs(cur + 1);
            vis[i] = 0;
        }
    }
}

int main()
{
    int cas = 0;
    a[0] = 1;
    for(int i = 2; i < N; ++i)
        p[i] = isPrime(i);

    while(~scanf("%d", &n))
    {
        if(cas) printf("\n");
        printf("Case %d:\n", ++cas);
        dfs(1);
    }

    return 0;
}
```

#

****

A ring is composed of n (even number) circles as shown in diagram. Put natural numbers ![$1, 2, \dots, n$](../images/dge.org-external-5-524img1.gif.png)into each circle separately, and the sum of numbers in two adjacent circles should be a prime.

![](../images/dge.org-external-5-p524.gif.png)

**Note:** the number of first circle should always be 1.

##

n (0 < n <= 16)

##

The output format is shown as sample below. Each row represents a series of circle numbers in the ring beginning from 1 clockwisely and anticlockwisely. The order of numbers must satisfy the above requirements.

You are to write a program that completes above process.

##

6 8

##

Case 1: 1 4 3 2 5 6 1 6 5 2 3 4 Case 2: 1 2 3 8 5 6 7 4 1 2 5 8 3 4 7 6 1 4 7 6 5 8 3 2 1 6 7 4 3 8 5 2