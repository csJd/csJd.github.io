---
title: HDU 1081 To The Max(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2014-09-02 09:51:57
---
题意 求一个n/*n矩阵的最大子矩阵和

HDU 1003 max sum 的升级版 把二维简化为一维就可以用1003的方法去做了 用mat[i][j]存 第i行前j个数的和 那么mat[k][j]-mat[k][i]就表示第k行 第i+1个数到第j个数的和了 再将k从一枚举到n就可以得到这个这个宽度为j-i的最大矩阵和了 然后i,j又分别从1枚举到n就能得到结果了 和1003的方法一样 只是多了两层循环

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 105;
int main()
{
    int t, n, sum, ans, mat[N][N];
    while (~scanf ("%d", &n))
    {
        for (int i = 1; i <= n; ++i)
            for (int j = 1; j <= n; ++j)
            {
                scanf ("%d", &t);
                mat[i][j] = mat[i][j - 1] + t;
            }
        for (int i = ans = 0; i < n; ++i)
            for (int j = i + 1; j <= n; ++j)
            {
                sum = 0;
                for (int k = 1; k <= n; ++k)
                {
                    if (sum < 0) sum = 0;
                    sum += (mat[k][j] - mat[k][i]);
                    if (sum > ans) ans = sum;
                }
            }
        printf ("%d\n", ans);
    }
    return 0;
}
```

# To The Max

Problem Description

Given a two-dimensional array of positive and negative integers, a sub-rectangle is any contiguous sub-array of size 1 x 1 or greater located within the whole array. The sum of a rectangle is the sum of all the elements in that rectangle. In this problem the sub-rectangle with the largest sum is referred to as the maximal sub-rectangle.
As an example, the maximal sub-rectangle of the array:
0 -2 -7 0
9 2 -6 2
-4 1 -4 1
-1 8 0 -2
is in the lower left corner:
9 2
-4 1
-1 8
and has a sum of 15.
Input

The input consists of an N x N array of integers. The input begins with a single positive integer N on a line by itself, indicating the size of the square two-dimensional array. This is followed by N 2 integers separated by whitespace (spaces and newlines). These are the N 2 integers of the array, presented in row-major order. That is, all numbers in the first row, left to right, then all numbers in the second row, left to right, etc. N may be as large as 100. The numbers in the array will be in the range [-127,127].
Output

Output the sum of the maximal sub-rectangle.
Sample Input

4 0 -2 -7 0 9 2 -6 2 -4 1 -4 1 -1 8 0 -2
Sample Output

15