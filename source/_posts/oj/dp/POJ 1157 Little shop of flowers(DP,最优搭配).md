---
title: POJ 1157 Little shop of flowers(DP,最优搭配)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-22 10:41:05
---
﻿﻿ 题意 你有f束花和v个花瓶 每束花放在不同的花瓶中都会有不同的价值 并且花的相对顺序不能改变 也就是说第i束花放在第j个花瓶中 那么第i+1朵花要放在j以后的花瓶中 令d[i][j]表示第i朵花放在第[j]个瓶子中前i朵花的最大价值 有状态转移方程 d[i][j]=max{d[i-1][1~j-1]}+val[i][j]; 题目有几个坑点 可能所有价值都为负数 必须所有花都放完  
```js 
#include<cstdio>  
#include<cstring>  
using namespace std;  
#define M 205  
int val[M][M],d[M][M],pre[M][M],f,v,key;  
  
void print(int i,int j)  
{  
    if(pre[i][j])  
    {  
        print(i-1,pre[i][j]);  
        printf(" %d",j);  
    }  
    else  
        printf("%d",j);  
}  
  
int main()  
{  
    scanf("%d%d",&f,&v);  
    memset(d,0x8f,sizeof(d));  
  
    for(int i=1; i<=f; ++i)  
        for(int j=1; j<=v; ++j)  
            scanf("%d",&val[i][j]);  
  
    int ans=d[0][0];  
    //注意要把ans初始化为负无穷 可能所有的val都为负；这里wa了好久；  
    d[0][0]=0;  
    //0朵花价值当然为0了；  
      
    for(int i=1; i<=f; ++i)  
        for(int j=1; j<=v; ++j)  
        {  
            for(int k=0; k<j; ++k)  
                if(d[i-1][k]+val[i][j]>d[i][j])  
                {  
                    d[i][j]=d[i-1][k]+val[i][j];  
                    pre[i][j]=k;  
                }  
        }  
    for(int j=1; j<=v; ++j)  
        if(d[f][j]>ans)  
        {  
            ans=d[f][j];  
            key=j;  
        }  
    //这里要在最后一层更新ans 因为要保证所有花都放进去；  
  
    printf("%d\n",ans);  
    print(f,key);  
    printf("\n");  
  
    return 0;  
}
```

**Little shop of flowers**

****

**PROBLEM**
****

You want to arrange the window of your flower shop in a most pleasant way. You have*F*bunches of flowers, each being of a different kind, and at least as many vases ordered in a row. The vases are glued onto the shelf and are numbered consecutively 1 through*V*, where*V*is the number of vases, from left to right so that the vase 1 is the leftmost, and the vase*V*is the rightmost vase. The bunches are moveable and are uniquely identified by integers between 1 and*F*. These id-numbers have a significance: They determine the required order of appearance of the flower bunches in the row of vases so that the bunch*i*must be in a vase to the left of the vase containing bunch*j*whenever*i*<*j*. Suppose, for example, you have bunch of azaleas (id-number=1), a bunch of begonias (id-number=2) and a bunch of carnations (id-number=3). Now, all the bunches must be put into the vases keeping their id-numbers in order. The bunch of azaleas must be in a vase to the left of begonias, and the bunch of begonias must be in a vase to the left of carnations. If there are more vases than bunches of flowers then the excess will be left empty. A vase can hold only one bunch of flowers.

Each vase has a distinct characteristic (just like flowers do). Hence, putting a bunch of flowers in a vase results in a certain aesthetic value, expressed by an integer. The aesthetic values are presented in a table as shown below. Leaving a vase empty has an aesthetic value of 0.

****

**V A S E S** ****

**1**

 ****

**2**

 ****

**3**

 ****

**4**

 ****

**5** ****

**Bunches**

 ****

**1 (azaleas)**

 
7
 
23
 
-5
 
-24
 
16 ****

**2 (begonias)**

 
5
 
21
 
-4
 
10
 
23 ****

**3 (carnations)**

 
-21
 
5
 
-4
 
-20
 
20

According to the table, azaleas, for example, would look great in vase 2, but they would look awful in vase 4.

To achieve the most pleasant effect you have to maximize the sum of aesthetic values for the arrangement while keeping the required ordering of the flowers. If more than one arrangement has the maximal sum value, any one of them will be acceptable. You have to produce exactly one arrangement.

****

ASSUMPTIONS

1 ≤*F*≤ 100 where*F*is the number of the bunches of flowers. The bunches are numbered 1 through*F*.

**

*F*≤*V*≤ 100 where*V*is the number of vases.

-50£*Aij*£50 where*Aij*is the aesthetic value obtained by putting the flower bunch*i*into the vase*j*.

****

Input

The first line contains two numbers:*F*,*V*.

The following*F*lines: Each of these lines contains*V*integers, so that*Aij*is given as the*j*’th number on the (*i*+1)’st line of the input file.

****

Output

The first line will contain the sum of aesthetic values for your arrangement.

The second line must present the arrangement as a list of*F*numbers, so that the*k*’th number on this line identifies the vase in which the bunch*k*is put.

Sample Input
3 5 7 23 -5 -24 16 5 21 -4 10 23 -21 5 -4 -20 20

Sample Output
53 2 4 5