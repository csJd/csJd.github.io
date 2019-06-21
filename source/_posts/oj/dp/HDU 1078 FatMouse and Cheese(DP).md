---
title: HDU 1078 FatMouse and Cheese(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-09-02 09:51:55
---
题意 老鼠在一个小镇吃奶酪 城镇可以看成一个n/*n的矩阵 其中每个格子都有一定数量的奶酪mat[i][j] 老鼠从(0,0) 开始吃 而且下个吃的格子里的奶酪必须比上个格子多 老鼠只能水平方向或者垂直方向走 而且每次走的距离不能超过k 求老鼠最多能吃多少奶酪

起点是固定的 比较容易 直接记忆化搜索

令d[i][j]表示以(i,j)为终点的最优解 那么对于所有(i,j)能到达的点(x,y)有 d[i][j]=max(d[i][j],d[x][y]+mat[x][y]) 结果直接输出d[0][0]就行了

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 105;
#define xx i+k*x[l]
#define yy j+k*y[l]
int d[N][N], mat[N][N], m, n;
int x[4] = { -1, 1, 0, 0}, y[4] = {0, 0, -1, 1};

int dp (int i, int j)
{
    if (d[i][j] > 0) return d[i][j];
    d[i][j] = mat[i][j];
    for (int k = 1; k <= m; ++k)
        for (int l = 0; l <= 4; ++l)
        {
            if (xx > 0 && xx <= n && yy > 0 && yy <= n && mat[xx][yy] > mat[i][j])
                d[i][j] = max (d[i][j], mat[i][j] + dp (xx, yy));
        }
    return d[i][j];
}

int main()
{
    while (scanf ("%d%d", &n, &m), n > 0)
    {
        memset (d, -1, sizeof (d));
        for (int i = 1; i <= n; ++i)
            for (int j = 1; j <= n; ++j)
                scanf ("%d", &mat[i][j]);
        printf ("%d\n", dp (1, 1));
    }
    return 0;
}
```

# FatMouse and Cheese

Problem Description

FatMouse has stored some cheese in a city. The city can be considered as a square grid of dimension n: each grid location is labelled (p,q) where 0 <= p < n and 0 <= q < n. At each grid location Fatmouse has hid between 0 and 100 blocks of cheese in a hole. Now he's going to enjoy his favorite food.
FatMouse begins by standing at location (0,0). He eats up the cheese where he stands and then runs either horizontally or vertically to another location. The problem is that there is a super Cat named Top Killer sitting near his hole, so each time he can run at most k locations to get into the hole before being caught by Top Killer. What is worse -- after eating up the cheese at one location, FatMouse gets fatter. So in order to gain enough energy for his next run, he has to run to a location which have more blocks of cheese than those that were at the current hole.
Given n, k, and the number of blocks of cheese at each grid location, compute the maximum amount of cheese FatMouse can eat before being unable to move.
Input

There are several test cases. Each test case consists of
a line containing two integers between 1 and 100: n and k
n lines, each with n numbers: the first line contains the number of blocks of cheese at locations (0,0) (0,1) ... (0,n-1); the next line contains the number of blocks of cheese at locations (1,0), (1,1), ... (1,n-1), and so on.
The input ends with a pair of -1's.
Output

For each test case output in a line the single integer giving the number of blocks of cheese collected.
Sample Input

3 1 1 2 5 10 11 6 12 12 7 -1 -1
Sample Output

37