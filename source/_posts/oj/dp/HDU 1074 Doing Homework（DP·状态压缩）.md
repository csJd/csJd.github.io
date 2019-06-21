---
title: HDU 1074 Doing Homework（DP·状态压缩）
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2015-08-28 14:47:03
---
题意 有n个作业要做 给你每个作业的最后期限 和做完这个作业需要的时间 作业每超过最后期限一天就会扣一分 只能把一个作业做完了再做另一个作业 问做完所有作业至少扣多少分

作业最多只有15个 看到这个数字容易想到是状态压缩 dp[i]表示i对应状态的最小扣分 i转换为二进制后为1的位表明该位对应的作业已经做了 为0的位没做 那么dp[i] = min{dp[k] + cost |k为将某一位变成1后等于 i 的状态}

由于要打印路径 所有还需要记录每个状态的上一个状态pre

```js 
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 16, M = 1 << N;
int d[N], c[N], n;
int dp[M], pre[M], t[M];
char s[N][105];

void print(int k)
{
    if(k == 0) return;
    print(pre[k]);
    k -= pre[k];
    for(int i = 0; i < n; ++i)
        if(k & 1 << i) puts(s[i]);
}

int main()
{
    int T, m, cost;
    scanf("%d", &T);
    while(T--)
    {
        scanf("%d", &n);
        for(int i = 0; i < n; ++i)
            scanf("%s%d%d", s[i], &d[i], &c[i]);
        memset(dp, 0x3f, sizeof(dp));
        dp[0] = t[0] = 0;  //边界 所有作业都没做的扣分为0
        m = 1 << n;

        for(int i = 1; i < m; ++i)
        {
            for(int j = 0; j < n; ++j)
            {
                if((i & 1 << j) == 0) continue;
                int k = i - (1 << j);
                t[i] = t[k] + c[j];
                cost = t[i] > d[j] ? t[i] - d[j] : 0;
                if(dp[k] + cost <= dp[i])  //'='保证字典序最小
                {
                    dp[i] = dp[k] + cost;
                    pre[i] =  k;  //记录路径
                }
            }
        }
        printf("%d\n", dp[m - 1]);
        print(m - 1);  //打印路径
    }
    return 0;
}
```

# Doing Homework

Problem Description

Ignatius has just come back school from the 30th ACM/ICPC. Now he has a lot of homework to do. Every teacher gives him a deadline of handing in the homework. If Ignatius hands in the homework after the deadline, the teacher will reduce his score of the final test, 1 day for 1 point. And as you know, doing homework always takes a long time. So Ignatius wants you to help him to arrange the order of doing homework to minimize the reduced score.
Input

The input contains several test cases. The first line of the input is a single integer T which is the number of test cases. T test cases follow.
Each test case start with a positive integer N(1<=N<=15) which indicate the number of homework. Then N lines follow. Each line contains a string S(the subject's name, each string will at most has 100 characters) and two integers D(the deadline of the subject), C(how many days will it take Ignatius to finish this subject's homework).
Note: All the subject names are given in the alphabet increasing order. So you may process the problem much easier.
Output

For each test case, you should output the smallest total reduced score, then give out the order of the subjects, one subject in a line. If there are more than one orders, you should output the alphabet smallest one.
Sample Input

2 3 Computer 3 3 English 20 1 Math 3 2 3 Computer 3 3 English 6 3 Math 6 3
Sample Output

2 Computer Math English 3 Computer English Math

*Hint* In the second test case, both Computer->English->Math and Computer->Math->English leads to reduce 3 points, but the word "English" appears earlier than the word "Math", so we choose the first order. That is so-called alphabet order.