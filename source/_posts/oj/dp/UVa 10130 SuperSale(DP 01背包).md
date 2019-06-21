---
title: UVa 10130 SuperSale(DP 01背包)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-22 23:14:43
---
题意 商畅做活动买东西 每个人每个商品只能拿一件 要拿价值尽量高的商品

很简单的01背包题目 求出每个人可以拿的最大值 加起来就是结果了

```js 
#include<cstdio>  
#include<cstring>  
#include<algorithm>  
using namespace std;  
#define maxn 1005  
int p[maxn],w[maxn],d[maxn],pe,t,n,g;  
int main()  
{  
    scanf("%d",&t);  
    while (t--)  
    {  
        scanf("%d",&n);  
        for(int i=1;i<=n;++i)  
            scanf("%d%d",&p[i],&w[i]);  
  
        scanf("%d",&g);  
        int ans=0;  
        for(int k=1;k<=g;++k)  
            {  
                memset(d,0,sizeof(d));  
                scanf("%d",&pe);  
                for(int i=1;i<=n;++i)  
                {  
                    for(int j=pe;j>=w[i];j--)  
                        d[j]=max(d[j],d[j-w[i]]+p[i]);  
                }  
                ans+=d[pe];  
            }  
        printf("%d\n",ans);  
    }  
    return 0;  
}
```

## SuperSale

There is a SuperSale in a SuperHiperMarket. Every person can take only one object of each kind, i.e. one TV, one carrot, but for extra low price. We are going with a whole family to that SuperHiperMarket. Every person can take as many objects, as he/she can carry out from the SuperSale. We have given list of objects with prices and their weight. We also know, what is the maximum weight that every person can stand. What is the maximal value of objects we can buy at SuperSale?

The input consists ofTtest cases. The number of them (1<=T<=1000) is given on the first line of the input file.

Each test case begins with a line containing a single integer numberNthat indicates the number of objects (1 <= N <= 1000). Then follows*N*lines, each containing two integers: P and W. The first integer (1<=P<=100) corresponds to the price of object. The secondinteger (1<=W<=30) corresponds to the weight of object. Next line contains one integer (1<=G<=100)it’s the number of people in our group. Next G lines contains maximal weight (1<=MW<=30) that can stand this i*-th*person from our family (1<=i<=G).

For every test case your program has to determine one integer. Print out the maximal value of goods which we can buy with that family.

2

3

72 17

44 23

31 24

1

26

6

64 26

85 22

52 4

99 18

39 13

54 9

4

23

20

20

26

72

514