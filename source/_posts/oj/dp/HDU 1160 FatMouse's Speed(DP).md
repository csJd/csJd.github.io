---
title: HDU 1160 FatMouse's Speed(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-09-02 09:51:53
---
题意 输入n个老鼠的体重和速度 从里面找出最长的序列 是的重量递增时速度递减

简单的DP 令d[i]表示以第i个老鼠为所求序列最后一个时序列的长度 对与每个老鼠i 遍历所有老鼠j 当(w[i] > w[j]) && (s[i] < s[j])时 有d[i]=max(d[i],d[j]+1) 输出路径记下最后一个递归就行了

```js 
#include<cstdio>
#include<algorithm>
using namespace std;
const int M=1005;
int w[M], s[M], d[M], pre[M], n, key;

int dp (int i)
{
    if (d[i] > 0) return d[i];
    for (int j = d[i] = 1; j <= n; ++j)
        if ( (w[i] > w[j]) && (s[i] < s[j]) && (d[i] < dp (j) + 1))
            d[i] = d[j] + 1, pre[i] = j;
    return d[i];
}

void print (int i)
{
    if (pre[i])
        print (pre[i]);
    printf ("%d\n", i);
}

int main()
{
    n = 0;
    while (scanf ("%d %d", &w[n], &s[++n]) != EOF);
    for (int i = key =1; i <= n; ++i)
        if (dp (i) > dp (key)) key = i;
    printf ("%d\n", d[key]);
    print (key);
    return 0;
}
```

# FatMouse's Speed

Problem Description

FatMouse believes that the fatter a mouse is, the faster it runs. To disprove this, you want to take the data on a collection of mice and put as large a subset of this data as possible into a sequence so that the weights are increasing, but the speeds are decreasing.
Input

Input contains data for a bunch of mice, one mouse per line, terminated by end of file.
The data for a particular mouse will consist of a pair of integers: the first representing its size in grams and the second representing its speed in centimeters per second. Both integers are between 1 and 10000. The data in each test case will contain information for at most 1000 mice.
Two mice may have the same weight, the same speed, or even the same weight and speed.
Output

Your program should output a sequence of lines of data; the first line should contain a number n; the remaining n lines should each contain a single positive integer (each one representing a mouse). If these n integers are m[1], m[2],..., m[n] then it must be the case that
W[m[1]] < W[m[2]] < ... < W[m[n]]
and
S[m[1]] > S[m[2]] > ... > S[m[n]]
In order for the answer to be correct, n should be as large as possible.
All inequalities are strict: weights must be strictly increasing, and speeds must be strictly decreasing. There may be many correct outputs for a given input, your program only needs to find one.
Sample Input

6008 1300 6000 2100 500 2000 1000 4000 1100 3000 6000 2000 8000 1400 6000 1200 2000 1900
Sample Output

4 4 5 9 7