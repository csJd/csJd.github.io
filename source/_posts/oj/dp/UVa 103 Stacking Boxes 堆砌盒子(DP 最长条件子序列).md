---
title: UVa 103 Stacking Boxes 堆砌盒子(DP 最长条件子序列)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-22 10:26:02
---
题意 盒子a的n边都小于盒子b时 盒子a就可以放在盒子b上 求这样最多能堆多高

DAG中不固定顶点的最长路径，小白P162有讲解，d(i)=max{d(j)+1} j表示所有能装得下i的盒子

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
#define M 35
int a[M][M],d[M],pre[M],k,n,m;
using namespace std;

bool Isnest(int i,int j)
{
    sort(a[i]+1,a[i]+1+n);
    sort(a[j]+1,a[j]+1+n);
    for(int u=1;u<=n;++u)
        if(a[i][u]<=a[j][u]) return 0;
    return 1;
}

int dp(int i)
{
    if(d[i]>0) return d[i];
    d[i]=1;
    for(int j=1;j<=k;++j)
        if(Isnest(i,j)&&dp(j)+1>d[i])
        {
            d[i]=d[j]+1;
            pre[i]=j;
        }
    return d[i];
}

void print(int i)
{
    if(pre[i])
    {
        print(pre[i]);
        printf(" %d",i);
    }
    else
    {
        printf("%d",i);
        return;
    }
}
int main()
{
    while (scanf("%d%d",&k,&n)!=EOF)
    {
        for(int i=1;i<=k;++i)
            for(int j=1;j<=n;++j)
            scanf("%d",&a[i][j]);
        m=0;
        memset(pre,0,sizeof(pre));
        memset(d,0,sizeof(d));
        for(int i=1;i<=k;++i)
            if(dp(i)>dp(m))
                m=i;
        printf("%d\n",d[m]);
        print(m);
        printf("\n");
    }
    return 0;
}
```

#

## Background

Some concepts in Mathematics and Computer Science are simple in one or two dimensions but become more complex when extended to arbitrary dimensions. Consider solving differential equations in several dimensions and analyzing the topology of an n-dimensional hypercube. The former is much more complicated than its one dimensional relative while the latter bears a remarkable resemblance to its ``lower-class'' cousin.

## The Problem

Consider an n-dimensional ``box'' given by its dimensions. In two dimensions the box (2,3) might represent a box with length 2 units and width 3 units. In three dimensions the box (4,8,9) can represent a box ![tex2html_wrap_inline40](../images/dge.org-external-1-103img1.gif.png) (length, width, and height). In 6 dimensions it is, perhaps, unclear what the box (4,5,6,7,8,9) represents; but we can analyze properties of the box such as the sum of its dimensions.

In this problem you will analyze a property of a group of n-dimensional boxes. You are to determine the longest nesting string of boxes, that is a sequence of boxes ![tex2html_wrap_inline44](../images/dge.org-external-1-103img2.gif.png) such that each box ![tex2html_wrap_inline46](../images/dge.org-external-1-103img3.gif.png)nests in box ![tex2html_wrap_inline48](../images/dge.org-external-1-103img4.gif.png) ( ![tex2html_wrap_inline50](../images/dge.org-external-1-103img5.gif.png) .

A box D = ( ![tex2html_wrap_inline52](../images/dge.org-external-1-103img6.gif.png) ) nests in a box E = ( ![tex2html_wrap_inline54](../images/dge.org-external-1-103img7.gif.png) ) if there is some rearrangement of the ![tex2html_wrap_inline56](../images/dge.org-external-1-103img8.gif.png)such that when rearranged each dimension is less than the corresponding dimension in box E. This loosely corresponds to turning box D to see if it will fit in box E. However, since any rearrangement suffices, box D can be contorted, not just turned (see examples below).

For example, the box D = (2,6) nests in the box E = (7,3) since D can be rearranged as (6,2) so that each dimension is less than the corresponding dimension in E. The box D = (9,5,7,3) does NOT nest in the box E = (2,10,6,8) since no rearrangement of D results in a box that satisfies the nesting property, but F = (9,5,7,1) does nest in box E since F can be rearranged as (1,9,5,7) which nests in E.

Formally, we define nesting as follows: box D = ( ![tex2html_wrap_inline52](../images/dge.org-external-1-103img6.gif.png) ) nests in box E = ( ![tex2html_wrap_inline54](../images/dge.org-external-1-103img7.gif.png) ) if there is a permutation ![tex2html_wrap_inline62](../images/dge.org-external-1-103img9.gif.png) of ![tex2html_wrap_inline64](../images/dge.org-external-1-103img10.gif.png) such that ( ![tex2html_wrap_inline66](../images/dge.org-external-1-103img11.gif.png) ) ``fits'' in ( ![tex2html_wrap_inline54](../images/dge.org-external-1-103img7.gif.png) ) i.e., if![tex2html_wrap_inline70](../images/dge.org-external-1-103img12.gif.png) for all ![tex2html_wrap_inline72](../images/dge.org-external-1-103img13.gif.png) .

## The Input

The input consists of a series of box sequences. Each box sequence begins with a line consisting of the the number of boxes k in the sequence followed by the dimensionality of the boxes, n (on the same line.)

This line is followed by k lines, one line per box with the n measurements of each box on one line separated by one or more spaces. The ![tex2html_wrap_inline82](../images/dge.org-external-1-103img14.gif.png) line in the sequence ( ![tex2html_wrap_inline84](../images/dge.org-external-1-103img15.gif.png) ) gives the measurements for the ![tex2html_wrap_inline82](../images/dge.org-external-1-103img14.gif.png) box.

There may be several box sequences in the input file. Your program should process all of them and determine, for each sequence, which of the k boxes determine the longest nesting string and the length of that nesting string (the number of boxes in the string).

In this problem the maximum dimensionality is 10 and the minimum dimensionality is 1. The maximum number of boxes in a sequence is 30.

## The Output

For each box sequence in the input file, output the length of the longest nesting string on one line followed on the next line by a list of the boxes that comprise this string in order. The ``smallest'' or ``innermost'' box of the nesting string should be listed first, the next box (if there is one) should be listed second, etc.

The boxes should be numbered according to the order in which they appeared in the input file (first box is box 1, etc.).

If there is more than one longest nesting string then any one of them can be output.

## Sample Input

5 2 3 7 8 10 5 2 9 11 21 18 8 6 5 2 20 1 30 10 23 15 7 9 11 3 40 50 34 24 14 4 9 10 11 12 13 14 31 4 18 8 27 17 44 32 13 19 41 19 1 2 3 4 5 6 80 37 47 18 21 9

## Sample Output

5 3 1 2 4 5 4 7 2 5 6

﻿﻿