---
title: UVA 10131 Is Bigger Smarter? （DP,最长条件子序列）
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-22 09:28:26
---
题意 找出一个最长的序列，满足大象的体重增加时 智商是减小的
也是基础的DP d[i]表示以第i个大象为起点的最长序列 当满足(w[i]>w[j])&&(s[i]<s[j]时 d[i]=max(d[i],dp(j)+1)

```js 
#include<cstdio>
#include<algorithm>
#define M 1005
using namespace std;
int w[M],s[M],d[M],pre[M],n,l;

int dp(int i)
{
    if(d[i]>0) return d[i];
    d[i]=1;
    for(int j=1; j<=n; ++j)
    {
        if((w[i]>w[j])&&(s[i]<s[j]))
        {
            if(d[i]<dp(j)+1)
            {
                d[i]=d[j]+1;
                pre[i]=j;
            }
        }
    }
    return d[i];
}

void print(int i)
{
    if(pre[i])
    {
        print(pre[i]);
        printf("%d\n",i);
    }
    else
    {
        printf("%d\n",i);
        return;
    }
}

int main()
{
    n=0;l=1;
    while (scanf("%d %d",&w[n],&s[++n])!=EOF);
    for(int i=1; i<=n; ++i)
        if(dp(i)>dp(l)) l=i;
    printf("%d\n",d[l]);
    print(l);
    return 0;
}
```

# Is Bigger Smarter?

## The Problem

Some people think that the bigger an elephant is, the smarter it is. To disprove this, you want to take the data on a collection of elephants and put as large a subset of this data as possible into a sequence so that the weights are increasing, but the IQ's are decreasing.

The input will consist of data for a bunch of elephants, one elephant per line, terminated by the end-of-file. The data for a particular elephant will consist of a pair of integers: the first representing its size in kilograms and the second representing its IQ in hundredths of IQ points. Both integers are between 1 and 10000. The data will contain information for at most 1000 elephants. Two elephants may have the same weight, the same IQ, or even the same weight and IQ.

Say that the numbers on the i-th data line are W[i] and S[i]. Your program should output a sequence of lines of data; the first line should contain a number n; the remaining n lines should each contain a single positive integer (each one representing an elephant). If these nintegers are a[1], a[2],..., a[n] then it must be the case that
W[a[1]] < W[a[2]] < ... < W[a[n]]

and

S[a[1]] > S[a[2]] > ... > S[a[n]]

In order for the answer to be correct,nshould be as large as possible. All inequalities are strict: weights must be strictly increasing, and IQs must be strictly decreasing. There may be many correct outputs for a given input, your program only needs to find one.

## Sample Input

6008 1300 6000 2100 500 2000 1000 4000 1100 3000 6000 2000 8000 1400 6000 1200 2000 1900

## Sample Output

4 4 5 9 7