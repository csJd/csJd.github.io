---
title: POJ 2570 Fiber Network（最短路 二进制处理）
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2014-10-30 22:15:14
---
题目翻译

一些公司决定搭建一个更快的网络，称为“光纤网”。他们已经在全世界建立了许多站点，这 些站点的作用类似于路由器。不幸的是，这些公司在关于站点之间的接线问题上存在争论，这样“光纤网”项目就被迫终止了，留下的是每个公司自己在某些站点之间铺设的线路。 现在，Internet 服务供应商，当想从站点 A传送数据到站点 B，就感到困惑了，到底哪个公司 能够提供必要的连接。请帮助供应商回答他们的查询，查询所有可以提供从站点 A到站定 B的线 路连接的公司。

输入描述：

输入文件包含多个测试数据。每个测试数据第 1行为一个整数 n，代表网络中站点的个数，n = 0 代表输入结束，否则 n的范围为：1≤n≤200。站点的编号为 1, …, n。接下来列出了这些站 点之间的连接。每对连接占一行，首先是两个整数 A和B，A = B = 0 代表连接列表结束，否则 A、 B的范围为：1≤A, B≤n，表示站点 A和站点 B之间的单向连接；每行后面列出了拥有站点 A到 B之间连接的公司，公司用小写字母标识，多个公司的集合为包含小写字母的字符串。 连接列表之后，是供应商查询的列表。每个查询包含两个整数 A和B，A = B = 0 代表查询列 表结束，也代表整个测试数据结束，否则 A、B 的范围为：1≤A, B≤n，代表查询的起始和终止 站点。假定任何一对连接和查询的两个站点都不相同。

输出描述：

对测试数据中的每个查询，输出一行，为满足以下条件的所有公司的标识：这些公司可以通 过自己的线路为供应商提供从查询的起始站点到终止站点的数据通路。如果没有满足条件的公司， 则仅输出字符"-"。每个测试数据的输出之后输出一个空行。

公司最多有26个 可以用2进制来表示站点间的连接关系 如果提供站点 1 到站点 2 的连接的公司集合为{ 'a', 'b', 'c' }，可以用 “00000000000000000000000000000111”表示，提供站点 2到站点 3的连接的公司集合为{ 'a', 'd' }，用“00000000000000000000000000001001”表示，这两个整数进行二进制与运算后 得到“00000000000000000000000000000001”，表示通过中间站点 2，提供站点 1到站点 3的连 接的公司集合为{ 'a' }。

这样就floyd进行处理就类似最短路问题了

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 205;
int d[N][N], n;

void floyd()
{
    for(int k = 1; k <= n; ++k)
    for(int i = 1; i <= n; ++i)
    for(int j = 1; j <= n; ++j)
        d[i][j] |= (d[i][k] & d[k][j]);

}

int main()
{
    int a, b;
    char s[30];
    while(scanf("%d", &n), n)
    {
        memset(d, 0, sizeof(d));
        while(scanf("%d%d", &a, &b), a)
        {
            scanf("%s", s);
            for(int i = 0; s[i] != '\0'; ++i)
                d[a][b] = d[a][b] | (1 << s[i] - 'a');
        }
        floyd();
        while(scanf("%d%d", &a, &b), a)
        {
            for(char c = 'a'; c <= 'z'; ++c)
                if(d[a][b] & (1 << c - 'a')) printf("%c", c);
            if(d[a][b] == 0) printf("-");
            printf("\n");
        }
        printf("\n");
    }
    return 0;
}
```

Fiber Network

Description
Several startup companies have decided to build a better Internet, called the "FiberNet". They have already installed many nodes that act as routers all around the world. Unfortunately, they started to quarrel about the connecting lines, and ended up with every company laying its own set of cables between some of the nodes.
Now, service providers, who want to send data from node A to node B are curious, which company is able to provide the necessary connections. Help the providers by answering their queries.

Input

The input contains several test cases. Each test case starts with the number of nodes of the network n. Input is terminated by n=0. Otherwise, 1<=n<=200. Nodes have the numbers 1, ..., n. Then follows a list of connections. Every connection starts with two numbers A, B. The list of connections is terminated by A=B=0. Otherwise, 1<=A,B<=n, and they denote the start and the endpoint of the unidirectional connection, respectively. For every connection, the two nodes are followed by the companies that have a connection from node A to node B. A company is identified by a lower-case letter. The set of companies having a connection is just a word composed of lower-case letters.
After the list of connections, each test case is completed by a list of queries. Each query consists of two numbers A, B. The list (and with it the test case) is terminated by A=B=0. Otherwise, 1<=A,B<=n, and they denote the start and the endpoint of the query. You may assume that no connection and no query contains identical start and end nodes.

Output

For each query in every test case generate a line containing the identifiers of all the companies, that can route data packages on their own connections from the start node to the end node of the query. If there are no companies, output "-" instead. Output a blank line after each test case. ![](../images/es-2570_1.jpg.png)

Sample Input

3 1 2 abc 2 3 ad 1 3 b 3 1 de 0 0 1 3 2 1 3 2 0 0 2 1 2 z 0 0 1 2 2 1 0 0 0

Sample Output

ab d - z -