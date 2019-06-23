---
title: POJ 2823 Sliding Window(单调队列)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2015-07-06 00:28:21
---
题意 长度为n的数组上有个长度为k的滑窗从左向右移动 求每次移动后滑窗区间的最小值和最大值 输出两行 第一行所有最小值 第二行所有最大值

可以用线段树来做 但是单调队列更简单

单调递增队列： 队尾单调入队(入队元素大于队尾元素时直接入队 否则队尾出队直到队尾元素小于入队元素或者队列为空) 队首队尾都可以出队

求最小值时 先判断队首元素是否在滑窗之内 不在队首就出队 然后队首元素就是滑窗中的最小值了 求最大值用单减队列就行了

```js 
#include <cstdio>
using namespace std;
const int N = 1e6 + 5;
int a[N], q[N], t[N];
int front, rear, n, k;

#define NOTMONO (!op && a[i] < q[rear - 1]) || (op && a[i] > q[rear - 1])
void getMonoQueue(int op) //op = 0 时单增队列  op = 1 时单减队列
{
    front = rear = 0;
    for(int i = 0; i < n; ++i)
    {
        while( rear > front && (NOTMONO)) --rear;
        t[rear] = i;      //记录滑窗滑到i点的时间
        q[rear++] = a[i];
        while(t[front] <= i - k) ++front;  //保证队首元素在滑窗之内
        if(i > k - 2)
            printf("%d%c", q[front], i == n - 1 ? '\n' : ' ');
    }
}

int main()
{
    while (~scanf("%d%d", &n, &k))
    {
        for(int i = 0; i < n; ++i)
            scanf("%d", &a[i]);
        getMonoQueue(0); //单增队列维护最小值
        getMonoQueue(1); //单减队列维护最大值
    }

    return 0;
}

//Last modified :   2015-07-06 12:16
```

Sliding Window

Description
An array of size *n* ≤ 10 6 is given to you. There is a sliding window of size *k* which is moving from the very left of the array to the very right. You can only see the *k* numbers in the window. Each time the sliding window moves rightwards by one position. Following is an example:
The array is [1 3 -1 -3 5 3 6 7], and *k* is 3. Window position Minimum value Maximum value [1 3 -1] -3 5 3 6 7 -1 3 1 [3 -1 -3] 5 3 6 7 -3 3 1 3 [-1 -3 5] 3 6 7 -3 5 1 3 -1 [-3 5 3] 6 7 -3 5 1 3 -1 -3 [5 3 6] 7 3 6 1 3 -1 -3 5 [3 6 7] 3 7

Your task is to determine the maximum and minimum values in the sliding window at each position.

Input

The input consists of two lines. The first line contains two integers *n* and *k* which are the lengths of the array and the sliding window. There are *n* integers in the second line.

Output

There are two lines in the output. The first line gives the minimum values in the window at each position, from left to right, respectively. The second line gives the maximum values.

Sample Input

8 3 1 3 -1 -3 5 3 6 7

Sample Output

-1 -3 -3 -3 3 3 3 3 5 5 6 7