---
title: Codeforce 22B Bargaining Table
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:50:46
---
B. Bargaining Table

Bob wants to put a new bargaining table in his office. To do so he measured the office room thoroughly and drew its plan: Bob's office room is a rectangular room *n* × *m* meters. Each square meter of the room is either occupied by some furniture, or free. A bargaining table is rectangular, and should be placed so, that its sides are parallel to the office walls. Bob doesn't want to change or rearrange anything, that's why all the squares that will be occupied by the table should be initially free. Bob wants the new table to sit as many people as possible, thus its perimeter should be maximal. Help Bob find out the maximum possible perimeter of a bargaining table for his office.
Input

The first line contains 2 space-separated numbers *n* and *m* (1 ≤ *n*, *m* ≤ 25) — the office room dimensions. Then there follow *n* lines with *m* characters 0 or 1 each. 0 stands for a free square meter of the office room. 1 stands for an occupied square meter. It's guaranteed that at least one square meter in the room is free.

Output

Output one number — the maximum possible perimeter of a bargaining table for Bob's office room.
Sample test(s)

input 3 3 000 010 000

output 8
input 5 4 1100 0000 0000 0000 0000

output 16

HDU 1505，1506的变形 只是由求面积变成了求周长 具体分析可见http://blog.csdn.net/iooden/article/details/38379065

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 30;
int l[N][N], r[N][N], h[N][N], n, m, ans;
char a[N][N];
int main()
{
    scanf ("%d%d", &n, &m);
    for (int i = 1; i <= n; ++i)
    {
        h[i][0] = h[i][m + 1] = -1;
        scanf ("%s", a[i] + 1);
        for (int j = 1; j <= m; ++j)
        {
            if (a[i][j] == '0') h[i][j] = h[i - 1][j] + 1;
            l[i][j] = r[i][j] = j;
        }
    }
    int ans = 0;
    for (int i = 1; i <= n; ++i)
    {
        for (int j = m; j >= 1; --j)
            while (h[i][r[i][j] + 1] >= h[i][j]&&h[i][j])
                r[i][j] = r[i][r[i][j] + 1];

        for (int j = 1; j <= m; ++j)
        {
            while (h[i][l[i][j] - 1] >= h[i][j]&&h[i][j])
                l[i][j] = l[i][l[i][j] - 1];
            ans = max (ans, r[i][j] - l[i][j] + 1 + h[i][j]);
        }
    }
    printf ("%d\n", 2 * ans);
    return 0;
}
```
另外这题数据比较小 也可以暴力枚举 枚举每点作为左上角 然后枚举合法的的长和宽, 判断形成的的矩阵是否全由 '0'组成, 如果是就更新结果

```js 
#include<cstdio>
#include<cstring>
const int maxn = 30;
int n, m;
int a[maxn][maxn];
int sum[maxn][maxn];
int main()
{
    while (~scanf ("%d%d", &n, &m))
    {
        memset (a, 0, sizeof (a));
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= m; j++)
                scanf ("%1d", &a[i][j]);
        memset (sum, 0, sizeof (sum));
        for (int i = 1; i <= n; i++)
        {
            for (int j = 1; j <= m; j++)
            {
                sum[i][j] = sum[i][j - 1] + sum[i - 1][j] - sum[i - 1][j - 1] + a[i][j];
            }
        }
        int x, y;
        int ans = 0;
        for (int x1 = 1; x1 <= n; x1++)
            for (int y1 = 1; y1 <= m; y1++)
                for (int x2 = x1; x2 <= n; x2++)
                    for (int y2 = y1; y2 <= m; y2++)
                    {
                        int tmp = sum[x2][y2] + sum[x1 - 1][y1 - 1] - sum[x1 - 1][y2] - sum[x2][y1 - 1];
                        if (tmp == 0)
                        {
                            int dx = x2 - x1 + 1;
                            int dy = y2 - y1 + 1;
                            if (ans < (dx + dy) * 2) ans = (dx + dy) * 2;
                        }
                    }
        printf ("%d\n", ans);
    }
    return 0;
}
```