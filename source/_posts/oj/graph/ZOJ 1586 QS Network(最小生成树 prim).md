---
title: ZOJ 1586 QS Network(最小生成树 prim)
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2014-10-24 15:25:41
---
题意 输入n 然后输入n个数 代表连接时每个站点自身的消耗 然后输入n/*n的矩阵 第i行第j列的数代表第i个站点和第j个站点之间路上的花费 链接i,j两个节点的总花费为两站点自身花费加上路上的花费 求⑩这n个站点连通的最小总花费

又是裸的最小生成树 给点的就用prim咯

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 1005;
int cost[N], mat[N][N], a[N], n, ans;

void prim()
{
    memset(cost, 0x3f, sizeof(cost));
    cost[1] = -1;
    //cost[i]为把第i个节点加入到最小生成树的最小花费,-1代表已经加入
    int cur = 1, next = 0;
    //cur为当前加入到最小生成树中的节点
    //next为下一个应该加入最小生成树的节点
    for(int i = 1; i < n; ++i)
    {
        for(int j = 1; j <= n; ++j)
        {
            if(cost[j] == -1 || j == cur) continue;
            cost[j] = min(cost[j], mat[cur][j]);
            if(cost[j] < cost[next]) next = j;
        }
        ans += cost[next];
        cur = next, cost[cur] = -1, next = 0;
    }
}

int main()
{
    int cas, t;
    scanf("%d", &cas);
    while(cas--)
    {
        scanf("%d", &n);
        for(int i = 1; i <= n; ++i)
            scanf("%d", &a[i]);
        for(int i = 1; i <= n; ++i)
        {
           for(int j = 1; j <= n; ++j)
            {
                scanf("%d", &t);
                mat[i][j] = a[i] + a[j] + t;
            }
        }
        ans = 0;  prim();
        printf("%d\n", ans);
    }
    return 0;
}
```
  QS Network    Time Limit:2 Seconds Memory Limit:65536 KB

## Sunny Cup 2003 - Preliminary Round

April 20th, 12:00 - 17:00

In the planet w-503 of galaxy cgb, there is a kind of intelligent creature named QS. QScommunicate with each other via networks. If two QS want to get connected, they need to buy two network adapters (one for each QS) and a segment of network cable. Please be advised that ONE NETWORK ADAPTER CAN ONLY BE USED IN A SINGLE CONNECTION.(ie. if a QS want to setup four connections, it needs to buy four adapters). In the procedure of communication, a QS broadcasts its message to all the QS it is connected with, the group of QS who receive the message broadcast the message to all the QS they connected with, the procedure repeats until all the QS's have received the message.

A sample is shown below:

![](../images/cn-onlinejudge-showImage.do-name=0000%2F1586%2F1586.gif.png)

A sample QS network, and QS A want to send a message.
Step 1. QS A sends message to QS B and QS C;
Step 2. QS B sends message to QS A ; QS C sends message to QS A and QS D;
Step 3. the procedure terminates because all the QS received the message.

Each QS has its favorate brand of network adapters and always buys the brand in all of its connections. Also the distance between QS vary. Given the price of each QS's favorate brand of network adapters and the price of cable between each pair of QS, your task is to write a program to determine the minimum cost to setup a QS network.

**Input**

The 1st line of the input contains an integer t which indicates the number of data sets.
From the second line there are t data sets.
In a single data set,the 1st line contains an interger n which indicates the number of QS.
The 2nd line contains n integers, indicating the price of each QS's favorate network adapter.
In the 3rd line to the n+2th line contain a matrix indicating the price of cable between ecah pair of QS.

Constrains:

all the integers in the input are non-negative and not more than 1000.

**Output**

for each data set,output the minimum cost in a line. NO extra empty lines needed.

**Sample Input**

1
3
10 20 30
0 100 200
100 0 300
200 300 0

**Sample Output**

370