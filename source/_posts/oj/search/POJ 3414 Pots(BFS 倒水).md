---
title: POJ 3414 Pots(BFS 倒水)
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2015-04-02 17:11:48
---
题意 你有两个容积分别为a,b杯子 你每次可以将某个杯子中的水倒满或者倒掉或者倒到另一个杯子 问能否通过这两个杯子量出c容量的水

和上一个倒可乐问题类似 只是这个操作更多了点 将两个杯子中各含有的水作为状态 每出队列一个状态 将所有可能到达的状态入队 直到有一个杯子里面水的体积为c 打印路径直接递归就行了

```js 
#include <map>
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 105;
int a, b, c, t, le, ri, v[N][N];
int x[N * N], p[N * N], op[N * N], d[N * N];
pair<int, int> q[N * N];

void check(int i, int j, int o, int k)
{
    if(v[i][j]) return;
    v[i][j] = 1, p[ri] = le;
    op[ri] = o, x[ri] = k, d[ri] = d[le] + 1;
    q[ri++] = make_pair(i, j);
}

int bfs()
{
    int ca, cb = le = ri = 0;
    q[ri++] = make_pair(0, 0);
    memset(v, 0, sizeof(v)), v[0][0] = 1;

    while(le < ri)
    {
        ca = q[le].first, cb = q[le].second;
        if(ca == c || cb == c) return le;
        check(a, cb, 1, 1);                 //FILL(1);
        check(ca, b, 1, 2);                 //FILL(2);
        check(0, cb, 2, 1);                 //DROP(1);
        check(ca, 0, 2, 2);                 //DROP(2);
        if(ca > b - cb) check(ca - b + cb, b, 3, 1);
        else check(0, ca + cb, 3, 1);       //POUR(1,2);
        if(cb > a - ca) check(a, cb - a + ca, 3, 2);
        else check(ca + cb, 0, 3, 2);       //POUR(2,1);
        ++le;
    }
    return 0;
}

void print(int k)
{
    if(p[k] > 0) print(p[k]);
    if(op[k] == 1) printf("FILL(%d)\n", x[k]);
    if(op[k] == 2) printf("DROP(%d)\n", x[k]);
    if(op[k] == 3) printf("POUR(%d,%d)\n", x[k], 3 - x[k]);
}

int main()
{
    int ans;
    while(~scanf("%d%d%d", &a, &b, &c))
    {
        if(ans = bfs())
            printf("%d\n",d[ans]), print(ans);
        else puts("impossible");
    }
    return 0;
}
```

Pots

Description
You are given two pots, having the volume of **A** and **B** liters respectively. The following operations can be performed:

Write a program to find the shortest possible sequence of these operations that will yield exactly **C** liters of water in one of the pots.

Input

On the first and only line are the numbers **A**, **B**, and **C**. These are all integers in the range from 1 to 100 and **C**≤max(**A**,**B**).

Output

The first line of the output must contain the length of the sequence of operations **K**. The following **K** lines must each describe one operation. If there are several sequences of minimal length, output any one of them. If the desired result can’t be achieved, the first and only line of the file must contain the word ‘**impossible**’.

Sample Input

3 5 4

Sample Output

6 FILL(2) POUR(2,1) DROP(1) POUR(2,1) FILL(2) POUR(2,1)