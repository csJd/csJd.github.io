---
title: HDU 1754 I Hate It(入门线段树)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2014-12-11 10:19:07
---
题意 中文

最基础的线段树了 只涉及到了点的修改

```js 
#include<cstdio>
#include<algorithm>
#define lc p<<1,s,mid
#define rc p<<1|1,mid+1,e
#define mid ((s+e)>>1)
using namespace std;
const int N = 200005;
int maxv[N << 2];

void pushup(int p)
{
    maxv[p] = max(maxv[p << 1], maxv[p << 1 | 1]);
}

void build(int p, int s, int e)
{
    if(s == e) scanf("%d", &maxv[p]);
    else
    {
        build(lc);
        build(rc);
        pushup(p);
    }
}

void update(int p, int s, int e, int a, int b)
{
    if(s == e && e == a)
    {
        maxv[p] = b;
        return;
    }
    if(a <= mid) update(lc, a, b);
    else update(rc, a, b);
    pushup(p);
}

int query(int p, int s, int e, int l, int r)
{
    if(s >= l && e <= r) return maxv[p];
    if(r <= mid) return query(lc, l, r);
    if(l > mid) return query(rc, l, r);
    return max(query(lc, l, mid), query(rc, mid + 1, r));
}

int main()
{
    int n, m, a, b;
    char c[2];
    while(~scanf("%d%d", &n, &m))
    {
        build(1, 1, n);
        while(m--)
        {
            scanf("%s%d%d", c, &a, &b);
            if(c[0] == 'Q') printf("%d\n", query(1, 1, n, a, b));
            else update(1, 1, n, a, b);
        }
    }
    return 0;
}
```

# I Hate It

Problem Description

很多学校流行一种比较的习惯。老师们很喜欢询问，从某某到某某当中，分数最高的是多少。
这让很多学生很反感。
不管你喜不喜欢，现在需要你做的是，就是按照老师的要求，写一个程序，模拟老师的询问。当然，老师有时候需要更新某位同学的成绩。
Input

本题目包含多组测试，请处理到文件结束。
在每个测试的第一行，有两个正整数 N 和 M ( 0<N<=200000,0<M<5000 )，分别代表学生的数目和操作的数目。
学生ID编号分别从1编到N。
第二行包含N个整数，代表这N个学生的初始成绩，其中第i个数代表ID为i的学生的成绩。
接下来有M行。每一行有一个字符 C (只取'Q'或'U') ，和两个正整数A，B。
当C为'Q'的时候，表示这是一条询问操作，它询问ID从A到B(包括A,B)的学生当中，成绩最高的是多少。
当C为'U'的时候，表示这是一条更新操作，要求把ID为A的学生的成绩更改为B。
Output

对于每一次询问操作，在一行里面输出最高成绩。
Sample Input

5 6 1 2 3 4 5 Q 1 5 U 3 6 Q 3 4 Q 4 5 U 2 9 Q 1 5
Sample Output

5 6 5 9

*Hint* Huge input,the C function scanf() will work better than cin