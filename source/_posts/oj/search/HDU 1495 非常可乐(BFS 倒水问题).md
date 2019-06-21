---
title: HDU 1495 非常可乐(BFS 倒水问题)
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2015-04-01 09:39:11
---
题意 将体积为s的可乐 利用容积分别为n和m的两个杯子平均分为两份 至少需要倒多少次可乐

可以把容器s,n,m中装的可乐量看成一种状态

容器都是没有刻度的 所以每次倒可乐要么把自己倒完 要么把对方倒满

每种状态可以通过一次倒水到达哪些状态 于是可以通过bfs判断到达每种状态需要倒多少次

3个容器中有一个装的可乐为s/2的状态就是答案了 s是奇数时明显不可能平分的 可以直接忽略

```js 
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 105;
int v[N][N][N], n, m, s, le, ri;

struct state {
    int a, b, c, d;
    state() {}
    state(int e, int f, int g, int h)
        : a(e), b(f), c(g), d(h) {}
} q[N * N * N];

void pour(int a, int b, int c, int d)
{
    if(!v[a][b][c])
    {
        v[a][b][c] = 1;
        q[ri++] = state(a, b, c, d + 1);
    }
}

int bfs()
{
    int a, b, c, d;
    le = ri = 0;
    q[ri++] = state(s, 0, 0, 0);
    memset(v, 0, sizeof(v));
    v[s][0][0] = 1;
    while(le < ri)
    {
        a = q[le].a, b = q[le].b, c = q[le].c, d = q[le++].d;
        if(a == s / 2 || b == s / 2 || c == s / 2)
            return d + (a && b && c != 0);
        pour(a - n + b, n, c, d);		    //s->n:
        pour(a - m + c, b, m, d);		    //s->m;
        pour(a + b, 0, c, d);			    //n->s;
        pour(a + c, b, 0, d);                       //m->s;
        if(b > m - c) pour(a, b - m + c, m, d);     //n->m
        else pour(a, 0, b + c, d);
        if(c > n - b) pour(a, n, c - n + b, d);     //m->n
        else pour(a, b + c, 0, d);
    }
    return 0;
}

int main()
{
    int ans;
    while(scanf("%d%d%d", &s, &n, &m), n)
    {
        ans = 0;
        if (s % 2 == 0) ans = bfs();
        if(!ans) puts("NO");
        else printf("%d\n", ans);
    }
    return 0;
}
```

# 非常可乐

Problem Description

大家一定觉的运动以后喝可乐是一件很惬意的事情，但是seeyou却不这么认为。因为每次当seeyou买了可乐以后，阿牛就要求和seeyou一起分享这一瓶可乐，而且一定要喝的和seeyou一样多。但seeyou的手中只有两个杯子，它们的容量分别是N 毫升和M 毫升 可乐的体积为S （S<101）毫升　(正好装满一瓶) ，它们三个之间可以相互倒可乐 (都是没有刻度的，且 S==N+M，101＞S＞0，N＞0，M＞0) 。聪明的ACMER你们说他们能平分吗？如果能请输出倒可乐的最少的次数，如果不能输出"NO"。
Input

三个整数 : S 可乐的体积 , N 和 M是两个杯子的容量，以"0 0 0"结束。
Output

如果能平分的话请输出最少要倒的次数，否则输出"NO"。
Sample Input

7 4 3 4 1 3 0 0 0
Sample Output

NO 3