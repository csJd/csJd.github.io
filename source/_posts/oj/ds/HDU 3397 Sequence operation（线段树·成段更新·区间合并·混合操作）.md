---
title: HDU 3397 Sequence operation（线段树·成段更新·区间合并·混合操作）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2015-08-12 10:12:46
---
题意 给你一个只有0, 1的数组 有这些操作

0. 将[a, b]区间的所有数都改为0

1. 将[a, b]区间的所有数都改为1

2. 将[a, b]区间的所有数都取反 即与1异或

3. 输出区间[a, b]中1的个数 即所有数的和

4. 输出区间[a, b]中最大连续1的长度

对于所有的3, 4操作输出对应的答案

单个的操作都很简单 但搞在一起就有点恶心了 还好数组里的数只有0和1

线段树维护9个值 对应区间0, 1的最大长度len[i] 对应区间左端点为起点的最大0, 1长度lle[i] 对应区间右端点为终点的最大0, 1长度 对应区间的1的个数sum 对应区间的置值标记setv 对应区间的取反标记opp

成段更新时有两种操作 取反 (opp) 和置值 (setv) 取反操作时因为是0, 1互换 将维护的对应的信息也交换就行了 sum就变成长度减去sum了 置值操作比较简单 但是要注意置值操作前的所有取反操作都是没有意义的 所以置值的时候要将对应区间的取反标记去掉

```js 
#include <cstdio>
#include <algorithm>
#define lc p<<1, s, mid
#define rc p<<1|1, mid+1, e
#define lp p<<1
#define rp p<<1|1
#define mid ((s+e)>>1)
using namespace std;
const int N = 100005, M = N * 4;
//最大连续i的长度 左端连续长度 右端连续长度
int len[2][M], lle[2][M], lri[2][M];
int opp[M], setv[M], sum[M];

void swap01(int p)  //将p对应区间的01信息交换
{
    swap(len[0][p], len[1][p]);
    swap(lle[0][p], lle[1][p]);
    swap(lri[0][p], lri[1][p]);
}

void setAll(int p, int i, int v) //将p对应的区间全部置为i
{
    int j = 1 - i;
    len[i][p] = lle[i][p] = lri[i][p] = v;
    len[j][p] = lle[j][p] = lri[j][p] = 0;
}

void pushup(int p, int s, int e)
{
    sum[p] = sum[lp] + sum[rp];
    for(int i = 0; i < 2; ++i)
    {
        len[i][p] = max(len[i][lp], len[i][rp]);
        lle[i][p] = lle[i][lp], lri[i][p] = lri[i][rp];
        if(lri[i][lp] && lle[i][rp]) //左右孩子边界可连接
        {
            len[i][p] = max(len[i][p], lri[i][lp] + lle[i][rp]);
            if(lle[i][lp] == mid + 1 - s) lle[i][p] += lle[i][rp];
            if(lri[i][rp] == e - mid) lri[i][p] += lri[i][lp];
        }
    }
}

void build(int p, int s, int e)
{
    opp[p] = 0;
    setv[p] = -1;
    if(s == e)
    {
        scanf("%d", &sum[p]);
        int i = sum[p];
        setAll(p, i, 1);
        return;
    }
    build(lc);
    build(rc);
    pushup(p, s, e);
}

void pushdown(int p, int s, int e)
{
    if(setv[p] != -1)
    {
        int i = setv[lp] = setv[rp] = setv[p];
        opp[lp] = opp[rp] = 0;
        sum[lp] = i * (mid + 1 - s);
        sum[rp] = i * (e - mid);

        setAll(lp, i, mid + 1 - s);
        setAll(rp, i, e-mid);
        setv[p] = -1;
    }

    if(opp[p])
    {
        opp[lp] ^= 1;
        opp[rp] ^= 1;
        sum[lp] = mid + 1 - s - sum[lp];
        sum[rp] = e - mid - sum[rp];

        swap01(lp);
        swap01(rp);
        opp[p] = 0;
    }
}

void update(int p, int s, int e, int l, int r, int v)
{
    if(l <= s && e <= r)
    {
        if(v == 2)
        {
            opp[p] ^= 1;
            sum[p] = e - s + 1 - sum[p];
            swap01(p);

        }
        else
        {
            int i = setv[p] = v;
            opp[p] = 0;
            sum[p] = v * (e - s + 1);
            setAll(p, i, e - s + 1);
        }
        return;
    }
    pushdown(p, s, e);
    if(l <= mid) update(lc, l, r, v);
    if(r > mid) update(rc, l, r, v);
    pushup(p, s, e);
}

int query(int p, int s, int e, int l, int r, int op)
{
    if(l <= s && e <= r) return op == 3 ? sum[p] : len[1][p];
    pushdown(p, s, e);
    int ret = 0;
    if(op == 3)  //the number of '1's
    {
        if(l <= mid) ret += query(lc, l, r, op);
        if(r > mid) ret += query(rc, l, r, op);
    }
    else  // the length of '1's
    {
        ret = max(ret, min(r, mid + lle[1][rp]) - max(l, mid + 1 - lri[1][lp]) + 1);
        if(l <= mid) ret = max(ret, query(lc, l, r, op));
        if(r > mid) ret = max(ret, query(rc, l, r, op));
    }
    return ret;
}

int main()
{
    int T, n, m, a, b, op;
    scanf("%d", &T);
    while(T--)
    {
        scanf("%d%d", &n, &m);
        build(1, 0, n - 1);
        while(m--)
        {
            scanf("%d%d%d", &op, &a, &b);
            if(op < 3) update(1, 0, n - 1, a, b, op);
            else
                printf("%d\n", query(1, 0, n - 1, a, b, op));
        }
    }
    return 0;
}
//Last modified :  2015-08-12 09:10 CST
```

# Sequence operation

Problem Description

lxhgww got a sequence contains n characters which are all '0's or '1's.
We have five operations here:
Change operations:
0 a b change all characters into '0's in [a , b]
1 a b change all characters into '1's in [a , b]
2 a b change all '0's into '1's and change all '1's into '0's in [a, b]
Output operations:
3 a b output the number of '1's in [a, b]
4 a b output the length of the longest continuous '1' string in [a , b]
Input

T(T<=10) in the first line is the case number.
Each case has two integers in the first line: n and m (1 <= n , m <= 100000).
The next line contains n characters, '0' or '1' separated by spaces.
Then m lines are the operations:
op a b: 0 <= op <= 4 , 0 <= a <= b < n.
Output

For each output operation , output the result.
Sample Input

1 10 10 0 0 0 1 1 0 1 0 1 1 1 0 2 3 0 5 2 2 2 4 0 4 0 3 6 2 3 7 4 2 8 1 0 5 0 5 6 3 3 9
Sample Output

5 2 6 5