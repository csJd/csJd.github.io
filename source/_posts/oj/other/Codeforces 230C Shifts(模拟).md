---
title: Codeforces 230C Shifts(模拟)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Other

date: 2015-04-07 20:05:14
---
题意 有n个m列的转盘 每个转盘的某一列为1或0 你每次可以将某个转盘转动一格 问至少转多少次使得某一列n个转盘上的数都是1

把每个转盘的所有列转为1所需要的最小时间都存起来 可以以某一个1为基点顺时针逆时针各转一圈就可以把每个点需要转的次数算出来 最后看哪一列的和最小就行了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 105, M = 10005;
int d[M], t[M], n, m;
char g[N][M];

void gett(int r, int c)
{
    int p = (c + 1) % m, cnt = t[c] = 0;
    while(p != c) //顺时针转一圈
    {
        if(g[r][p] == '1') cnt = -1;
        t[p] = ++cnt;
        p = (p + 1) % m;
    }

    p = (p + m - 1) % m, cnt = 0;
    while(p != c) //逆时针转一圈
    {
        if(g[r][p] == '1') cnt = -1;
        if(t[p] > cnt + 1) t[p] = cnt + 1;
        d[p] += t[p], ++cnt;
        p = (p + m - 1) % m;
    }
}

int main()
{
    int i, j;
    while(~scanf("%d%d", &n, &m))
    {
        memset(d, 0, sizeof(d));
        for(i = 0; i < n; ++i) scanf("%s", g[i]);

        for(i = 0; i < n; ++i)
        {
            for(j = 0; j < m; ++j)
                if(g[i][j] == '1') gett(i, j), j = m;
            if(j == m) break;
        }
        sort(d, d + m);
        printf("%d\n", i < n ? -1 : d[0]);
    }
    return 0;
}
```

C. Shifts
You are given a table consisting of *n* rows and *m* columns. Each cell of the table contains a number, 0 or 1. In one move we can choose some row of the table and cyclically shift its values either one cell to the left, or one cell to the right.

To cyclically shift a table row one cell to the right means to move the value of each cell, except for the last one, to the right neighboring cell, and to move the value of the last cell to the first cell. A cyclical shift of a row to the left is performed similarly, but in the other direction. For example, if we cyclically shift a row "00110" one cell to the right, we get a row "00011", but if we shift a row "00110" one cell to the left, we get a row "01100".

Determine the minimum number of moves needed to make some table column consist only of numbers 1.

Input

The first line contains two space-separated integers: *n* (1 ≤ *n* ≤ 100) — the number of rows in the table and *m* (1 ≤ *m* ≤ 104) — the number of columns in the table. Then *n* lines follow, each of them contains *m* characters "0" or "1": the *j*-th character of the *i*-th line describes the contents of the cell in the *i*-th row and in the *j*-th column of the table.

It is guaranteed that the description of the table contains no other characters besides "0" and "1".
Output

Print a single number: the minimum number of moves needed to get only numbers 1 in some column of the table. If this is impossible, print -1.

Sample test(s)

input 3 6 101010 000100 100000

output 3
input 2 3 111 000

output -1