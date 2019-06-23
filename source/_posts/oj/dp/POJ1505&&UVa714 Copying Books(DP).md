---
title: POJ1505&&UVa714 Copying Books(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:50:23
---
Copying Books

**Time Limit:** 3000MS  **Memory Limit:** 10000K **Total Submissions:** 7109  **Accepted:** 2221

Description
Before the invention of book-printing, it was very hard to make a copy of a book. All the contents had to be re-written by hand by so called scribers. The scriber had been given a book and after several months he finished its copy. One of the most famous scribers lived in the 15th century and his name was Xaverius Endricus Remius Ontius Xendrianus (Xerox). Anyway, the work was very annoying and boring. And the only way to speed it up was to hire more scribers.
Once upon a time, there was a theater ensemble that wanted to play famous Antique Tragedies. The scripts of these plays were divided into many books and actors needed more copies of them, of course. So they hired many scribers to make copies of these books. Imagine you have m books (numbered 1, 2 ... m) that may have different number of pages (p1, p2 ... pm) and you want to make one copy of each of them. Your task is to divide these books among k scribes, k <= m. Each book can be assigned to a single scriber only, and every scriber must get a continuous sequence of books. That means, there exists an increasing succession of numbers 0 = b0 < b1 < b2, ... < bk-1 <= bk = m such that i-th scriber gets a sequence of books with numbers between bi-1+1 and bi. The time needed to make a copy of all the books is determined by the scriber who was assigned the most work. Therefore, our goal is to minimize the maximum number of pages assigned to a single scriber. Your task is to find the optimal assignment.

Input

The input consists of N cases. The first line of the input contains only positive integer N. Then follow the cases. Each case consists of exactly two lines. At the first line, there are two integers m and k, 1 <= k <= m <= 500. At the second line, there are integers p1, p2, ... pm separated by spaces. All these values are positive and less than 10000000.

Output

For each case, print exactly one line. The line must contain the input succession p1, p2, ... pm divided into exactly k parts such that the maximum sum of a single part should be as small as possible. Use the slash character ('/') to separate the parts. There must be exactly one space character between any two successive numbers and between the number and the slash.
If there is more than one solution, print the one that minimizes the work assigned to the first scriber, then to the second scriber etc. But each scriber must be assigned at least one book.

Sample Input

2 9 3 100 200 300 400 500 600 700 800 900 5 4 100 100 100 100 100

Sample Output

100 200 300 400 500 / 600 700 / 800 900 100 / 100 / 100 / 100 100

Source

[Central Europe 1998](http://poj.org/searchproblem?field=source&key=Central+Europe+1998)

题意 k个人复制m本书 求最小的时间 即把m个数分成k份 使和最大的那份最小

d[i][j]表示i个人完成前j本书需要的时间 有转移方程d[i][j]=min(d[i][j],max(d[i-1][k],s[j]-s[k])) k表示i-1到j之间的所有数 s[k]表示从第一本书到第k本书需要时间的和 初始d[1][i]=s[i];

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 550, INF = 0x3f3f3f3f;
int a[N], flag[N], s[N],  d[N][N], t, cas, n, m, ans;
int dp (int i, int j)
{
    if (d[i][j] > 0) return d[i][j];
    d[i][j] = INF;
    for (int k = i - 1; k < j; ++k)
        d[i][j] = min (d[i][j], max (dp (i - 1, k), s[j] - s[k]));
    return d[i][j];
}

int main()
{
    scanf ("%d", &cas);
    while (cas--)
    {
        scanf ("%d%d", &n, &m);
        memset (d, -1, sizeof (d));  memset (flag, -1, sizeof (flag));
        for (int i = 1; i <= n; ++i)
        { scanf ("%d", &a[i]); d[1][i] = s[i] = s[i - 1] + a[i]; }
        ans = dp (m, n);

        for (int i = n,t=0 ; i >= 1; --i)
            if ( ( (t = t + a[i]) > ans) || m > i)
                {  t = a[i]; flag[i] = 0; --m; }
        for (int i = 1; i <= n; ++i)
        {
            printf (flag[i] ? "%d" : "%d /", a[i]);
            printf (i == n ? "\n" : " ");
        }
    }
    return 0;
}
```