---
title: HDU 2602 Bone Collector（01背包）
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-09-02 09:51:07
---
# Bone Collector

Problem Description

Many years ago , in Teddy’s hometown there was a man who was called “Bone Collector”. This man like to collect varies of bones , such as dog’s , cow’s , also he went to the grave …
The bone collector had a big bag with a volume of V ,and along his trip of collecting there are a lot of bones , obviously , different bone has different value and different volume, now given the each bone’s value along his trip , can you calculate out the maximum of the total value the bone collector can get ?
  ![](../images/cn-data-images-C154-1003-1.jpg.png)
Input

The first line contain a integer T , the number of cases.
Followed by T cases , each case three lines , the first line contain two integer N , V, (N <= 1000 , V <= 1000 )representing the number of bones and the volume of his bag. And the second line contain N integers representing the value of each bone. The third line contain N integers representing the volume of each bone.
Output

One integer per line representing the maximum of the total value (this number will be less than 231).
Sample Input

1 5 10 1 2 3 4 5 5 4 3 2 1
Sample Output

14

裸的01背包啦啦啦啦

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 1005;
int n, v, d[N], val[N], vol[N], cas;
int main()
{
    scanf ("%d", &cas);
    while (cas--)
    {
        memset (d, 0, sizeof (d));
        scanf ("%d%d", &n, &v);
        for (int i = 1; i <= n; ++i)
            scanf ("%d", &val[i]);
        for (int i = 1; i <= n; ++i)
        {
            scanf ("%d", &vol[i]);
            for (int j = v; j >= vol[i]; --j)
                d[j] = max (d[j], d[j - vol[i]] + val[i]);
        }
        printf ("%d\n", d[v]);
    }
    return 0;
}
```