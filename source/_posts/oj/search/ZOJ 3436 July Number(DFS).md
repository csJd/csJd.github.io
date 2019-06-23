---
title: ZOJ 3436 July Number(DFS)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Search

date: 2015-07-26 17:08:26
---
题意 把一个数替换为这个数相邻数字差组成的数 知道这个数只剩一位数 若最后的一位数是7 则称原来的数为 July Number 给你一个区间 求这个区间中July Number的个数

从7开始DFS 位数多的数总能由位数小的数推出

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 1e6;
int july[N], n;
set<int> ans;

int pw[] = {1, 10, 100, 1000, 10000, 100000, 1000000, 10000000, 100000000};
//剩余长度, 当前位要填的数，通过什么数来搜索, 路径
void dfs(int len, int d, int bas, int cur)
{
    if(d < 0 || d > 9) return;  //要填的数不合法
    if(!len)
    {
        july[n++] = cur * 10 + d;
        return;
    }
    int k = pw[len - 1];
    dfs(len - 1, d - bas / k, bas % k, cur * 10 + d);
    dfs(len - 1, d + bas / k, bas % k, cur * 10 + d);
}

int main()
{
    ans.insert(7);
    set<int>::iterator it;
    for(int l = 2; l < 10; ++l)
    {
        n = 0;
        for(it = ans.begin(); it != ans.end(); ++it)
            for(int i = 1; i < 10; ++i)
                dfs(l - 1, i, *it, 0);
        for(int i = 0; i < n; ++i) ans.insert(july[i]);
        //printf("%d\n", ans.size());
    }

    int a, b = n = 0;
    for(it = ans.begin(); it != ans.end(); ++it) 
        july[n++] = *it;
    while(~scanf("%d%d", &a, &b))
        printf("%d\n", upper_bound(july, july + n, b) - lower_bound(july, july + n, a));

    return 0;
}
```
  July Number    Time Limit:2 Seconds Memory Limit:65536 KB

The digital difference of a positive number is constituted by the difference between each two neighboring digits (with the leading zeros omitted). For example the digital difference of 1135 is 022 = 22. The repeated digital difference, or differential root, can be obtained by caculating the digital difference until a single-digit number is reached. A number whose differential root is 7 is also called July Number. Your job is to tell how many July Numbers are there lying in the given interval [*a*, *b*].

#### Input

There are multiple cases. Each case contains two integers *a* and *b*. 1 ≤ *a* ≤ *b* ≤ 109.

#### Output

One integer *k*, the number of July Numbers.

#### Sample Input

1 10

#### Sample Output

1
Author: HE, Ningxu
Contest: ZOJ Monthly, November 2010