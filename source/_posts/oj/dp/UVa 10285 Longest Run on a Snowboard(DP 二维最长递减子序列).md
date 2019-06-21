---
title: UVa 10285 Longest Run on a Snowboard(DP 二维最长递减子序列)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-23 10:06:31
---
题意 输入一个城市的滑雪地图 你可以从高的地方滑到伤下左右低的地方 求这个城市的最长滑雪线路长度 即在一个矩阵中找出最长递减连续序列

令d[i][j]为以格子map(i,j)为起点的最长序列 则有状态转移方程d[i][j]=max{d[a][b]}+1 a,b为与i,j相邻且值比i,j小的所有点

```js 
#include<cstdio>  
#include<cstring>  
#include<algorithm>  
using namespace std;  
#define maxn 105  
#define a i+x[k]  
#define b j+y[k]  
int ma[maxn][maxn],d[maxn][maxn],r,c,n,ans;  
int x[4]={0,0,1,-1},y[4]={-1,1,0,0};  
  
int dp(int i,int j)  
{  
    if(d[i][j]>0) return d[i][j];  
    d[i][j]=1;  
    for(int k=0;k<4;++k)  
    if((ma[i][j]>ma[a][b])&&(a>0&&a<=r)&&(b>0&&b<=c))  
    {  
        d[i][j]=max(d[i][j],dp(a,b)+1);  
    }  
    return d[i][j];  
}  
  
int main()  
{  
    char name[100];  
    scanf("%d",&n);  
    for(int cas=1;cas<=n;++cas)  
    {  
        scanf("%s%d%d",name,&r,&c);  
        for(int i=1;i<=r;++i)  
            for(int j=1;j<=c;++j)  
                scanf("%d",&ma[i][j]);  
  
        memset(d,0,sizeof(d));  
        ans=0;  
        for(int i=1;i<=r;++i)  
            for(int j=1;j<=c;++j)  
                ans=max(dp(i,j),ans);  
  
        printf("%s: %d\n",name,ans);  
    }  
    return 0;  
}
```

**Longest Run on a Snowboard**

Michael likes snowboarding. That's not very surprising, since snowboarding is really great. The bad thing is that in order to gain speed, the area must slide downwards. Another disadvantage is that when you've reached the bottom of the hill you have to walk up again or wait for the ski-lift.

Michael would like to know how long the longest run in an area is. That area is given by a grid of numbers, defining the heights at those points. Look at this example:
**1 2 3 4 5** **16 17 18 19 6** **15 24 25 20 7** **14 23 22 21 8** **13 12 11 10 9**

One can slide down from one point to a connected other one if and only if the height decreases. One point is connected to another if it's at left, at right, above or below it. In the sample map, a possible slide would be**24-17-16-1**(start at**24**, end at**1**). Of course if you would go**25-24-23-...-3-2-1**, it would be a much longer run. In fact, it's the longest possible.

**Input**

The first line contains the number of test cases**N**. Each test case starts with a line containing the name (it's a single string), the number of rows**R**and the number of columns**C**. After that follow**R**lines with**C**numbers each, defining the heights.**R**and C won't be bigger than**100**,**N**not bigger than**15**and the heights are always in the range from**0**to**100**.

For each test case, print a line containing the name of the area, a colon, a space and the length of the longest run one can slide down in that area.
**Sample Input** 2 Feldberg 10 5 56 14 51 58 88 26 94 24 39 41 24 16 8 51 51 76 72 77 43 10 38 50 59 84 81 5 23 37 71 77 96 10 93 53 82 94 15 96 69 9 74 0 62 38 96 37 54 55 82 38 Spiral 5 5 1 2 3 4 5 16 17 18 19 6 15 24 25 20 7 14 23 22 21 8 13 12 11 10 9

**Sample Output**
Feldberg: 7 Spiral: 25