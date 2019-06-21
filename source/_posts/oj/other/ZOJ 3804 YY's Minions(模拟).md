---
title: ZOJ 3804 YY's Minions(模拟)
author: Deng
tags: 
       - Algorithm

category: 
       - Other
       - OJ

date: 2014-09-01 22:23:18
---
题意 你有n/*m个小兵排成一个矩阵 每个在矩阵里的小兵有两种状态 0睡着 或 1清醒 每秒都会发生如下事件 睡着的小兵在他周围恰好有3个醒着的小兵时会醒过来 醒着的 小兵在周围醒着的的小兵数大于3或小于2时会睡着 还有k个小兵会在一定的时间离开 X 求f秒之后所有小兵的状态

直接模拟就行咯~~

```js 
#include<cstdio>
#include<stack>
#include<cstring>
using namespace std;
const int N=55,F=1005;
#define r (x+i)
#define c (y+j)
char s[N][N],cn[N][N];
int n,m;
int cnt(int x,int y)
{
    int ccc=0;
    for(int i=-1; i<=1; ++i)
        for(int j=-1; j<=1; ++j)
            if(r>0&&r<=n&&c>0&&c<=m&&(i||j))
            {
                if(s[r][c]=='1') ++ccc;
            }
    return ccc;
}

void turn(int x,int y)
{
    int cc=cn[x][y];
    if(s[x][y]=='0'&&cc==3)
        s[x][y]='1';
    else if(s[x][y]=='1'&&((cc<2)||(cc>3)))
        s[x][y]='0';
}

int main()
{
    int cas,f,k,t,x,y,row,col;
    scanf("%d",&cas);
    while(cas--)
    {
        scanf("%d%d%d%d",&n,&m,&f,&k);
        for(int i=1; i<=n; ++i)
            scanf("%s",s[i]+1);
        stack<int> lx[F],ly[F];
        for(int i=1; i<=k; ++i)
        {
            scanf("%d%d%d",&t,&x,&y);
            lx[t].push(x);
            ly[t].push(y);
        }

        for(t=1; t<=f; ++t)
        {
            for(int i=1; i<=n; ++i)
                for(int j=1; j<=m; ++j)
                    cn[i][j]=cnt(i,j);

            for(int i=1; i<=n; ++i)
                for(int j=1; j<=m; ++j)
                    turn(i,j);

            while(!(lx[t].empty()))
            {
                row=lx[t].top();
                col=ly[t].top();
                s[row][col]='X';
                lx[t].pop();
                ly[t].pop();
            }
        }
        for(int i=1; i<=n; ++i)
            printf("%s\n",s[i]+1);
    }
    return 0;
}
```
  YY's Minions    Time Limit:2 Seconds  Memory Limit:65536 KB

Despite YY's so much homework, she would like to take some time to play with her minions first.

YY lines her minions up to anN/*Mmatrix. Every minionhas two statuses: awake or asleep. We use 0(the digit) to represent that it is asleep, and 1 for awake. Also, we define the minions who are around a minion**closest**in one of the**eight**directions its neighbors. And every minute every minion will change its status by the following specific rules:
If this minion is awake, and the number of its neighbors who are awake is less than 2, this minion will feel lonely and turn to asleep.If this minion is awake, and the number of its neighbors who are awake is more than 3, this minion will turn to asleep for it will feel too crowded.If this minion is awake, and the number of its neighbors who are awake is exactly 2 or 3, this minion will keep being awake and feel very happy.If this minion is asleep, and the number of its neighbors who are awake is exactly 3, this minion will wake up because of the noise.

Note that all changes take place at the same time at the beginning of a specific minute.

Also, some minions will get bored and leave this silly game. We use 'X's to describe them. We suppose that a minion would leave afterTminutes. It will leave at the end of the Tthminute. Its status is considered during the change at the beginning of the Tthminute, and should be ignored after that. Of course, one minion will not leave twice!

YY is a girl full of curiosity and wants to know every minion's status afterFminutes. But you know she is weak and lazy! Please help this cute girl to solve this problem :)

#### Input

There are multiple test cases.

The first line contains the number of test casesQ. 1<=Q<=100.
For each case, there are several lines:
The first line contains four integersN,M,F,K.Kmeans the number of leaving messages. 1<=N,M<=50, 1<=F<=1000, 1<=K<=N/*M.
NextNlines are the matrix which shows the initial status of each minion. Each line containsMchars. We guarantee that 'X' wouldn't appear in initial status matrix.
And nextKlines are the leaving messages. Each line contains three integersTi,Xi,Yi, They mean the minion who is located in (Xi,Yi) will leave the game at the end of theTithminutes. 1<=Ti<=F, 1<=Xi<=N, 1<=Yi<=M.

#### Output

For each case, outputNlines as a matrix which shows the status of each minion afterFminutes.

#### Sample Input

2 3 3 2 1 101 110 001 1 2 2 5 5 6 3 10111 01000 00000 01100 10000 2 3 3 2 4 1 5 1 5

#### Sample Output

010 1X0 010 0000X 11000 00X00 X0000 00000

#### Hint

For case 1: T=0, the game starts 101 110 001 --------------- at the beginning of T=1, a change took place 100 101 010 --------------- at the end of T=1 (the minion in (2,2) left) 100 1X1 010 --------------- at the beginning of T=2, a change took place 010 1X0 010 --------------- at the end of T=2 (nothing changed for no minion left at T=2) 010 1X0 010

﻿﻿