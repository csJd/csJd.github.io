---
title: UVa 348 Optimal Array Multiplication Sequence (DP 最优矩阵链乘)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-25 21:24:16
---
﻿﻿

题意 求给定数目矩阵的最优链乘 并输出路径

矩阵链乘在小白第二版 P277有讲解 也是经典的动态规划 有了转移方程题目就好做了 设d[i][j]为矩阵i到矩阵j的最优链乘 初始状态d[i][i]=0 k为i，j之间的一个矩阵 x[i],y[i]分别表示第i个矩阵的行和列 则有d[i][j]=min{d[i][k]+d[k+1][j]+x[i]/*y[k]/*y[j]}
感觉这题比较难的就是路径打印了 仔细观察一下结构 其实也不难 记录每个k值 直接递归就行了

```js 
#include<cstdio>  
#include<cstring>  
using namespace std;  
#define maxn 11  
#define INF 0x3f3f3f3f  
#define t dp(i,k)+dp(k+1,j)+x[i]*y[k]*y[j]  
int d[maxn][maxn],pre[maxn][maxn],x[maxn],y[maxn],n;  
int dp(int i,int j)  
{  
    if(d[i][j]<INF) return d[i][j];  
    for(int k=i; k<j; ++k)  
        if(d[i][j]>t)  
        {  
            d[i][j]=t;  
            pre[i][j]=k;  
        }  
    return d[i][j];  
}  
  
void print(int i,int j)  
{  
    int k=pre[i][j];  
    if(k)  
    {  
        printf("(");  
        print(i,k);  
        printf(" x ");  
        print(k+1,j);  
        printf(")");  
    }  
    else  
    {  
        printf("A%d",i);  
    }  
}  
  
int main()  
{  
    int k=1;  
    while(scanf("%d",&n),n)  
    {  
        memset(d,0x3f,sizeof(d));  
        memset(pre,0,sizeof(pre));  
        for(int i=1; i<=n; ++i)  
        {  
            scanf("%d%d",&x[i],&y[i]);  
            d[i][i]=0;  
        }  
        dp(1,n);  
        printf("Case %d: ",k);  
        print(1,n);  
        printf("\n");  
        k++;  
    }  
    return 0;  
}
```

#

****

Given two arrays *A* and *B*, we can determine the array *C* = *A B* using the standard definition of matrix multiplication:

![](../images/dge.org-external-3-348img13.gif.png)

The number of columns in the *A* array must be the same as the number of rows in the *B* array. Notationally, let's say that *rows*(*A*) and *columns*(*A*) are the number of rows and columns, respectively, in the *A* array. The number of individual multiplications required to compute the entire *C* array (which will have the same number of rows as *A* and the same number of columns as *B*) is then *rows*(*A*)*columns*(*B*) *columns*(*A*). For example, if *A* is a ![tex2html_wrap_inline67](../images/dge.org-external-3-348img1.gif.png) array, and *B* is a ![tex2html_wrap_inline71](../images/dge.org-external-3-348img2.gif.png) array, it will take![tex2html_wrap_inline73](../images/dge.org-external-3-348img3.gif.png) , or 3000 multiplications to compute the *C* array.

To perform multiplication of more than two arrays we have a choice of how to proceed. For example, if *X*, *Y*, and *Z* are arrays, then to compute *X Y Z* we could either compute (*X Y*) *Z* or *X* (*Y Z*). Suppose *X*is a ![tex2html_wrap_inline103](../images/dge.org-external-3-348img4.gif.png) array, *Y* is a ![tex2html_wrap_inline67](../images/dge.org-external-3-348img1.gif.png) array, and *Z* is a ![tex2html_wrap_inline111](../images/dge.org-external-3-348img5.gif.png) array. Let's look at the number of multiplications required to compute the product using the two different sequences:

(*X Y*) *Z*

*X* (*Y Z*)

Clearly we'll be able to compute (*X Y*) *Z* using fewer individual multiplications.

Given the size of each array in a sequence of arrays to be multiplied, you are to determine an optimal computational sequence. Optimality, for this problem, is relative to the number of individual multiplications required.

##

For each array in the multiple sequences of arrays to be multiplied you will be given only the dimensions of the array. Each sequence will consist of an integer *N* which indicates the number of arrays to be multiplied, and then *N* pairs of integers, each pair giving the number of rows and columns in an array; the order in which the dimensions are given is the same as the order in which the arrays are to be multiplied. A value of zero for *N* indicates the end of the input. *N* will be no larger than 10.

##

Assume the arrays are named ![tex2html_wrap_inline157](../images/dge.org-external-3-348img12.gif.png) . Your output for each input case is to be a line containing a parenthesized expression clearly indicating the order in which the arrays are to be multiplied. Prefix the output for each case with the case number (they are sequentially numbered, starting with 1). Your output should strongly resemble that shown in the samples shown below. If, by chance, there are multiple correct sequences, any of these will be accepted as a valid answer.

##

3 1 5 5 20 20 1 3 5 10 10 20 20 35 6 30 35 35 15 15 5 5 10 10 20 20 25 0

##

Case 1: (A1 x (A2 x A3)) Case 2: ((A1 x A2) x A3) Case 3: ((A1 x (A2 x A3)) x ((A4 x A5) x A6))

﻿﻿