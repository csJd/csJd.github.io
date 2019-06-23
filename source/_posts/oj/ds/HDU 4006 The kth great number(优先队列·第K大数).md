---
title: HDU 4006 The kth great number(优先队列·第K大数)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2015-04-14 13:05:56
---
题意 动态查询第K大的数

用小数在前优先队列维护K个数 每要插入一个数时 若这个数小于队首元素那么就不用插入了 否则队首元素出队 这个数入队 每次询问只用输出队首元素就行了

```js 
#include<cstdio>
#include<queue>
using namespace std;
int main()
{
    int n, a, k;
    char op[5];
    while(~scanf("%d%d", &n, &k))
    {
        priority_queue<int,vector<int>,greater<int> > pq;
        while(n--)
        {
            scanf("%s", op);
            if(op[0] == 'I')
            {
                scanf("%d",&a);
                if(pq.size()<k) pq.push(a);
                else if(a > pq.top()) pq.push(a),pq.pop();
            }
            else printf("%d\n", pq.top());
        }
    }
    return 0;
}
```

# The kth great number

Problem Description

Xiao Ming and Xiao Bao are playing a simple Numbers game. In a round Xiao Ming can choose to write down a number, or ask Xiao Bao what the kth great number is. Because the number written by Xiao Ming is too much, Xiao Bao is feeling giddy. Now, try to help Xiao Bao.
Input

There are several test cases. For each test case, the first line of input contains two positive integer n, k. Then n lines follow. If Xiao Ming choose to write down a number, there will be an " I" followed by a number that Xiao Ming will write down. If Xiao Ming choose to ask Xiao Bao, there will be a "Q", then you need to output the kth great number.
Output

The output consists of one integer representing the largest number of islands that all lie on one line.
Sample Input

8 3 I 1 I 2 I 3 Q I 5 Q I 4 Q
Sample Output

1 2 3

*Hint*Xiao Ming won't ask Xiao Bao the kth great number when the number of the written number is smaller than k. (1=<k<=n<=1000000).