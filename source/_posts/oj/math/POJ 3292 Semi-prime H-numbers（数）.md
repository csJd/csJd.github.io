---
title: POJ 3292 Semi-prime H-numbers（数）
author: Deng
tags: 
       - Algorithm

category: 
       - Mathematics
       - OJ

date: 2014-09-02 09:50:50
---
题意 所有可以表示为4/*k+1（k>=0）的数都称为“H数” 而在所有“H数”中只能被1和自身整除的H数称为“H素数“ 能表示成两个”H素数“积的数又称为”Semi-prime H数“

输入n 求1到n之间有多少个”Semi-prime H数“;

方法 先打个H素数表 再用H素数表中的数依次相乘 得到的数都标记 再用一个数组保存每个数以内的标记数 输入n后直接读数组就行了

```js 
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
using namespace std;
const int N=1000001;
int vis[N],hp[N],ans[N],n;
int main()
{
    int num=0,m=sqrt(N+0.5);
    for(int i=5;i<=m;i+=4)
    {
        if(vis[i]==0)
            for(int j=i*i;j<=N;j+=i)
            vis[j]=1;
    }
    for(int i=5;i<N;i+=4)
        if(!vis[i]) hp[++num]=i;

    memset(vis,0,sizeof(vis));
    for(int i=1;hp[i]*hp[i]<=N;++i)
        for(int j=i;hp[i]*hp[j]<=N;++j)
            ++vis[hp[i]*hp[j]];
    num=0;
    for(int i=1;i<N;++i)
        {
            if(vis[i]>=1) ++num;
            ans[i]=num;
        }

    while(scanf("%d",&n),n)
        printf("%d %d\n",n,ans[n]);

    return 0;
}
```
Semi-prime H-numbers

Description
This problem is based on an exercise of David Hilbert, who pedagogically suggested that one study the theory of *4n+1* numbers. Here, we do only a bit of that.

An **H**-number is a positive number which is one more than a multiple of four: 1, 5, 9, 13, 17, 21,... are the **H**-numbers. For this problem we pretend that these are the *only* numbers. The **H**-numbers are closed under multiplication.

As with regular integers, we partition the **H**-numbers into units, **H**-primes, and **H**-composites. 1 is the only unit. An **H**-number *h* is **H**-prime if it is not the unit, and is the product of two **H**-numbers in only one way: 1 × *h*. The rest of the numbers are **H**-composite.

For examples, the first few **H**-composites are: 5 × 5 = 25, 5 × 9 = 45, 5 × 13 = 65, 9 × 9 = 81, 5 × 17 = 85.

Your task is to count the number of **H**-semi-primes. An **H**-semi-prime is an **H**-number which is the product of exactly two **H**-primes. The two **H**-primes may be equal or different. In the example above, all five numbers are **H**-semi-primes. 125 = 5 × 5 × 5 is not an **H**-semi-prime, because it's the product of three **H**-primes.

Input

Each line of input contains an **H**-number ≤ 1,000,001. The last line of input contains 0 and this line should not be processed.

Output

For each inputted **H**-number *h*, print a line stating *h* and the number of **H**-semi-primes between 1 and *h* inclusive, separated by one space in the format shown in the sample.

Sample Input

21 85 789 0

Sample Output

21 0 85 5 789 62

Semi-prime H-numbers

Description
This problem is based on an exercise of David Hilbert, who pedagogically suggested that one study the theory of *4n+1* numbers. Here, we do only a bit of that.

An **H**-number is a positive number which is one more than a multiple of four: 1, 5, 9, 13, 17, 21,... are the **H**-numbers. For this problem we pretend that these are the *only* numbers. The **H**-numbers are closed under multiplication.

As with regular integers, we partition the **H**-numbers into units, **H**-primes, and **H**-composites. 1 is the only unit. An **H**-number *h* is **H**-prime if it is not the unit, and is the product of two **H**-numbers in only one way: 1 × *h*. The rest of the numbers are **H**-composite.

For examples, the first few **H**-composites are: 5 × 5 = 25, 5 × 9 = 45, 5 × 13 = 65, 9 × 9 = 81, 5 × 17 = 85.

Your task is to count the number of **H**-semi-primes. An **H**-semi-prime is an **H**-number which is the product of exactly two **H**-primes. The two **H**-primes may be equal or different. In the example above, all five numbers are **H**-semi-primes. 125 = 5 × 5 × 5 is not an **H**-semi-prime, because it's the product of three **H**-primes.

Input

Each line of input contains an **H**-number ≤ 1,000,001. The last line of input contains 0 and this line should not be processed.

Output

For each inputted **H**-number *h*, print a line stating *h* and the number of **H**-semi-primes between 1 and *h* inclusive, separated by one space in the format shown in the sample.

Sample Input

21 85 789 0

Sample Output

21 0 85 5 789 62