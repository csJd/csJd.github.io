---
title: POJ 1321 棋盘问题(DFS)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Search

date: 2015-03-25 20:03:41
---
题意 中文

简单的搜索题 标记列是否已经有子进行深搜即可 k可能小于n 所以每行都有放子或不放子两种选择

```js 
#include <cstdio>
using namespace std;
const int N = 10;
int n, k, ans, v[N];
char g[N][N];

void dfs(int r, int cnt)
{
    if(cnt >= k) ++ans;
    if(r >= n || cnt >= k) return;
    for(int c = 0; c < n; ++c)   //第r行c列放一个子
        if((!v[c]) && g[r][c] == '#')
            v[c] = 1, dfs(r + 1, cnt + 1), v[c] = 0;
    dfs(r + 1, cnt);   //第r行不放子
}

int main()
{
    while(~scanf("%d%d", &n, &k), n + 1)
    {
        for(int i = 0; i < n; ++i)
            scanf("%s", g[i]);
        dfs(ans = 0, 0);
        printf("%d\n", ans);
    }
    return 0;
}
```

棋盘问题

Description
在一个给定形状的棋盘（形状可能是不规则的）上面摆放棋子，棋子没有区别。要求摆放时任意的两个棋子不能放在棋盘中的同一行或者同一列，请编程求解对于给定形状和大小的棋盘，摆放k个棋子的所有可行的摆放方案C。

Input

输入含有多组测试数据。
每组数据的第一行是两个正整数，n k，用一个空格隔开，表示了将在一个n/*n的矩阵内描述棋盘，以及摆放棋子的数目。 n <= 8 , k <= n
当为-1 -1时表示输入结束。
随后的n行描述了棋盘的形状：每行有n个字符，其中 /# 表示棋盘区域， . 表示空白区域（数据保证不出现多余的空白行或者空白列）。

Output

对于每一组数据，给出一行输出，输出摆放的方案数目C （数据保证C<2^31）。

Sample Input

2 1 /#. ./# 4 4 .../# ../#. ./#.. /#... -1 -1

Sample Output

2 1