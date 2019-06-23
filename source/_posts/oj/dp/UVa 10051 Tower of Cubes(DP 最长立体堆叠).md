---
title: UVa 10051 Tower of Cubes(DP 最长立体堆叠)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-26 08:39:25
---
﻿﻿

题意 给你n个立方体 立方体每面都涂有颜色 当一个立方体序号小于另一个立方体且这个立方体底面的颜色等于另一个立方体顶面的颜色 这个立方体就可以放在另一个立方体上面 求这些立方体堆起来的最大高度；

每个立方体有6种放置方式 为了便于控制 个人喜欢将一个立方体分解为6个 这样每个立方体只用考虑顶面和底面d[i]表示分解后以第i个立方体为基底可以达到的最大高度 j从1到i-1枚举 当满足top[i]==bot[j]时 d[i]=max(d[i],d[j]+1)

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 3005;
int buf[6], d[N], pre[N], n, m, ans;
char face[6][8] = {"front", "back", "left", "right", "top", "bottom"};
struct Cube{    int wei, bot, top;  } cube[N];

void print (int i)
{
    if (pre[i])  print (pre[i]);
    printf ("%d %s\n", cube[i].wei, face[ (i - 1) % 6]);
    return;
}

int main()
{
    int cas = 0;
    while (scanf ("%d", &n), n)
    {
        for (int i = m = 1; i <= n; ++i)
        {
            for (int k = 0; k < 6; ++k)
                scanf ("%d", &buf[k]);
            for (int k = 0; k < 6; ++k)
            {
                cube[m].wei = i;
                cube[m].top = buf[k];
                cube[m++].bot = (k % 2 ? buf[k - 1] : buf[k + 1]);
            }
        }
        memset (d, 0, sizeof (d));
        memset (pre, 0, sizeof (pre));

        for (int i = ans = 1; i <= 6 * n; ++i)
            for (int j = d[i] = 1; j < i; ++j)
                if (cube[j].wei < cube[i].wei && cube[j].bot == cube[i].top && d[i] < d[j] + 1)
                {
                    d[i] = d[j] + 1;
                    pre[i] = j;
                    if (d[i] > d[ans])
                        ans = i;
                }
        if (cas) printf ("\n");
        printf ("Case #%d\n%d\n", ++cas, d[ans]);
        print (ans);
    }
    return 0;
}
```

#

****

In this problem you are given *N* colorful cubes each having a distinct weight. Each face of a cube is colored with one color. Your job is to build a tower using the cubes you have subject to the following restrictions:

##

The input may contain multiple test cases. The first line of each test case contains an integer *N* ( ![$1 \le N \le 500$](../images/dge.org-external-100-10051img2.gif.png)) indicating the number of cubes you are given. The *i*th ( ![$1 \le i \le N$](../images/dge.org-external-100-10051img3.gif.png)) of the next *N* lines contains the description of the *i*th cube. A cube is described by giving the colors of its faces in the following order: front, back, left, right, top and bottom face. For your convenience colors are identified by integers in the range 1 to 100. You may assume that cubes are given in the increasing order of their weights, that is, cube 1 is the lightest and cube *N* is the heaviest.

The input terminates with a value 0 for *N*.

##

For each test case in the input first print the test case number on a separate line as shown in the sample output. On the next line print the number of cubes in the tallest tower you have built. From the next line describe the cubes in your tower from top to bottom with one description per line. Each description contains an integer (giving the serial number of this cube in the input) followed by a single whitespace character and then the identification string (front, back, left, right, top or bottom) of the top face of the cube in the tower. Note that there may be multiple solutions and any one of them is acceptable.

Print a blank line between two successive test cases.

##

3 1 2 2 2 1 2 3 3 3 3 3 3 3 2 1 1 1 1 10 1 5 10 3 6 5 2 6 7 3 6 9 5 7 3 2 1 9 1 3 3 5 8 10 6 6 2 2 4 4 1 2 3 4 5 6 10 9 8 7 6 5 6 1 2 3 4 7 1 2 3 3 2 1 3 2 1 1 2 3 0

##

Case /#1 2 2 front 3 front Case /#2 8 1 bottom 2 back 3 right 4 left 6 top 8 front 9 front 10 top