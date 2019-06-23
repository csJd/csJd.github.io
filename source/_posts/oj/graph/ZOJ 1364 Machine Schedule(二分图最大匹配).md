---
title: ZOJ 1364 Machine Schedule(二分图最大匹配)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2015-07-10 15:30:36
---
题意 机器调度问题 有两个机器A，B A有n种工作模式0...n-1 B有m种工作模式0...m-1 然后又k个任务要做 每个任务可以用A机器的模式i或b机器的模式j来完成 机器开始都处于模式0 每次换模式时都要重启 问完成所有任务机器至少重启多少次

最基础的二分图最大匹配问题 对于每个任务把i和j之间连一条边就可以构成一个二分图 那么每个任务都可以对应一条边 那么现在就是要找最少的点 使这些点能覆盖所有的边 即点覆盖数 又因为二分图的点覆盖数 = 匹配数 那么就是裸的求二分图最大匹配问题了 两边的点数都不超过100直接DFS增广就行了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 105;
int g[N][N], a[N], b[N], vis[N];
int n, m, k, ans;

int dfs(int i)   //DFS增广
{
    for(int j = 1; j < m; ++j)
    {
        if(g[i][j] && !vis[j])
        {
            vis[j] = 1;
            if( b[j] == -1 || dfs(b[j]))
            {
                //机器a的模式i与机器b的模式j匹配
                a[i] = j;
                b[j] = i;
                return 1;
            }
        }
    }
    return 0;
}

int main()
{
    int u, v, t;
    while(scanf("%d", &n), n)
    {
        memset(g, 0, sizeof(g));
        scanf("%d%d", &m, &k);
        for(int i = 0; i < k; ++i)
        {
            scanf("%d%d%d", &t, &u, &v);
            g[u][v] = 1;
        }

        ans = 0;
        memset(a, -1, sizeof(a));
        memset(b, -1, sizeof(b));
        //状态0不需要重启  所以可以忽略0
        for(int i = 1; i < n; ++i)
        {
            if(a[i] == -1)  //i没被匹配 以i为起点找一条增广路
            {
                memset(vis, 0, sizeof(vis));
                ans += dfs(i);  //
            }
        }

        printf("%d\n", ans);
    }
    return 0;
}
//Last modified :   2015-07-10 14:50
```
  Machine Schedule    Time Limit:2 Seconds Memory Limit:65536 KB

As we all know, machine scheduling is a very classical problem in computer science and has been studied for a very long history. Scheduling problems differ widely in the nature of the constraints that must be satisfied and the type of schedule desired. Here we consider a 2-machine scheduling problem.
There are two machines A and B. Machine A has n kinds of working modes, which is called mode_0, mode_1, ��, mode_n-1, likewise machine B has m kinds of working modes, mode_0, mode_1, �� , mode_m-1. At the beginning they are both work at mode_0.
For k jobs given, each of them can be processed in either one of the two machines in particular mode. For example, job 0 can either be processed in machine A at mode_3 or in machine B at mode_4, job 1 can either be processed in machine A at mode_2 or in machine B at mode_4, and so on. Thus, for job i, the constraint can be represent as a triple (i, x, y), which means it can be processed either in machine A at mode_x, or in machine B at mode_y.
Obviously, to accomplish all the jobs, we need to change the machine's working mode from time to time, but unfortunately, the machine's working mode can only be changed by restarting it manually. By changing the sequence of the jobs and assigning each job to a suitable machine, please write a program to minimize the times of restarting machines.

**Input**
The input file for this program consists of several configurations. The first line of one configuration contains three positive integers: n, m (n, m < 100) and k (k < 1000). The following k lines give the constrains of the k jobs, each line is a triple: i, x, y.
The input will be terminated by a line containing a single zero.

**Output**
The output should be one integer per line, which means the minimal times of restarting machine.

**Sample Input**

5 5 10
0 1 1
1 1 2
2 1 3
3 1 4
4 2 1
5 2 2
6 2 3
7 2 4
8 3 3
9 4 3
0

**Sample Output**

3