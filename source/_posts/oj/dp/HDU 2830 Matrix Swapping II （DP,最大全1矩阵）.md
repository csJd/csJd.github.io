---
title: HDU 2830 Matrix Swapping II （DP,最大全1矩阵）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:51:20
---
题意 给你一个n/*m矩阵 每列都可以随便交换位置 求最优交换后最大的全1子矩阵

又是HDU 1505 1506的变种 但这个更容易了 因为每列都可以交换位置了 那么这一行中所有比i高的都可以与i相邻了 只需要统计这一行有多少个比i高就行了 可以在算出每一行后 把高度大的放前面去 用num[i]记录排序后的列原来的数 这样就有j列比h[i][num[j]]高了 最后的答案也就是max(j/*h[i][num[j]])

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 1005;
char mat[N][N];
int num[N], h[N][N], n, m, i;

bool cmp (int a, int b)
{
    return h[i][a] > h[i][b];
}//cmp不能有=

int main()
{
    while (~scanf ("%d%d", &n, &m))
    {
        int ans = 0;
        memset (h, 0, sizeof (h));
        for (i = 1; i <= n; ++i)
            scanf ("%s", mat[i] + 1);
        for (i = 1; i <= n; ++i)
        {
            for (int j = 1; j <= m; ++j)
            {
                if (mat[i][j] == '1')  h[i][j] = h[i - 1][j] + 1;
                num[j] = j;
            }
            sort (num + 1, num + m + 1, cmp);
            for (int j = 1; j <= m; ++j)
                ans = max (ans, j * h[i][num[j]]);
        }
        printf ("%d\n", ans);
    }
    return 0;
}
```

# Matrix Swapping II

Problem Description

Given an N /* M matrix with each entry equal to 0 or 1. We can find some rectangles in the matrix whose entries are all 1, and we define the maximum area of such rectangle as this matrix’s goodness.
We can swap any two columns any times, and we are to make the goodness of the matrix as large as possible.
Input

There are several test cases in the input. The first line of each test case contains two integers N and M (1 ≤ N,M ≤ 1000). Then N lines follow, each contains M numbers (0 or 1), indicating the N /* M matrix
Output

Output one line for each test case, indicating the maximum possible goodness.
Sample Input

3 4 1011 1001 0001 3 4 1010 1001 0001
Sample Output

4 2 Note: Huge Input, scanf() is recommended.