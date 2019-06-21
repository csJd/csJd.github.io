---
title: ZOJ 3450 Doraemon's Railgun (DP·分组背包)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2015-07-29 09:31:22
---
题意 多啦A梦有一个超电磁炮 然后要打死n堆敌人 在同一条射线上的敌人只有先打死前面的一堆才能打后面的一堆 给你打死某堆敌人需要的时间和这堆敌人的人数 问你在T0时间内最多打死多少个敌人

分组背包问题 先要把同一条射线上的敌人放到一个分组里后面的敌人的时间和人数都要加上前面所有的 因为只有前面的都打完了才能打后面的 然后每组最多只能选择一个判断共线用向量处理 然后去背包就行了

注意给你的样例可能出现t=0的情况 在分组时需要处理一下 被这里卡了好久

```js 
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <vector>
using namespace std;
typedef long long ll;
const int N = 505;
int vis[N], dp[10086];

struct enemy
{
    ll x, y;
    int t, w;
} e[N];


bool cmp(enemy a, enemy b)  //按与原点的距离排序
{
    return a.x * a.x + a.y * a.y < b.x * b.x + b.y * b.y;
}

bool aLine(int i, int j) //判断是否在同一条射线上·
{
    if(e[i].x * e[j].y == e[i].y * e[j].x)
        return e[i].x * e[j].x >= 0 &&  e[i].y * e[j].y >= 0;
    return 0;
}

int main()
{
    ll x, y, x0, y0;
    int n, t0;
    while(~scanf("%lld%lld%d%d", &x0, &y0, &n, &t0))
    {
        for(int i = 0; i < n; ++i)
        {
            scanf("%lld%lld%d%d", &x, &y, &e[i].t, &e[i].w);
            e[i].x = x - x0;
            e[i].y = y - y0;
        }
        sort(e, e + n, cmp);  //按与原点的距离排序

        vector<enemy> em[N];  //把在一条射线上的放到一个分组里
        memset(vis, 0, sizeof(vis));
        for(int i = 0; i < n; ++i)
        {
            if(vis[i]) continue;  //i已经在别的组里了
            em[i].push_back(e[i]);
            for(int j = i + 1; j < n; ++j)
            {
                if(!aLine(i, j)) continue;
                int k = em[i].size() - 1;
                vis[j] = 1;
                if(e[j].t == 0)  //把耗时为0的敌人放到上一个里
                {
                    em[i][k].w += e[j].w;
                    continue;
                }

                em[i].push_back(e[j]);
                em[i][k + 1].w += em[i][k].w;
                em[i][k + 1].t += em[i][k].t;
            }
        }

        memset(dp, 0, sizeof(dp)); //分组背包
        for(int i = 0; i < n; ++i)
        {
            int k = em[i].size();
            for(int v = t0; v >= 0; --v)
                for(int j = 0; j < k && em[i][j].t <= v; ++j)
                    dp[v] = max(dp[v], dp[v - em[i][j].t] + em[i][j].w);
        }
        printf("%d\n", dp[t0]);
    }

    return 0;
}
```
  Doraemon's Railgun    Time Limit:2 Seconds Memory Limit:65536 KB    ![toaru-dorano-railgun](../images/cn-onlinejudge-showImage.do-name=toaru-dorano-railgun-.png)

Doraemon's city is being attacked again. This time Doraemon has built a powerful railgun in the city. So he will use it to attack enemy outside the city.

There are *N* groups of enemy. Now each group is staying outside of the city. Group *i* is located at different (*X*i, *Y*i) and contains *W*i soldiers. After T0 days, all the enemy will begin to attack the city. Before it, the railgun can fire artillery shells to them.

The railgun is located at (X0, Y0), which can fire one group at one time, The artillery shell will fly straightly to the enemy. But in case there are several groups in a straight line, the railgun can only eliminate the nearest one first if Doraemon wants to attack further one. It took *T*i days to eliminate group *i*. Now please calculate the maximum number of soldiers it can eliminate.

#### Input

There are multiple cases. At the first line of each case, there will be four integers, X0, Y0, *N*, T0 (-1000000000 ≤ X0, Y0 ≤ 1000000000; 1 ≤ *N* ≤ 500; 1 ≤ T0 ≤ 10000). Next *N* lines follow, each line contains four integers, *X*i, *Y*i, *T*i, *W*i (-1000000000 ≤ *X*i, *Y*i ≤ 1000000000; 0 ≤ *T*i, *W*i ≤ 10000).

#### Output

For each case, output one integer, which is the maximum number of soldiers the railgun can eliminate.

#### Sample Input

0 0 5 10 0 5 2 3 0 10 2 8 3 2 4 6 6 7 3 9 4 4 10 2

#### Sample Output

20