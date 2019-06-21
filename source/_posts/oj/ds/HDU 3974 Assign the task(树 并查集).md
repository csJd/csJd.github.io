---
title: HDU 3974 Assign the task(树 并查集)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-04-22 21:48:27
---
题意 公司中有n个员工 除了boss 每个员工都有自己的上司 自己下属的下属也是自己的下属 当给一个员工分配任务时 这个员工会把任务也分配到自己的所有下属 每个员工都只做最后一个被分配的任务 对于每个C x 输出员工x正在做的任务 没有就输出-1

把员工的关系数建成类似并查集的结构 把每个直接分配任务的员工的任务和任务分配时间保存起来 查询时只要找这个员工所有父节点中最晚分配的任务

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 50005;
struct employee{
    int task, t;
} e[N];
int par[N];

int main()
{
    int cas, a, b, n, m;
    char op[5];
    scanf("%d", &cas);
    for(int k = 1; k <= cas; ++k)
    {
        printf("Case #%d:\n", k);
        memset(par, -1, sizeof(par));
        scanf("%d", &n);
        for(int i = 1; i < n; ++i)
        {
            e[i].t = e[i].task = 0;
            scanf("%d%d", &a, &b), par[a] = b;
        }
        e[n].t = e[n].task = 0;

        scanf("%d", &m);
        int t = 0, last, ans;
        while(m--)
        {
            scanf("%s%d", op, &a);
            if(op[0] == 'C')
            {
                last = 0;//所有祖先节点最晚任务的时间
                while(a != -1)
                {
                    if(e[a].t > last)
                        last = e[a].t, ans = e[a].task;
                    a = par[a];
                }
                printf("%d\n", last ? ans : -1);
            }
            else
            {
                scanf("%d", &b);
                e[a].task = b, e[a].t = ++t;
            }
        }
    }
    return 0;
}
//Last modified :   2015-04-22 20:48
```

# Assign the task

Problem Description

There is a company that has N employees(numbered from 1 to N),every employee in the company has a immediate boss (except for the leader of whole company).If you are the immediate boss of someone,that person is your subordinate, and all his subordinates are your subordinates as well. If you are nobody's boss, then you have no subordinates,the employee who has no immediate boss is the leader of whole company.So it means the N employees form a tree.
The company usually assigns some tasks to some employees to finish.When a task is assigned to someone,He/She will assigned it to all his/her subordinates.In other words,the person and all his/her subordinates received a task in the same time. Furthermore,whenever a employee received a task,he/she will stop the current task(if he/she has) and start the new one.
Write a program that will help in figuring out some employee’s current task after the company assign some tasks to some employee.
Input

The first line contains a single positive integer T( T <= 10 ), indicates the number of test cases.
For each test case:
The first line contains an integer N (N ≤ 50,000) , which is the number of the employees.
The following N - 1 lines each contain two integers u and v, which means the employee v is the immediate boss of employee u(1<=u,v<=N).
The next line contains an integer M (M ≤ 50,000).
The following M lines each contain a message which is either
"C x" which means an inquiry for the current task of employee x
or
"T x y"which means the company assign task y to employee x.
(1<=x<=N,0<=y<=10^9)
Output

For each test case, print the test case number (beginning with 1) in the first line and then for every inquiry, output the correspond answer per line.
Sample Input

1 5 4 3 3 2 1 3 5 2 5 C 3 T 2 1 C 3 T 3 2 C 3
Sample Output

Case /#1: -1 1 2
Source

[2011 Multi-University Training Contest 14 - Host by FZU](http://acm.hdu.edu.cn/search.php?field=problem&key=2011+Multi-University+Training+Contest+14+-+Host+by+FZU&source=1&searchmode=source)