---
title: POJ 1840 Eqs(hash)
author: Deng
tags: 
       - Algorithm

category: 
       - Mathematics
       - OJ

date: 2014-09-02 09:52:12
---
题意 输入a1,a2,a3,a4,a5 求有多少种不同的x1,x2,x3,x4,x5序列使得等式成立 a,x取值在-50到50之间

直接暴力的话肯定会超时的 100的五次方 10e了都 然后可以考虑将等式变一下形 把a1/*x1^3+a2/*x2^3移到右边 也就是-(a1/*x1^3+a2^x2^3)=a3/*x3^3+a4/*x4^3+a5/*x5^3

考虑到a1/*x1^3+a2^x2^3的最大值50/*50^3+50/*50^3=12500000 这个数并不大 可以开这么大的数组把每个结果出现的次数存下来 又因为结果最小可能是负的12500000 负数不能做数组的下标 加上个12500000/*2就行了 这样分别枚举左右两边 把左边出现过的结果都存在一个数组里面 再枚举右边 没出现一次结果 答案就加上前面这个结果出现的次数 枚举完就出现答案了

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int maxs = 50 * 50 * 50 * 50 * 4 + 10;
unsigned short cnt[maxs];
int main()
{
    int a1, a2, a3, a4, a5, sum,ans=0;
    scanf ("%d%d%d%d%d", &a1, &a2, &a3, &a4, &a5);
    for (int x1 = -50; x1 <= 50; ++x1)
    {
        if (x1 == 0) ++x1;
        for (int x2 = -50; x2 <= 50; ++x2)
        {
            if (x2 == 0) ++x2;
            sum = (a1 * x1 * x1 * x1 + a2 * x2 * x2 * x2) * (-1);
            if (sum < 0) ++cnt[sum + maxs];
            else ++cnt[sum];
        }
    }

    for (int x3 = -50; x3 <= 50; ++x3)
    {
        if (x3 == 0) ++x3;
        for (int x4 = -50; x4 <= 50; ++x4)
        {
            if (x4 == 0) ++x4;
            for (int x5 = -50; x5 <= 50; ++x5)
            {
                if (x5 == 0) ++x5;
                sum = (a3 * x3 * x3 * x3 + a4 * x4 * x4 * x4 + a5 * x5 * x5 * x5) ;
                if (sum < 0) sum += maxs;
                ans += cnt[sum];
            }
        }
    }
    printf ("%d\n", ans);
    return 0;
}
```

Eqs

Description
Consider equations having the following form:
a1x13+ a2x23+ a3x33+ a4x43+ a5x53=0
The coefficients are given integers from the interval [-50,50].
It is consider a solution a system (x1, x2, x3, x4, x5) that verifies the equation, xi∈[-50,50], xi != 0, any i∈{1,2,3,4,5}.
Determine how many solutions satisfy the given equation.

Input

The only line of input contains the 5 coefficients a1, a2, a3, a4, a5, separated by blanks.

Output

The output will contain on the first line the number of the solutions for the given equation.

Sample Input

37 29 41 43 47

Sample Output

654

还有用hashmap做的 空间优化了不少

```js 
#include<iostream>
#include<cstdio>
#include<cstring>
#include<hash_map>
using namespace std;
int first[50*50*50+10];
int ecnt,w[10005],v[10005],nex[10005];

void add(int x)
{
    int t=(x+50*50*50*100)/200,flag=1;
    for(int e=first[t];(~e)&&flag;e=nex[e])
    {
        if(v[e]==x)
        {
            flag=0,w[e]++;
        }
    }
    if(flag)
    {
        w[ecnt]=1;
        v[ecnt]=x;
        nex[ecnt]=first[t];
        first[t]=ecnt++;
    }
}
int getcnt(int x)
{
    if(x>50*50*50*50*2||x<-50*50*50*50*2)return 0;
    int t=(x+50*50*50*100)/200;
    for(int e=first[t];(~e);e=nex[e])
    {
        if(v[e]==x)return w[e];
    }
    return 0;
}
int main()
{
    int a1, a2, a3, a4, a5, sum,ans=0;
    scanf ("%d%d%d%d%d", &a1, &a2, &a3, &a4, &a5);
    memset(first,-1,sizeof first);
    ecnt=0;
    for (int x1 = -50; x1 <= 50; ++x1)
    {
        if (x1 == 0) ++x1;
        for (int x2 = -50; x2 <= 50; ++x2)
        {
            if (x2 == 0) ++x2;
            sum = (a1 * x1 * x1 * x1 + a2 * x2 * x2 * x2) * (-1);
            add(sum);
        }
    }
    for (int x3 = -50; x3 <= 50; ++x3)
    {
        if (x3 == 0) ++x3;
        for (int x4 = -50; x4 <= 50; ++x4)
        {
            if (x4 == 0) ++x4;
            for (int x5 = -50; x5 <= 50; ++x5)
            {
                if (x5 == 0) ++x5;
                sum = (a3 * x3 * x3 * x3 + a4 * x4 * x4 * x4 + a5 * x5 * x5 * x5) ;
                ans +=getcnt(sum);
            }
        }
    }
    printf ("%d\n", ans);
    return 0;
}
```