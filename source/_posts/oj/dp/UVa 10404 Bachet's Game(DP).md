---
title: UVa 10404 Bachet's Game(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-23 10:52:21
---
题意 给你n个小石头 和一个数组a[m] 然后两个人轮流取石头 stan先取 olive后取 他们都完美发挥 谁取完最后一个石头谁就是赢家 感觉不是很容易看出来是dp题 令d[i]表示只有i个石子时谁赢 1表示stan赢 0表示olive赢

i-a[j]表示从i个石子一次取走a[j]个还剩下的 所以有 当(i-a[j]>0&&d[i-a[j]]=0)时 d[i]=1;

```js 
#include<cstdio>  
#include<cstring>  
using namespace std;  
int d[1000005],a[12],ans,n,m;  
int main()  
{  
    while(scanf("%d",&n)!=EOF)  
    {  
        scanf("%d",&m);  
        for(int i=1;i<=m;++i)  
                scanf("%d",&a[i]);  
        memset(d,0,sizeof(d));  
        for(int i=1;i<=n;++i)  
            for(int j=1;j<=m;++j)  
            {  
                if(i-a[j]>=0&&d[i-a[j]]==0)  
                {  
                    d[i]=1;  
                    break;  
                }  
            }  
        printf(d[n]?"Stan wins\n":"Ollie wins\n");  
    }  
}
```

##

## Bachet's Game

![](../images/dge.org-external-104-p10404.jpg.png)Bachet's game is probably known to all but probably not by this name. Initially there are**n**stones on the table. There are two players Stan and Ollie, who move alternately. Stan always starts. The legal moves consist in removing at least one but not more than**k**stones from the table. The winner is the one to take the last stone.

Here we consider a variation of this game. The number of stones that can be removed in a single move must be a member of a certain set of **m** numbers. Among the **m** numbers there is always 1 and thus the game never stalls.

The input consists of a number of lines. Each line describes one game by a sequence of positive numbers. The first number is**n**<= 1000000 the number of stones on the table; the second number is**m**<= 10 giving the number of numbers that follow; the last**m**numbers on the line specify how many stones can be removed from the table in a single move.

For each line of input, output one line saying eitherStan winsorOllie winsassuming that both of them play perfectly.

20 3 1 3 8 21 3 1 3 8 22 3 1 3 8 23 3 1 3 8 1000000 10 1 23 38 11 7 5 4 8 3 13 999996 10 1 23 38 11 7 5 4 8 3 13

Stan wins Stan wins Ollie wins Stan wins Stan wins Ollie wins

﻿﻿