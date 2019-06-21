---
title: HDU 1024 Max Sum Plus Plus (DP·滚动数组)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2015-08-27 10:02:55
---
题意 从n个数的数组中选出不相交的m段 求被选数的和的最大值

Max Sum 的升级版 不只是要选一段连续的了 而是选m段 思想还是类似 依旧dp

状态和状态转移方程不是很难想 在 Max Sum 这个问题中 dp[i] 表示的是以第i个数结尾的一段的 Max Sum 由于这里还有一个多少段的状态 于是这里令 dp[i][j] 表示在前 i 个数中选取 j 组 且第 i 个数在最后一组中的 Max Sum Plus Plus

那么现在对于第i个数有两种决策

1, 第 i 个数和第 i-1 个数连接成一段**dp[i][j] = dp[i-1][j] + a[i]**

2, 第 i 个数自己单独做一段 那么前面就需要有 j-1 段**dp[i][j] = max{dp[k][j-1] | j - 1<= k < i} + a[i]**

那么也就有了状态转移方程**dp[i][j] = max(dp[i-1][j], max{dp[k][j-1] | j - 1<= k < i}) + a[i];**

有了方程就可以写了呀 我们先不考虑复杂度问题 假设时间空间都是无限的 恩 无限的

那么很容易有下面的代码

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;

const int N = 10005, INF = 1e9;
int a[N], dp[N][N];

int main()
{
    int n, m;
    while(~scanf("%d%d", &m, &n))
    {
        for(int i = 1; i <= n; i++)
            scanf("%d", &a[i]);
        memset(dp, 0, sizeof dp);

        //dp[i][j] 表示前i个数第i个数被选 选j组的Max Sum Plus Plus
        //dp[i][j] = max(dp[i-1][j], max{dp[k][j-1], j - 1 <= k < i}) + a[i]
        int ans = -INF;
        for(int j = 1; j <= m; j++)
        {
            for(int i = j; i <= n; i++)
            {
                dp[i][j] = i - 1 < j ? -INF : dp[i - 1][j]; //i-1 < j时dp[i - 1][j]是没意义的
                for(int k = j - 1; k < i; ++k)
                    dp[i][j] = max(dp[i][j], dp[k][j - 1]);
                dp[i][j] += a[i];
                if(j == m) ans = max(ans, dp[i][j]);
            }
        }

        printf("%d\n", ans);
    }
    return 0;
}
```
可是并不是无限的呀 上面这个时间复杂度0(n/*n/*m) 空间复杂度O(n/*n) n高达1000000 简直可怕

再来看看转移方程**dp[i][j] = max(dp[i-1][j],max{dp[k][j-1] | j - 1<= k < i}) + a[i];**

发现更新 dp[i][j] 的时候只用到了 dp[.][j] 和 dp[.][j-1] 里面的值 也就是 dp[.][0~j-2] 都已经没用了 那也就不用存这些没用的了 也就是j只用开两维就够了 也就是所谓的滚动数组了 用 dp[.][1] 和 dp[.][0] 来轮换表示 dp[.][j] 和 dp[.][j-1] 这样空间复杂度就降到了O(n) 这是可以接受的 那么有空间优化后的代码

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;

const int N = 1000005, INF = 1e9;
int a[N], dp[N][2];

int main()
{
    int n, m;
    while(~scanf("%d%d", &m, &n))
    {
        for(int i = 1; i <= n; i++)
            scanf("%d", &a[i]);
        memset(dp, 0, sizeof dp);

        //dp[i][j] 表示前i个数第i个数被选 选j组的Max Sum Plus Plus
        //dp[i][j] = max(dp[i-1][j], max{dp[k][j-1], j - 1 <= k < i}) + a[i]
        int ans = -INF, j0 = 0, j1 = 1;
        for(int j = 1; j <= m; j++)
        {
            for(int i = j; i <= n; i++)
            {
                //dp[i][j0]存的是dp[i][j-1]的值 dp[i][j1] 存的是dp[i][j]的值
                dp[i][j1] = i - 1 < j ? -INF : dp[i - 1][j1]; //i-1 < j时dp[i - 1][j]是没意义的
                for(int k = j - 1; k < i; ++k)
                    dp[i][j1] = max(dp[i][j1], dp[k][j0]);
                dp[i][j1] += a[i];
                if(j == m) ans = max(ans, dp[i][j1]);
            }
            swap(j0, j1); //滚动数组交换0, 1  因为这轮的j在下轮就变成j - 1了
        }

        printf("%d\n", ans);
    }
    return 0;
}
```
现在空间问题解决了 那么时间问题呢 来看这段代码

