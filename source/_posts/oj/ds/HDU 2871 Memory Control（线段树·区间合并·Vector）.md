---
title: HDU 2871 Memory Control（线段树·区间合并·Vector）
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-08-16 21:32:22
---
题意 模拟内存申请 有n个内存单元 有以下4种操作

Reset 将n个内存单元全部清空

New x 申请一个长度为x的连续内存块 申请成功就输出左端

Free x 将x所在的内存块空间释放 释放成功输出释放的内存始末位置

Get x 输出第x个内存块的起始位置

Reset 和 New 都是基本的区间合并知识 比较简单 Free和Get需要知道内层块的位置 所以我们在New的时候要将申请的内存块的始末位置都存起来 两个内层块不会相交 这样就能通过二分找到需要的内层块了 Free后要删去对应的内存块 用vector是为了使Get操作能在O(1)时间内完成 用List的话插入删除快一些但是Get就慢了 所以随便用什么来保存被占的内层块 相对来说vector比较容易实现

```js 
#include <cstdio>
#include <vector>
#include <algorithm>
#define lc p<<1, s, mid
#define lp p<<1
#define rc p<<1|1, mid + 1, e
#define rp p<<1|1
#define mid ((s+e)>>1)
using namespace std;
const int N = 50005;
int len[N << 2], lle[N << 2], lri[N << 2], setv[N << 2];

struct Block //内层块结构体
{
    int l, r;
    bool operator< (const Block &b) const{
        return l < b.l;
    }
} block;
vector<Block> vb;
vector<Block>::iterator it;

void pushup(int p, int s, int e)
{
    len[p] = max(len[lp], len[rp]);
    len[p] = max(len[p], lri[lp] + lle[rp]);
    lle[p] = lle[lp], lri[p] = lri[rp];
    if(lle[p] == mid - s + 1) lle[p] += lle[rp];
    if(lri[p] == e - mid)  lri[p] += lri[lp];
}

void pushdown(int p, int s, int e)
{
    if(setv[p] == -1) return;
    setv[lp] = setv[rp] = setv[p];
    len[lp] = lle[lp] = lri[lp] = setv[p] * (mid - s + 1);
    len[rp] = lle[rp] = lri[rp] = setv[p] * (e - mid);
    setv[p] = -1;
}

void build(int p, int s, int e)
{
    setv[p] = -1;
    if(s == e)
    {
        len[p] = lle[p] = lri[p] = 1;
        return;
    }
    build(lc);
    build(rc);
    pushup(p, s, e);
}

void update(int p, int s, int e, int l, int r, int v)
{
    if(l <= s && e <= r)
    {
        setv[p] = v;
        len[p] = lle[p] = lri[p] = v * (e - s + 1);
        return;
    }
    pushdown(p, s, e);
    if(l <= mid) update(lc, l, r, v);
    if(r > mid) update(rc, l, r, v);
    pushup(p, s, e);
}

int query(int p, int s, int e, int v)
{
    if(s == e) return s;
    pushdown(p, s, e);
    if(len[lp] >= v) return query(lc, v);
    if(lri[lp] + lle[rp] >= v) return mid + 1 - lri[lp];
    return query(rc, v);
}

int main()
{
    int n, m, x, p;
    char op[10];
    while(~scanf("%d%d", &n, &m))
    {
        build(1, 1, n);
        vb.clear();
        while(m--)
        {
            scanf("%s", op);

            if(op[0] == 'R') //Reset
            {
                update(1, 1, n, 1, n, 1); //用build会TLE
                vb.clear();
                puts("Reset Now");
            }

            if(op[0] == 'N')  //New
            {
                scanf("%d", &x);
                if(len[1] < x) puts("Reject New");
                else
                {
                    p = query(1, 1, n, x);
                    printf("New at %d\n", p);
                    update(1, 1, n, p, p + x - 1, 0);

                    block.l = p;
                    block.r = p + x - 1;
                    it = upper_bound(vb.begin(), vb.end(), block);
                    //找到第一个起点大于p的位置 确保插入后还是升序的
                    vb.insert(it, block);
                }
            }

            if(op[0] == 'G')  //Get
            {
                scanf("%d", &x);
                if(x > vb.size()) puts("Reject Get");
                else printf("Get at %d\n", vb[x - 1].l);
            }

            if(op[0] == 'F') //Free
            {
                scanf("%d", &x);
                block.l = block.r = x;
                it = upper_bound(vb.begin(), vb.end(), block);
                int i = it - vb.begin() - 1;
                if(i < 0 || vb[i].r < x) puts("Reject Free");
                else
                {
                    printf("Free from %d to %d\n", vb[i].l, vb[i].r);
                    update(1, 1, n, vb[i].l, vb[i].r, 1);
                    vb.erase(--it);
                }
            }
        }
        puts("");
    }
    return 0;
}
```

# Memory Control

Problem Description

Memory units are numbered from 1 up to N.
A sequence of memory units is called a memory block.
The memory control system we consider now has four kinds of operations:
1. Reset Reset all memory units free.
2. New x Allocate a memory block consisted of x continuous free memory units with the least start number
3. Free x Release the memory block which includes unit x
4. Get x Return the start number of the xth memory block(Note that we count the memory blocks allocated from left to right)
Where 1<=x<=N.You are request to find out the output for M operations.
Input

Input contains multiple cases.
Each test case starts with two integer N,M(1<=N,M<=50000) ,indicating that there are N units of memory and M operations.
Follow by M lines,each line contains one operation as describe above.
Output

For each “Reset” operation, output “Reset Now”.
For each “New” operation, if it’s possible to allocate a memory block,
output “New at A”,where Ais the least start number,otherwise output “Reject New”.
For each “Free” operation, if it’s possible to find a memory block occupy unit x,
output “Free from A to B”,where A and B refer to the start and end number of the memory block,otherwise output “Reject Free”.
For each “Get” operation, if it’s possible to find the xth memory blocks,
output “Get at A”,where A is its start number,otherwise output “Reject Get”.
Output one blank line after each test case.
Sample Input

6 10 New 2 New 5 New 2 New 2 Free 3 Get 1 Get 2 Get 3 Free 3 Reset
Sample Output

New at 1 Reject New New at 3 New at 5 Free from 3 to 4 Get at 1 Get at 5 Reject Get Reject Free Reset Now