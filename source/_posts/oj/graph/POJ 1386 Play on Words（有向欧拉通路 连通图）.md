---
title: POJ 1386 Play on Words（有向欧拉通路 连通图）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-11-12 07:59:08
---
题意 见下方中文翻译

每个单词可以看成首尾两个字母相连的一条边 然后就是输入m条边 判断能否构成有向欧拉通路了

有向图存在欧拉通路的充要条件：

1. 有向图的基图连通；

2. 所有点的出度和入度相等或者只有两个入度和出度不相等的点 且这两点入度与出度的差一个为-1（起点）一个为1（终点）.

判断是否连通就是应用并查集了

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 30, M = 100010;
struct edge{int u, v; } e[M];
int vis[N], in[N], out[N], par[N], m, ok;

int Find(int x)
{
    int r = x, tmp;
    while(par[r] >= 0) r = par[r];
    while(x != r)
    {
        tmp = par[x];
        par[x] = r;
        x = tmp;
    }
    return r;
}

void Union(int u, int v)
{
    int ru = Find(u), rv = Find(v), tmp = par[ru] + par[rv];
    if(par[ru] < par[rv]) par[rv] = ru, par[ru] = tmp;
    else par[ru] = rv, par[rv] = tmp;
}

void connect()
{
    memset(par, -1, sizeof(par)); //初始化并查集
    for(int i = 0; i < m; ++i)
    {
        int u = e[i].u, v = e[i].v;
        if(Find(u) != Find(v)) Union(u, v);
    }
    for(int i = 0; i < 26; ++i)
        for(int j = 0; j < 26; ++j)
            if(vis[i] && vis[j] && Find(i) != Find(j)) ok = 0;
}

int main()
{
    char s[1005];
    int u, v, cas;
    scanf("%d", &cas);
    while(cas--)
    {
        for(int i = 0; i < 26; ++i)
            vis[i] = in[i] = out[i] = 0;
        scanf("%d", &m);
        for(int i = 0; i < m; ++i)
        {
            scanf("%s", s);
            u = s[0] - 'a', v = s[strlen(s) - 1] - 'a';
            vis[u] = vis[v] = 1;
            e[i].u = u, e[i].v = v;
            ++in[u], ++out[v];
        }

        int id = 0, od = 0;//i[d]记录入度比出度大1的点的个数 o[d]小1
        ok = 1;
        for(int i = 0; i < 26; ++i)
        {
            if(!vis[i]) continue;
            int k = in[i] - out[i];
            if(k < -1 || k > 1) {ok = 0; break;}
            if(k == 1) ++id;
            if(k == -1) ++od;
        }
        if(id > 1 || od > 1 || id - od) ok = 0;
        connect();
        if(ok)  printf("Ordering is possible.\n");
        else printf("The door cannot be opened.\n");
    }
    return 0;
}
```
 题目描述：
有些秘门带有一个有趣的词迷。考古学家必须解开词迷才能打开门。由于没有其他方法可以 打开门，因此词迷就变得很重要。 每个门上有许多磁盘。每个盘上有一个单词，这些磁盘必须重新排列使得每个单词第一个字 母跟前一个单词后一个字母相同。例如单词"acm"可以跟在单词"motorola"的后面。你的任务是 编写一个程序，读入一组单词，然后判定是否可以经过重组使得每个单词第一个字母跟前一个单 词后一个字母相同，这样才能打开门。

输入描述：

输入文件中包含 T 个测试数据。输入文件的第一行就是 T，接下来是 T 个测试数据。每个测 试数据的第一行是一个整数 N，表示单词的个数（1≤N≤100000）；接下来有 N行，每行是一个 单词；每个单词至少有 2个、至多有 1000 个小写字母，即单词中只可能出现字母'a'～'z'；在同一 个测试数据中，一个单词可能出现多次。

输出描述：

如果通过重组单词可以达到要求，输出"Ordering is possible."，否则输出"The door cannot be opened."。

Sample Input
3 2 acm ibm 3 acm malform mouse 2 ok ok

Sample Output

The door cannot be opened. Ordering is possible. The door cannot be opened.