```js 
<span style="white-space:pre">	</span>for(int j = 1; j <= m; j++)
      <span style="white-space:pre">	</span>{
            for(int i = j; i <= n; i++)
            {
                //
                dp[i][j1] = i - 1 < j ? -INF : dp[i - 1][j1]; //
                for(int k = j - 1; k < i; ++k)
                    dp[i][j1] = max(dp[i][j1], dp[k][j0]); //这轮循环只比上轮多了个dp[i-1][j0]！！！
                dp[i][j1] += a[i];
                if(j == m) ans = max(ans, dp[i][j1]);
            }
            swap(j0, j1); //
        }
```
看注释部分！！！ 这轮只比上轮多了一个数 我还循环那么多是什么鬼 把上轮的值 ma 记录下来 这轮只用和 dp[i-1][j0] 比较一下就行了 那么就有了可以ac的代码

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;

const int N = 1000005, INF = 1e9;
int a[N], dp[N][2];

int main()
{
    int n, m;
    while(~scanf("%d%d", &m, &n))
    {
        for(int i = 1; i <= n; i++)
            scanf("%d", &a[i]);
        memset(dp, 0, sizeof dp);

        //dp[i][j] 表示前i个数第i个数被选 选j组的Max Sum Plus Plus
        //dp[i][j] = max(dp[i-1][j], max{dp[k][j-1], j - 1 <= k < i}) + a[i]
        int ans = -INF, j0 = 0, j1 = 1;
        for(int j = 1; j <= m; j++)
        {
            int ma = dp[j - 1][j1] = -INF; //dp[j - 1][j]是没意义的
            for(int i = j; i <= n; i++)
            {
                //dp[i][j0]存的是dp[i][j-1]的值 dp[i][j1]存的是dp[i][j]的值
                ma = max(ma, dp[i - 1][j0]); //ma维护max{dp[k][j-1], j - 1 <= k < i}
                dp[i][j1] = max(dp[i - 1][j1], ma) + a[i];
                if(j == m) ans = max(ans, dp[i][j1]);
            }
            swap(j0, j1); //滚动数组交换0, 1  因为这轮的j在下轮就变成j - 1了
        }

        printf("%d\n", ans);
    }
    return 0;
}
```

# Max Sum Plus Plus

Problem Description

Now I think you have got an AC in Ignatius.L's "Max Sum" problem. To be a brave ACMer, we always challenge ourselves to more difficult problems. Now you are faced with a more difficult problem.
Given a consecutive number sequence S *1*, S *2*, S *3*, S *4* ... S *x*, ... S *n* (1 ≤ x ≤ n ≤ 1,000,000, -32768 ≤ S *x* ≤ 32767). We define a function sum(i, j) = S *i* + ... + S *j* (1 ≤ i ≤ j ≤ n).
Now given an integer m (m > 0), your task is to find m pairs of i and j which make sum(i *1*, j *1*) + sum(i *2*, j *2*) + sum(i *3*, j *3*) + ... + sum(i *m*, j *m*) maximal (i *x* ≤ i *y* ≤ j *x* or i *x* ≤ j *y* ≤ j *x* is not allowed).
But I`m lazy, I don't want to write a special-judge module, so you don't have to output m pairs of i and j, just output the maximal summation of sum(i *x*, j *x*)(1 ≤ x ≤ m) instead. ^_^
Input

Each test case will begin with two integers m and n, followed by n integers S *1*, S *2*, S *3* ... S *n*.
Process to the end of file.
Output

Output the maximal summation described above in one line.
Sample Input

1 3 1 2 3 2 6 -1 4 -2 3 -2 3
Sample Output

6 8

*Hint* Huge input, scanf and dynamic programming is recommended.
Author

JGShining（极光炫影）