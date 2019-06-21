---
title: HDU 1978 How many ways(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-09-02 09:51:34
---
题意 中文 但要注意小于你能量的点也是能到达的

令d[i][j]表示到达第i行第j列的方法数 初始化为0 d[1][1]为1 输入一个点的能量t后 枚举这个点能到的所有点(i+x,j+y)(x+y<=t) 有d[i+x][j+y]+=d[i][j] 因为只能向右走和向下走 可以保证每次更新后均为当前最优解 输入最后一个点后 就得到答案了

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 105, MOD = 10000;
int main()
{
    int d[N][N], n, m, cas, t;
    scanf ("%d", &cas);
    while (cas--)
    {
        scanf ("%d%d", &n, &m);
        memset (d, 0, sizeof (d)), d[1][1] = 1;
        for (int i = 1; i <= n; ++i)
            for (int j = 1; j <= m; ++j)
            {
                scanf ("%d", &t);
                for (int x = 0; x <= t && i + x <= n; ++x)
                    for (int y = 0; x + y <= t && j + y <= m; ++y)
                    {
                        if (x == 0 && y == 0) continue;
                        d[i + x][j + y] = (d[i][j] + d[i + x][j + y]) % MOD;
                    }
            }
        printf ("%d\n", d[n][m]);
    }
    return 0;
}
```

# How many ways

Problem Description

这是一个简单的生存游戏，你控制一个机器人从一个棋盘的起始点(1,1)走到棋盘的终点(n,m)。游戏的规则描述如下：
1.机器人一开始在棋盘的起始点并有起始点所标有的能量。
2.机器人只能向右或者向下走，并且每走一步消耗一单位能量。
3.机器人不能在原地停留。
4.当机器人选择了一条可行路径后，当他走到这条路径的终点时，他将只有终点所标记的能量。
![](../images/cn-data-images-C113-1003-1.gif.png)
如上图，机器人一开始在(1,1)点，并拥有4单位能量，蓝色方块表示他所能到达的点，如果他在这次路径选择中选择的终点是(2,4)
点，当他到达(2,4)点时将拥有1单位的能量，并开始下一次路径选择，直到到达(6,6)点。
我们的问题是机器人有多少种方式从起点走到终点。这可能是一个很大的数，输出的结果对10000取模。
Input

第一行输入一个整数T,表示数据的组数。
对于每一组数据第一行输入两个整数n,m(1 <= n,m <= 100)。表示棋盘的大小。接下来输入n行,每行m个整数e(0 <= e < 20)。
Output

对于每一组数据输出方式总数对10000取模的结果.
Sample Input

1 6 6 4 5 6 6 4 3 2 2 3 1 7 2 1 1 4 6 2 7 5 8 4 3 9 5 7 6 6 2 1 5 3 1 1 3 7 2
Sample Output

3948