---
title: POJ 1135 Domino Effect(最短路 多米诺骨牌)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-10-26 21:34:13
---
题意

题目描述：
你知道多米诺骨牌除了用来玩多米诺骨牌游戏外，还有其他用途吗？多米诺骨牌游戏：取一 些多米诺骨牌，竖着排成连续的一行，两张骨牌之间只有很短的空隙。如果排列得很好，当你推 倒第 1张骨牌，会使其他骨牌连续地倒下（这就是短语“多米诺效应”的由来）。 然而当骨牌数量很少时，这种玩法就没多大意思了，所以一些人在 80 年代早期开创了另一个 极端的多米诺骨牌游戏：用上百万张不同颜色、不同材料的骨牌拼成一幅复杂的图案。他们开创 了一种流行的艺术。在这种骨牌游戏中，通常有多行骨牌同时倒下。 你的任务是编写程序，给定这样的多米诺骨牌游戏，计算后倒下的是哪一张骨牌、在什么 时间倒下。这些多米诺骨牌游戏包含一些“关键牌”，他们之间由一行普通骨牌连接。当一张关键 牌倒下时，连接这张关键牌的所有行都开始倒下。当倒下的行到达其他还没倒下的关键骨牌时， 则这些关键骨牌也开始倒下，同样也使得连接到它的所有行开始倒下。每一行骨牌可以从两个端 点中的任何一张关键牌开始倒下，甚至两个端点的关键牌都可以分别倒下，在这种情形下，该行 后倒下的骨牌为中间的某张骨牌。假定骨牌倒下的速度一致。

输入描述：
输入文件包含多个测试数据，每个测试数据描述了一个多米诺骨牌游戏。每个测试数据的第 1行为两个整数：n和m，n表示关键牌的数目，1≤n<500；m表示这 n张牌之间用 m行普通骨 牌连接。n 张关键牌的编号为 1～n。每两张关键牌之间至多有一行普通牌，并且多米诺骨牌图案 是连通的，也就是说从一张骨牌可以通过一系列的行连接到其他每张骨牌。 接下来有 m行，每行为 3个整数：a、b和t，表示第 a张关键牌和第 b张关键牌之间有一行 普通牌连接，这一行从一端倒向另一端需要 t 秒。每个多米诺骨牌游戏都是从推倒第 1 张关键牌 开始的。 输入文件后一行为 n = m = 0，表示输入结束。

输出描述：
对输入文件中的每个测试数据，首先输出一行"System /#k"，其中 k为测试数据的序号；然后 再输出一行，首先是后一块骨牌倒下的时间，精确到小数点后一位有效数字，然后是后倒下 骨牌的位置，这张后倒下的骨牌要么是关键牌，要么是两张关键牌之间的某张普通牌。输出格 式如样例输出所示。如果存在多个解，则输出任意一个。每个测试数据的输出之后输出一个空行。

先用最短路算出每个关键牌i的倒下时间d[i] 然后两个关键牌i,j间最后倒下的时间应该为 (d[i]+d[j]+mat[i][j])/2 取时间最大的就是答案了 注意当i==j时说明最后倒下的是关键节点 此时输出有所不同

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 505;
int d[N], v[N], mat[N][N], n, m;

void dijkstra()
{
    memset(d, 0x3f, sizeof(d));
    memset(v, 0, sizeof(v));
    d[1] = 0;
    for(int i =  1; i <= n; ++i)
    {
        int cur = 0;
        for(int j = 1; j <= n; ++j)
            if(!v[j] && d[j] < d[cur]) cur = j;
        v[cur] = 1;
        for(int j = 1; j <= n; ++j)
            if(d[j] > d[cur] + mat[cur][j])
                d[j] = d[cur] + mat[cur][j];
    }
}

int main()
{
    int pi, pj, ans, a, b, c, cas = 0;
    while(scanf("%d%d", &n, &m), n)
    {
        memset(mat, 0x3f, sizeof(mat));
        for(int i = 1; i <= n; ++i) mat[i][i] = 0;
        for(int i = 1; i <= m; ++i)
        {
            scanf("%d%d%d", &a, &b, &c);
            mat[a][b] = mat[b][a] = c;
        }
        dijkstra();

        int ans = 0;
        for(int i = 1; i <= n; ++i)
        {
            for(int j = i; j <= n; ++j)
            {
                if(mat[i][j] >= d[0]) continue;
                if(d[i] + d[j] + mat[i][j] > ans || (i == j && 2 * d[i] >= ans))
                {
                    ans = d[i] + d[j] + mat[i][j];
                    pi = i, pj = j;
                }
            }
        }

        double t = ans / 2.0;
        printf("System #%d\nThe last domino falls after %.1f seconds, ", ++cas, t);
        if(pi == pj)  printf("at key domino %d.\n\n", pi);
        else   printf("between key dominoes %d and %d.\n\n", pi, pj);
    }
    return 0;
}
```

Domino Effect

Description
Did you know that you can use domino bones for other things besides playing Dominoes? Take a number of dominoes and build a row by standing them on end with only a small distance in between. If you do it right, you can tip the first domino and cause all others to fall down in succession (this is where the phrase ``domino effect'' comes from).
While this is somewhat pointless with only a few dominoes, some people went to the opposite extreme in the early Eighties. Using millions of dominoes of different colors and materials to fill whole halls with elaborate patterns of falling dominoes, they created (short-lived) pieces of art. In these constructions, usually not only one but several rows of dominoes were falling at the same time. As you can imagine, timing is an essential factor here.
It is now your task to write a program that, given such a system of rows formed by dominoes, computes when and where the last domino falls. The system consists of several ``key dominoes'' connected by rows of simple dominoes. When a key domino falls, all rows connected to the domino will also start falling (except for the ones that have already fallen). When the falling rows reach other key dominoes that have not fallen yet, these other key dominoes will fall as well and set off the rows connected to them. Domino rows may start collapsing at either end. It is even possible that a row is collapsing on both ends, in which case the last domino falling in that row is somewhere between its key dominoes. You can assume that rows fall at a uniform rate.

Input

The input file contains descriptions of several domino systems. The first line of each description contains two integers: the number n of key dominoes (1 <= n < 500) and the number m of rows between them. The key dominoes are numbered from 1 to n. There is at most one row between any pair of key dominoes and the domino graph is connected, i.e. there is at least one way to get from a domino to any other domino by following a series of domino rows.
The following m lines each contain three integers a, b, and l, stating that there is a row between key dominoes a and b that takes l seconds to fall down from end to end.
Each system is started by tipping over key domino number 1.
The file ends with an empty system (with n = m = 0), which should not be processed.

Output

For each case output a line stating the number of the case ('System /#1', 'System /#2', etc.). Then output a line containing the time when the last domino falls, exact to one digit to the right of the decimal point, and the location of the last domino falling, which is either at a key domino or between two key dominoes(in this case, output the two numbers in ascending order). Adhere to the format shown in the output sample. The test data will ensure there is only one solution. Output a blank line after each system.

Sample Input

2 1 1 2 27 3 3 1 2 5 1 3 5 2 3 5 0 0

Sample Output

System /#1 The last domino falls after 27.0 seconds, at key domino 2. System /#2 The last domino falls after 7.5 seconds, between key dominoes 2 and 3.