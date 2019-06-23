---
title: HDU 1069 Monkey and Banana(DP 长方体堆放问题)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:51:18
---
题意 给你n种长方体 每种都有无穷个 三条棱长为a,b,c 当一个长方体的长宽都小于另一个时 这个长方体就可以堆在另一个上面 求这些长方体能堆起的最大高度

每个长方体都有6种放置方式 但只有三种高度 分别为a,b,c 为了便于操坐 可以把一个长方体分为三个 每个的高度都是唯一的 然后就可以用最长连通来求了 令d[i]表示以第i个长方体为最顶上一个时的最大高度 当第i个长方体的长和宽小于第j个的长和宽或宽和长时 第i个就可以放在第j个上面 即 d[i]=max(d[i],d[j]+a[i].h)

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 35 * 3;
int d[N], n;
struct Cube
{
    int a, b, c;
    Cube (int aa = 0, int bb = 0, int cc = 0) : a (aa), b (bb), c (cc) {}
} cub[N];

int dp (int i)
{
    if (d[i] > 0) return d[i];
    d[i] = cub[i].c;
    for (int j = 1; j <= 3 * n; ++j)
        if ( (cub[i].a < cub[j].a && cub[i].b < cub[j].b) || (cub[i].a < cub[j].b && cub[i].b < cub[j].a))
            d[i] = max (d[i], dp (j) + cub[i].c);
    return d[i];
}

int main()
{
    int cas = 0, ans, a, b, c;
    while (scanf ("%d", &n), n)
    {
        memset (d, 0, sizeof (d));
        for (int i = ans = 0; i < n; ++i)
        {
            scanf ("%d%d%d", &a, &b, &c);
            cub[3 * i + 1] = Cube (a, b, c);
            cub[3 * i + 2] = Cube (a, c, b);
            cub[3 * i + 3] = Cube (b, c, a);
        }
        for (int i = 1; i <= 3 * n; ++i)
            ans = max (ans, dp (i));
        printf ("Case %d: maximum height = %d\n", ++cas, ans);
    }
    return 0;
}
```

# Monkey and Banana

Problem Description

A group of researchers are designing an experiment to test the IQ of a monkey. They will hang a banana at the roof of a building, and at the mean time, provide the monkey with some blocks. If the monkey is clever enough, it shall be able to reach the banana by placing one block on the top another to build a tower and climb up to get its favorite food.
The researchers have n types of blocks, and an unlimited supply of blocks of each type. Each type-i block was a rectangular solid with linear dimensions (xi, yi, zi). A block could be reoriented so that any two of its three dimensions determined the dimensions of the base and the other dimension was the height.
They want to make sure that the tallest tower possible by stacking blocks can reach the roof. The problem is that, in building a tower, one block could only be placed on top of another block as long as the two base dimensions of the upper block were both strictly smaller than the corresponding base dimensions of the lower block because there has to be some space for the monkey to step on. This meant, for example, that blocks oriented to have equal-sized bases couldn't be stacked.
Your job is to write a program that determines the height of the tallest tower the monkey can build with a given set of blocks.
Input

The input file will contain one or more test cases. The first line of each test case contains an integer n,
representing the number of different blocks in the following data set. The maximum value for n is 30.
Each of the next n lines contains three integers representing the values xi, yi and zi.
Input is terminated by a value of zero (0) for n.
Output

For each test case, print one line containing the case number (they are numbered sequentially starting from 1) and the height of the tallest possible tower in the format "Case case: maximum height = height".
Sample Input

1 10 20 30 2 6 8 10 5 5 5 7 1 1 1 2 2 2 3 3 3 4 4 4 5 5 5 6 6 6 7 7 7 5 31 41 59 26 53 58 97 93 23 84 62 64 33 83 27 0
Sample Output

Case 1: maximum height = 40 Case 2: maximum height = 21 Case 3: maximum height = 28 Case 4: maximum height = 342