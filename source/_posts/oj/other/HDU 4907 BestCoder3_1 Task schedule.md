---
title: HDU 4907 BestCoder3_1 Task schedule
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Other

date: 2014-09-02 09:50:34
---
# Task schedule

Time Limit: 2000/1000 MS (Java/Others) Memory Limit: 32768/32768 K (Java/Others)
Total Submission(s): 0 Accepted Submission(s): 0
Problem Description

有一台机器，并且给你这台机器的工作表，工作表上有n个任务，机器在ti时间执行第i个任务，1秒即可完成1个任务。
有m个询问，每个询问有一个数字q，表示如果在q时间有一个工作表之外的任务请求，请计算何时这个任务才能被执行。
机器总是按照工作表执行，当机器空闲时立即执行工作表之外的任务请求。
Input

输入的第一行包含一个整数T， 表示一共有T组测试数据。
对于每组测试数据：
第一行是两个数字n, m，表示工作表里面有n个任务, 有m个询问；
第二行是n个不同的数字t1, t2, t3....tn，表示机器在ti时间执行第i个任务。
接下来m行，每一行有一个数字q，表示在q时间有一个工作表之外的任务请求。
特别提醒：m个询问之间是无关的。
[Technical Specification]
1. T <= 50
2. 1 <= n, m <= 10^5
3. 1 <= ti <= 2/*10^5, 1 <= i <= n
4. 1 <= q <= 2/*10^5
Output

对于每一个询问，请计算并输出该任务何时才能被执行，每个询问输出一行。
Sample Input

1 5 5 1 2 3 5 6 1 2 3 4 5
Sample Output

4 4 4 4 7

```js 
//从前往后直接暴力会超时
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 2e5 + 10;
int busy[N], ans[N], m, n, t, cas, q;
int main()
{
    scanf ("%d", &cas);
    while (cas--)
    {
        scanf ("%d%d", &n, &m);
        memset (busy, 0, sizeof (busy));
        for (int i = 1; i <= n; ++i)
            scanf ("%d", &t), busy[t] = 1;
        ans[200001] = 200001;
        for (int i = 200000; i >= 1; --i)
            if (busy[i]) ans[i] = ans[i + 1];
            else ans[i] = i;
        while (m--)
            scanf ("%d", &q), printf ("%d\n", ans[q]);
    }
    return 0;
}
```