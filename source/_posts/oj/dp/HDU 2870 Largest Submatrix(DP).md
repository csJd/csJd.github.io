---
title: HDU 2870 Largest Submatrix(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:51:22
---
题意 求最大相同字符子矩阵 其中一些字符可以转换

其实就是HDU1505 1506的加强版 但是分了a,b,c三种情况 看哪次得到的面积最大

对于某一个情况 可以把该字符和可以转换为该字符的位置赋值0 其它位置赋值１　这样就转化成了求最大全0矩阵的问题了

对于转换后矩阵中的每个点　看他向上有多少个连续0　把这个值存在ｈ数组中　再用l数组和ｒ数组记录ｈ连续大于等于该位置的最左边位置和最右位置　这样包含(i,j)点的最大矩阵面积就是(r[i][j]-l[i][j]+1)/*h[i][j]　面积最大的点就是最大全0矩阵了

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 1005;
char s[N][N];
int mat[N][N], l[N][N], r[N][N], h[N][N], n, m, ans;

void makeMat (char a, char b, char c)
{
    memset (mat, 0, sizeof (mat));
    memset (l, 0, sizeof (l));
    memset (r, 0, sizeof (r));
    memset (h, 0, sizeof (h));
    for (int i = 1; i <= n; ++i)
    {
        h[i][0] = h[i][m + 1] = -1;
        for (int j = 1; j <= m; ++j)
        {
            l[i][j] = r[i][j] = j;
            if (s[i][j] == a || s[i][j] == b || s[i][j] == c)  mat[i][j] = 1;
            if (mat[i][j] == 0) h[i][j] = h[i - 1][j] + 1;
        }
    }
}

int solve (char a, char b, char c)
{
    makeMat (a, b, c);
    int aans = 0;
    for (int i = 1; i <= n; ++i)
    {
        for (int j = m; j >= 1; --j)
            while (h[i][r[i][j] + 1] >= h[i][j])
                r[i][j] = r[i][r[i][j] + 1];

        for (int j = 1; j <= m; ++j)
        {
            while (h[i][l[i][j] - 1] >= h[i][j])
                l[i][j] = l[i][l[i][j] - 1];
            aans = max (aans, (r[i][j] - l[i][j] + 1) * h[i][j]);
        }
    }
    return aans;
}

int main()
{
    while (~scanf ("%d%d", &n, &m))
    {
        for (int i = 1; i <= n; ++i)
            scanf ("%s", s[i] + 1);
        ans = max (solve ('x', 'b', 'c'), solve ('a', 'y', 'c'));
        ans = max (ans, solve ('a', 'b', 'w'));
        printf ("%d\n", ans);
    }
    return 0;
}
```

# Largest Submatrix

Problem Description

Now here is a matrix with letter 'a','b','c','w','x','y','z' and you can change 'w' to 'a' or 'b', change 'x' to 'b' or 'c', change 'y' to 'a' or 'c', and change 'z' to 'a', 'b' or 'c'. After you changed it, what's the largest submatrix with the same letters you can make?
Input

The input contains multiple test cases. Each test case begins with m and n (1 ≤ m, n ≤ 1000) on line. Then come the elements of a matrix in row-major order on m lines each with n letters. The input ends once EOF is met.
Output

For each test case, output one line containing the number of elements of the largest submatrix of all same letters.
Sample Input

2 4 abcw wxyz
Sample Output

3