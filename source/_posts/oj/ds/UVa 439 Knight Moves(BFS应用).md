---
title: UVa 439 Knight Moves(BFS应用)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2015-01-23 18:20:47
---
题意 求国际象棋中骑士从一个位置移东到另一个位置所需最少步数

基础的BFS应用

```js 
#include <bits/stdc++.h>
using namespace std;
int x[] = { -2, -1, -2, -1, 1, 2, 1, 2};
int y[] = { -1, -2, 1, 2, -2, -1, 2, 1};
int d[15][15], sx, sy, ex, ey;
pair<int, int> q[105], t;

int bfs()
{
    int cx, cy, r, c, front = 0, rear = 0;
    memset(d, -1, sizeof(d));
    d[sx][sy] = 0;
    q[rear++] = make_pair(sx, sy);
    while(front < rear)
    {
        t = q[front++];
        cx = t.first, cy = t.second;
        if(cx == ex && cy == ey) return d[cx][cy];
        for(int i = 0; i < 8; ++i)
        {
            r = cx + x[i], c = cy + y[i];
            if(r < 0 || r > 7 || c < 0 || c > 7 || d[r][c] >= 0) continue;
            d[r][c] = d[cx][cy] + 1;
            q[rear++] = make_pair(r, c);
        }
    }
}

int main()
{
    char a[5], b[5];
    while(~scanf("%s%s", a, b))
    {
        sx = a[0] - 'a', sy = a[1] - '1';
        ex = b[0] - 'a', ey = b[1] - '1';
        printf("To get from %s to %s takes %d knight moves.\n", a, b, bfs());
    }
    return 0;
}
```

#

****

A friend of you is doing research on the *Traveling Knight Problem (TKP)* where you are to find the shortest closed tour of knight moves that visits each square of a given set of *n* squares on a chessboard exactly once. He thinks that the most difficult part of the problem is determining the smallest number of knight moves between two given squares and that, once you have accomplished this, finding the tour would be easy.

Of course you know that it is vice versa. So you offer him to write a program that solves the "difficult" part.

Your job is to write a program that takes two squares *a* and *b* as input and then determines the number of knight moves on a shortest route from *a* to *b*.

## Input Specification

The input file will contain one or more test cases. Each test case consists of one line containing two squares separated by one space. A square is a string consisting of a letter (a-h) representing the column and a digit (1-8) representing the row on the chessboard.

## Output Specification

For each test case, print one line saying "To get from *xx* to *yy* takes *n* knight moves.".

## Sample Input

e2 e4 a1 b2 b2 c3 a1 h8 a1 h7 h8 a1 b1 c3 f6 f6

## Sample Output

To get from e2 to e4 takes 2 knight moves. To get from a1 to b2 takes 4 knight moves. To get from b2 to c3 takes 2 knight moves. To get from a1 to h8 takes 6 knight moves. To get from a1 to h7 takes 5 knight moves. To get from h8 to a1 takes 6 knight moves. To get from b1 to c3 takes 1 knight moves. To get from f6 to f6 takes 0 knight moves.