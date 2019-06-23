---
title: UVa 10305 Ordering Tasks(拓扑排序)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2015-01-23 15:28:42
---
题意 输出n个数m组小于关系的一种可能的拓扑排序

应用dfs拓扑排序 访问j时 若存在i<j且i没被访问 就访问i

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 105;
int n, m, t, v[N], tpo[N], g[N][N];

void dfs(int j)
{
    if(v[j]) return;
    for(int i = 1; i <= n; ++i)
        if(g[i][j]) dfs(i);
    v[j] = 1;
    tpo[++t] = j;
}

int main()
{
    int a, b, i;
    while(~scanf("%d %d", &n, &m), n)
    {
        memset(g, 0, sizeof(g));
        memset(v, 0, sizeof(v));
        while(m--)
        {
            scanf("%d%d", &a, &b);
            g[a][b] = 1;
        }

        t = 0;
        for(i = 1; i <= n; ++i)
            if(!v[i]) dfs(i);
        for(i = 1; i < n; ++i)
            printf("%d ", tpo[i]);
        printf("%d\n", tpo[n]);
    }
    return 0;
}
```

John has n tasks to do. Unfortunately, the tasks are not independent and the execution of one task is only possible if other tasks have already been executed.

**Input**

The input will consist of several instances of the problem. Each instance begins with a line containing two integers, **1 <= n <= 100** and**m**. **n** is the number of tasks (numbered from **1** to **n**) and **m** is the number of direct precedence relations between tasks. After this, there will be **m** lines with two integers **i** and **j**, representing the fact that task **i** must be executed before task **j**. An instance with **n = m = 0**will finish the input.

**Output**

5 4 1 2 2 3 1 3 1 5 0 0

1 4 2 5 3