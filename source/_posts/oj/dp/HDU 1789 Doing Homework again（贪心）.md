---
title: HDU 1789 Doing Homework again（贪心）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:51:47
---
题意 某大参加ACM竞赛回来落下很多作业 每个作业都有最后期限 没在最后期限之内做完期末就要扣掉对应的分 求最少扣多少分

把所有作业按扣分大小从大到小排序 然后就贪阿 能完成前面的就完成前面的 实在不能的就扣分吧~

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 1005;
int dli[N], red[N], k[N], cas, ans, n;
bool vis[N];

bool cmp (int i, int j)
{
    return red[i] > red[j];
}

int main()
{
    scanf ("%d", &cas);
    while (cas--)
    {
        ans = 0;
        memset (vis, 0, sizeof (vis));
        scanf ("%d", &n);
        for (int i = 1; i <= n; ++i)
            scanf ("%d", &dli[i]), k[i] = i;
        for (int j = 1; j <= n; ++j)
            scanf ("%d", &red[j]);
            
        sort (k + 1, k + n + 1, cmp);
        for (int i = 1, j; i <= n; ++i)
        {
            for (j = dli[k[i]]; j >= 1; --j)
                if (!vis[j])
                {
                    vis[j] = 1;
                    break;
                }
            if (j == 0) ans += red[k[i]];
        }
        printf ("%d\n", ans);
    }
    return 0;
}<span style="font-family:Comic Sans MS;">
</span>
```

# Doing Homework again

Problem Description

Ignatius has just come back school from the 30th ACM/ICPC. Now he has a lot of homework to do. Every teacher gives him a deadline of handing in the homework. If Ignatius hands in the homework after the deadline, the teacher will reduce his score of the final test. And now we assume that doing everyone homework always takes one day. So Ignatius wants you to help him to arrange the order of doing homework to minimize the reduced score.
Input

The input contains several test cases. The first line of the input is a single integer T that is the number of test cases. T test cases follow.
Each test case start with a positive integer N(1<=N<=1000) which indicate the number of homework.. Then 2 lines follow. The first line contains N integers that indicate the deadlines of the subjects, and the next line contains N integers that indicate the reduced scores.
Output

For each test case, you should output the smallest total reduced score, one line per test case.
Sample Input

3 3 3 3 3 10 5 1 3 1 3 1 6 2 3 7 1 4 6 4 2 4 3 3 2 1 7 6 5 4
Sample Output

0 3 5