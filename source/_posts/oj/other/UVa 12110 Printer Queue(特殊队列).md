---
title: UVa 12110 Printer Queue(特殊队列)
author: Deng
tags: 
       - Algorithm

category: 
       - Other
       - OJ

date: 2015-01-22 13:15:38
---
题意 模拟打印队列 队列中有优先级大于队首的元素 队首元素就排到队尾 否则队首元素出队 输出开始在p位置的元素是第几个出队的

直接模拟这个过程就行了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 205;
int q[N];
int main()
{
    int cas, n, p, cnt, front, rear, i;
    scanf("%d", &cas);
    while(cas--)
    {
        scanf("%d%d", &n, &p);
        for(i = 0; i < n; ++i) scanf("%d", &q[i]);
        cnt = front = 0, rear = n;
        while(front <= p)
        {
            for(i = front + 1; i < rear; ++i)
                if(q[i] > q[front]) break;
            if(i < rear)
            {
                if(front == p) p = rear;
                q[rear++] = q[front];
            }
            else ++cnt;
            ++front;
        }
        printf("%d\n", cnt);
    }
    return 0;
}
```