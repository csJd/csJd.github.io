---
title: UVa 624 CD （DP 01背包）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-22 09:38:23
---
题意 你要把CD上的歌录到tape上 使得剩余磁带空间最小 很容易看出是01背包问题 输出路径也很容易 直接递归

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
int s,n,val[30],d[1000005],pre[1000005];
void print(int i)
{
    if(pre[i])
    {
        print(pre[i]);
        printf("%d ",i-pre[i]);
    }
    else
    {
        printf("%d ",i);
        return;
    }
}
int main()
{
    while(scanf("%d %d",&s,&n)!=EOF)
    {
        memset(d,0,sizeof(d));
        memset(pre,0,sizeof(pre));
        for(int i=0; i<n; ++i)
        {
            scanf("%d",&val[i]);
            for(int j=s; j>=val[i]; --j)
                if(d[j-val[i]]+val[i]>d[j])
                {
                    d[j]=d[j-val[i]]+val[i];
                    pre[j]=j-val[i];
                }
        }
        print(d[s]);
        printf("sum:%d\n",d[s]);

    }
    return 0;
}
```

#

You have a long drive by car ahead. You have a tape recorder, but unfortunately your best music is on CDs. You need to have it on tapes so the problem to solve is: you have a tapeNminutes long. How to choose tracks from CD to get most out of tape space and have as short unused space as possible.

Assumptions:

Program should find the set of tracks which fills the tape best and print it in the same sequence as the tracks are stored on the CD

##

Any number of lines. Each one contains valueN, (after space) number of tracks and durations of the tracks. For example from first line in sample data:N=5, number of tracks=3, first track lasts for 1 minute, second one 3 minutes, next one 4 minutes

##

Set of tracks (and durations) which are the correct solutions and string ``sum:" and sum of duration times.

##

5 3 1 3 4 10 4 9 8 4 2 20 4 10 5 7 4 90 8 10 23 1 2 3 4 5 7 45 8 4 10 44 43 12 9 8 2

##

1 4 sum:5 8 2 sum:10 10 5 4 sum:19 10 23 1 2 3 4 5 7 sum:55 4 10 12 9 8 2 sum:45

Miguel A. Revilla
2000-01-10

﻿﻿