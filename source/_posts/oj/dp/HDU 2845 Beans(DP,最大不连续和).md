---
title: HDU 2845 Beans(DP,最大不连续和)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-09-02 09:51:28
---
题意 吃豆子游戏 当你吃了一个格子的豆子 该格子左右两个和上下两行就不能吃了 输入每个格子的豆子数 求你最多能吃多少颗豆子

可以先求出每行你最多可以吃多少颗豆子 然后每行就压缩成只有一个格子了 里面的豆子数就是那一行最多可以吃的豆子数 然后问题就变成求一列最多可以吃多少颗豆子了 和处理每一行一样处理 那么问题就简化成求一行数字的最大不连续和问题了

令d[i]表示某一行前i个豆子的最大和 有两种情况 吃第i个格子中的豆子和不吃第i个格子中的豆子 a[i]为第i个格子中的豆子数

吃 d[i]=d[i-2]+a[i] 不吃 d[i]=d[i-1]

所以有转移方程 d[i]=max(d[i-2]+a[i],d[i-1])

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N=2014;
int d[N],row[N],col[N],mat[N][N],n,m;
int main()
{
    while (~scanf ("%d%d", &n, &m))
    {
        for (int i = 1; i <= n; ++i)
            for (int j = 1; j <= m; ++j)
                scanf ("%d", &mat[i][j]);
        memset (row, 0, sizeof (row));
        for (int i = 1; i <= n; ++i)
        {
            memset (d, 0, sizeof (d));
            d[1] = mat[i][1];
            for (int j = 2; j <= m; ++j)
                d[j] = max (d[j - 1], d[j - 2] + mat[i][j]);
            row[i] = max (row[i], d[m]);
        }
        memset (col, 0, sizeof (col));
        col[1] = row[1];
        for (int i = 2; i <= n; ++i)
            col[i] = max (col[i - 1], col[i - 2] + row[i]);
        printf ("%d\n", col[n]);
    }
    return 0;
}
```

# Beans

Problem Description

Bean-eating is an interesting game, everyone owns an M/*N matrix, which is filled with different qualities beans. Meantime, there is only one bean in any 1/*1 grid. Now you want to eat the beans and collect the qualities, but everyone must obey by the following rules: if you eat the bean at the coordinate(x, y), you can’t eat the beans anyway at the coordinates listed (if exiting): (x, y-1), (x, y+1), and the both rows whose abscissas are x-1 and x+1.
  ![](../images/cn-data-images-convip1-1001-1.JPG.png)  Now, how much qualities can you eat and then get ?
Input

There are a few cases. In each case, there are two integer M (row number) and N (column number). The next M lines each contain N integers, representing the qualities of the beans. We can make sure that the quality of bean isn't beyond 1000, and 1<=M/*N<=200000.
Output

For each case, you just output the MAX qualities you can eat and then get.
Sample Input

4 6 11 0 7 5 13 9 78 4 81 6 22 4 1 40 9 34 16 10 11 22 0 33 39 6
Sample Output

242
其实上面的代码还是有 bug的 题目说的是N/*M<=200000 并没有说N和M的具体范围 这里给出一位数组读入的版本

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 200010;
int d[N], row[N], col[N], mat[N], n, m;
int main()
{
    while (~scanf ("%d%d", &n, &m))
    {
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < m; ++j)
                scanf ("%d", &mat[i * m + j]);

        for (int i = 0; i < n; ++i)
        {
            d[0] = mat[i * m + 0], d[1] = max (mat[i * m + 1], d[0]);
            for (int j = 2; j < m; ++j)
                d[j] = max (d[j - 1], d[j - 2] + mat[i * m + j]);
            row[i] = d[m - 1];
        }

        col[0] = row[0], col[1] = max (row[0], row[1]);
        for (int i = 2; i < n; ++i)
            col[i] = max (col[i - 1], col[i - 2] + row[i]);
        printf ("%d\n", col[n - 1]);
    }
    return 0;
}
```