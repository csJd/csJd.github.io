---
title: POJ 2828 Buy Tickets(线段树·插队)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2015-07-13 00:25:10
---
题意 n个人排队 每个人都有个属性值 依次输入n个pos[i] val[i] 表示第i个人直接插到当前第pos[i]个人后面 他的属性值为val[i] 要求最后依次输出队中各个人的属性值

从头到尾看的话 队列是动态的 无法操作 但是反过来看时 pos[i]就可以表示第i个人前面有多少个空位了 然后想到了用线段树做就简单了 线段树维护对应区间还有多少个空位 每次把i放到前面刚好有pos[i]个空位的位置就行了 具体看代码

```js 
#include <cstdio>
#define lc p<<1, s, mid
#define rc p<<1|1, mid+1, e
#define mid ((s+e)>>1)

using namespace std;
const int N = 2e5 + 5;
int tot[N * 4], ans[N];
//tot维护对应区间还能放多少人
int pos[N], val[N];
//pos[i] 保存第i个人进队时前面有多少人
//val[i] 保存第i个人的val

void pushup(int p)
{
    tot[p] = tot[p << 1] + tot[p << 1 | 1];
}

void build(int p, int s, int e)
{
    //tot维护对应区间还能放多少人
    if(s == e)
    {
        tot[p] = 1;
        return;
    }
    build(lc);
    build(rc);
    pushup(p);
}

//第i个人插入
void update(int p, int s, int e, int i)
{
    if(s == e)
    {
        tot[p] = 0;
        ans[e] = val[i];
        return;
    }
    if(tot[p << 1] > pos[i])
        update(lc, i);  //左区间的空位足够
    else
    {
        pos[i] -= tot[p << 1];
        update(rc, i);
    }
    pushup(p);
}

int main()
{
    int n;
    while(~scanf("%d", &n))
    {
        build(1, 1, n);
        for(int i = 1; i <= n; ++i)
            scanf("%d%d", &pos[i], &val[i]);

        for(int i = n; i > 0; --i) //倒着更新
            update(1, 1, n, i);

        for(int i = 1; i < n; ++i)
            printf("%d ", ans[i]);

        printf("%d\n", ans[n]);
    }

    return 0;
}
//Last modified :   2015-07-13 11:13
```