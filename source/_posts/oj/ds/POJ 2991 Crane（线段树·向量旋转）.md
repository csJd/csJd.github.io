---
title: POJ 2991 Crane（线段树·向量旋转）
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-08-16 15:16:48
---
题意 有一个Crane由n条线段连接组成 每个连接点处均可以任意旋转 给你n条线段的长度 然后又m次旋转操作 给你p和r 将第p和第p+1条线段之间的角度旋转为r 即第p条线段绕p的终点逆时针旋转r度后能够与第p+1条重合 问每次旋转后最后一条线段的终点坐标

可以发现 旋转第p+1条线段时 p+1后面的所有线段也一起旋转了 可以把Crane分解为n个向量 这些向量的和也就是Crane终点的坐标 用rad[i]保存第i个向量与第i+1个向量之间的夹角 每次旋转操作时 我们要把rad[i]变为r 也就是要把第i+1和后面的所有向量逆时针旋转r - rad[i] 对于向量的旋转操作有

**(x,y)**逆时针旋转 r 弧度 -->**(x/*cos(r) - y/*sin(r), x/*sin(r) + y/*cos(r))**

**(x,y)**顺时针旋转 r 弧度 -->**(-x/*cos(r) + y/*sin(r), x/*sin(r) + y/*cos(r))**

那么我们可以用线段树来维护对应区间的向量的和 根节点对应的x, y也就是答案了

```js 
#include <cstdio>
#include <cmath>
#define lc p<<1, s, mid
#define rc p<<1|1, mid+1, e
#define mid ((s+e)>>1)
using namespace std;
const int N = 10005;
const double pi = acos(-1.0);
const double eps = 1e-8;
double x[N << 2], y[N << 2], rot[N << 2];
//x 对应区间所有向量的和的横坐标
//y 对应区间所有向量的和的纵坐标
//rot 子节点对应区间所有向量需要逆时针旋转的弧度
double rad[N]; //rad[i] 保存第i个向量和第i+1个向量间的弧度

void rotate(double &x, double &y, double r)
{
    //将向量(x, y)逆时针旋转r弧度
    double xx = x;
    x = xx * cos(r) - y * sin(r);
    y = xx * sin(r) + y * cos(r);
}

void pushup(int p)
{
    x[p] = x[p << 1] + x[p << 1 | 1];
    y[p] = y[p << 1] + y[p << 1 | 1];
}

void pushdown(int p, int s, int e)
{
    if(fabs(rot[p]) < eps) return;
    int lp = p << 1, rp = p << 1 | 1;
    rot[lp] += rot[p];
    rot[rp] += rot[p];
    rotate(x[lp], y[lp], rot[p]);
    rotate(x[rp], y[rp], rot[p]);
    rot[p] = 0;
}

void build(int p, int s, int e)
{
    rot[p] = 0;
    if(s == e)
    {
        scanf("%lf", &y[p]);
        x[p] = 0;
        rad[s] = pi;
        return;
    }
    build(lc);
    build(rc);
    pushup(p);
}

void update(int p, int s, int e, int l, int r, double v)
{
    if(l <= s && e <= r)
    {
        rot[p] += v;
        rotate(x[p], y[p], v);
        return;
    }
    pushdown(p, s, e);
    if(l <= mid) update(lc, l, r, v);
    if(r > mid) update(rc, l, r, v);
    pushup(p);
}

int main()
{
    int n, m, p, first = 1;
    double r;
    while(~scanf("%d%d", &n, &m))
    {
        if(!first) puts("");
        else first = 0;
        build(1, 1, n);
        while(m--)
        {
            scanf("%d%lf", &p, &r);
            r = r / 180 * pi;
            //p与p+1之间的弧度为rad[p] 要达到r p+1还需要逆时针旋转r - rad[p]弧度
            update(1, 1, n, p + 1, n, r - rad[p]);
            rad[p] = r;  //旋转后p与p+1之间的弧度变为r
            printf("%f %f\n", x[1], y[1]); //x[1], y[1]为所有向量和的坐标
        }
    }
    return 0;
}
```

Crane

Description
ACM has bought a new crane (crane -- jeřáb) . The crane consists of n segments of various lengths, connected by flexible joints. The end of the i-th segment is joined to the beginning of the i + 1-th one, for 1 ≤ i < n. The beginning of the first segment is fixed at point with coordinates (0, 0) and its end at point with coordinates (0, w), where w is the length of the first segment. All of the segments lie always in one plane, and the joints allow arbitrary rotation in that plane. After series of unpleasant accidents, it was decided that software that controls the crane must contain a piece of code that constantly checks the position of the end of crane, and stops the crane if a collision should happen.
Your task is to write a part of this software that determines the position of the end of the n-th segment after each command. The state of the crane is determined by the angles between consecutive segments. Initially, all of the angles are straight, i.e., 180 o. The operator issues commands that change the angle in exactly one joint.

Input

The input consists of several instances, separated by single empty lines.
The first line of each instance consists of two integers 1 ≤ n ≤10 000 and c 0 separated by a single space -- the number of segments of the crane and the number of commands. The second line consists of n integers l1,..., ln (1 li 100) separated by single spaces. The length of the i-th segment of the crane is li. The following c lines specify the commands of the operator. Each line describing the command consists of two integers s and a (1 ≤ s < n, 0 ≤ a ≤ 359) separated by a single space -- the order to change the angle between the s-th and the s + 1-th segment to a degrees (the angle is measured counterclockwise from the s-th to the s + 1-th segment).

Output

The output for each instance consists of c lines. The i-th of the lines consists of two rational numbers x and y separated by a single space -- the coordinates of the end of the n-th segment after the i-th command, rounded to two digits after the decimal point.
The outputs for each two consecutive instances must be separated by a single empty line.

Sample Input

2 1 10 5 1 90 3 2 5 5 5 1 270 2 90

Sample Output

5.00 10.00 -10.00 5.00 -5.00 10.00