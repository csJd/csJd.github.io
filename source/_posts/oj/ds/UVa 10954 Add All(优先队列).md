---
title: UVa 10954 Add All(优先队列)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2014-11-17 15:14:08
---
题意 求把所有数加起来的最小代价a+b的代价为(a+b)

越先运算的数 要被加的次数越多 所以每次相加的两个数都应该是剩下序列中最小的数 然后结果要放到序列中 也就是优先队列了

```js 
#include<cstdio>
#include<queue>
using namespace std;
priority_queue<int, vector<int>, greater<int> >q;
typedef long long ll;
ll ans;
int main()
{
    int n, t;
    while(scanf("%d", &n), n)
    {
        for(int i = 0; i < n; ++i)
        {
            scanf("%d", &t);
            q.push(t);
        }

        ans = 0;
        while(!q.empty())
        {
            int a = q.top();      q.pop();
            int b = q.top();      q.pop();
            ans += (a + b);
            if(q.empty()) break;
            q.push(a + b);
        }
        printf("%lld\n", ans);
   }
    return 0;
}
```

Yup!! The problem name reflects your task; just add a set of numbers. But you may feel yourselves condescended, to write a C/C++ program just to add a set of numbers. Such a problem will simply question your erudition. So, let’s add some flavor of ingenuity to it.

Addition operation requires cost now, and the cost is the summation of those two to be added. So, to add **1** and **10**, you need a cost of**11**. If you want to add **1**, **2** and **3**. There are several ways –

1 + 2 = 3, cost = 3

3 + 3 = 6, cost = 6

Total = 9
 
1 + 3 = 4, cost = 4

2 + 4 = 6, cost = 6

Total = 10
 
2 + 3 = 5, cost = 5

1 + 5 = 6, cost = 6

Total = 11

I hope you have understood already your mission, to add a set of integers so that the cost is minimal.

## Input

Each test case will start with a positive number, **N (2 ≤ N ≤ 5000)** followed by **N** positive integers (all are less than **100000**). Input is terminated by a case where the value of **N** is zero. This case should not be processed.

## Output

For each case print the minimum total cost of addition in a single line.

# **Sample Input Output for Sample Input**

**3**

**1 2 3**

**4**

**1 2 3 4**

**0**
**** 
**9**

**19**

****
****