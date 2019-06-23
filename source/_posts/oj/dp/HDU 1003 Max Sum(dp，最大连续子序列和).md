---
title: HDU 1003 Max Sum(dp，最大连续子序列和)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:50:53
---
# Max Sum

Problem Description

Given a sequence a[1],a[2],a[3]......a[n], your job is to calculate the max sum of a sub-sequence. For example, given (6,-1,5,4,-7), the max sum in this sequence is 6 + (-1) + 5 + 4 = 14.
Input

The first line of the input contains an integer T(1<=T<=20) which means the number of test cases. Then T lines follow, each line starts with a number N(1<=N<=100000), then N integers followed(all the integers are between -1000 and 1000).
Output

For each test case, you should output two lines. The first line is "Case /#:", /# means the number of the test case. The second line contains three integers, the Max Sum in the sequence, the start position of the sub-sequence, the end position of the sub-sequence. If there are more than one result, output the first one. Output a blank line between two cases.
Sample Input

2 5 6 -1 5 4 -7 7 0 6 -1 1 -6 7 -5
Sample Output

Case 1: 14 1 4  Case 2: 7 1 6

题意 求n个数字的最大连续和

DP的入门题目 令d[i]表示以第i个数a为右端的最大连续子序列和 那么很容易得出转移方程 d[i]=max(d[i-1]+a,a)

很显然 当第i个数比以第i-1个数为右端的最大和加上第i个数还大的时候 以第i个数为右端的最大和就是第i个数自己了 同时更新左端为自己

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 100005;
int main()
{
    int a, cas, ans, l, le, ri, n, d[N];
    scanf ("%d", &cas);
    for (int k = 1; k <= cas; ++k)
    {
        memset (d, 0x8f, sizeof (d));
        ans = d[0];
        scanf ("%d", &n);
        for (int i = 1; i <= n; ++i)
        {
            scanf ("%d", &a);
            if (d[i - 1] + a < a)
                d[i] = a, l = i;
            else
                d[i] = d[i - 1] + a;
            if (d[i] > ans)
                ans = d[i], le = l, ri = i;
        }
        if (k > 1) printf ("\n");
        printf ("Case %d:\n%d %d %d\n", k, ans, le, ri);
    }
    return 0;
}
```