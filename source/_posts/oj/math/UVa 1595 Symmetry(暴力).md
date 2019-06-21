---
title: UVa 1595 Symmetry(暴力)
author: Deng
tags: 
       - Algorithm

category: 
       - Mathematics
       - OJ

date: 2015-01-22 10:04:16
---
题意 给你不超过1000个点的坐标 判断这些点是否是关于一条竖线对称的

没想到暴力枚举可以过 如果存在对称轴的话那么对称轴的横坐标一定是最左边的点和最右边的点的中点 为了避免中点是小数 可以将横坐标都乘上2 然后在判断所有点是否有对称点就行了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 1005;
int x[N], y[N], n, mx;

bool have(int i)
{
    for(int j = 0; j < n; ++j)
        if(y[i] == y[j] && x[i] + x[j] == 2 * mx) return true;
    return false;
}

int main()
{
    int cas, lx, rx, a, i;
    scanf("%d", &cas);
    while(cas--)
    {
        lx = rx = 0;
        scanf("%d", &n);
        for(i = 0; i < n; ++i)
        {
            scanf("%d%d", &a, &y[i]);
            x[i] = a * 2;
            if(x[i] < x[lx]) lx = i;
            if(x[i] > x[rx]) rx = i;
        }
        mx = (x[lx] + x[rx]) / 2;
        for(i = 0; i < n; ++i)
            if(!have(i)) break;

        if(i >= n) puts("YES");
        else puts("NO");
    }
    return 0;
}
```

The figure shown on the left is *left-right symmetric* as it is possible to fold the sheet of paper along a *vertical line*, drawn as a dashed line, and to cut the figure into two identical halves. The figure on the right is not left-right symmetric as it is impossible to find such a vertical line.

![\epsfbox{p3226.eps}](../images/dge.org-external-15-p3226.jpg.png)

Write a program that determines whether a figure, drawn with dots, is left-right symmetric or not. The dots are all distinct.

##

The input consists of *T* test cases. The number of test cases *T* is given in the first line of the input file. The first line of each test case contains an integer *N* , where *N* ( 1![$ \le$](../images/dge.org-external-15-3226img2.gif.png)*N*![$ \le$](../images/dge.org-external-15-3226img2.gif.png)1, 000) is the number of dots in a figure. Each of the following *N* lines contains the *x*-coordinate and *y*-coordinate of a dot. Both *x*-coordinates and *y*-coordinates are integers between -10,000 and 10,000, both inclusive.

##

Print exactly one line for each test case. The line should contain `YES' if the figure is left-right symmetric. and `NO', otherwise.

The following shows sample input and output for three test cases.

##

3 5 -2 5 0 0 6 5 4 0 2 3 4 2 3 0 4 4 0 0 0 4 5 14 6 10 5 10 6 14

##

YES NO YES