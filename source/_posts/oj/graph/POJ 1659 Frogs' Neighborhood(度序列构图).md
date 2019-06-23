---
title: POJ 1659 Frogs' Neighborhood(度序列构图)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-10-11 09:21:02
---
题意 中文

根据Havel-Hakimi定理构图就行咯 先把顶点按度数从大到小排序 可图的话 度数大的顶点与它后面的度数个顶点相连肯定是满足的 出现了-1就说明不可图了

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 20;
int mat[N][N], ord[N];

bool cmp(int i, int j)
{
    return mat[i][0] > mat[j][0];
}

int main()
{
    int cas, i, j, k, t, n;
    scanf("%d", &cas);
    while(cas--)
    {

        memset(mat, 0, sizeof(mat));
        scanf("%d", &n);
        for(i = 1; i <= n; ++i)
        {
            scanf("%d", &mat[i][0]);
            ord[i] = i;
        }

        for(i = 1; i <= n; ++i)
        {
            sort(ord + i, ord + n + 1, cmp);
            t = ord[i];
            if(mat[t][0] < 0) break;
            for(j = 1; j <= mat[t][0]; ++j)
            {
                k = ord[i + j];
                mat[t][k] = mat[k][t] = 1;
                --mat[k][0];
            }
        }

        if(i <= n) printf("NO\n");
        else
        {
            printf("YES\n");
            for(i = 1; i <= n; ++i)
            {
                for(int j = 1; j <= n; ++j)
                    printf("%d ", mat[i][j]);
                printf("\n");
            }
        }
        if(cas) printf("\n");
    }
    return 0;
}
```

Frogs' Neighborhood

Description
未名湖附近共有*N*个大小湖泊*L*1, *L*2, ..., *Ln*(其中包括未名湖)，每个湖泊*Li*里住着一只青蛙*Fi*(1 ≤ *i* ≤ *N*)。如果湖泊*Li*和*Lj*之间有水路相连，则青蛙*Fi*和*Fj*互称为邻居。现在已知每只青蛙的邻居数目*x*1, *x*2, ..., *xn*，请你给出每两个湖泊之间的相连关系。

Input

第一行是测试数据的组数*T*(0 ≤ *T* ≤ 20)。每组数据包括两行，第一行是整数N(2 < *N* < 10)，第二行是*N*个整数，*x*1, *x*2,..., *x*n(0 ≤ *xi* ≤ *N*)。

Output

对输入的每组测试数据，如果不存在可能的相连关系，输出"NO"。否则输出"YES"，并用*N*×*N*的矩阵表示湖泊间的相邻关系，即如果湖泊*i*与湖泊*j*之间有水路相连，则第*i*行的第*j*个数字为1，否则为0。每两个数字之间输出一个空格。如果存在多种可能，只需给出一种符合条件的情形。相邻两组测试数据之间输出一个空行。

Sample Input

3 7 4 3 1 5 4 2 1 6 4 3 1 4 2 0 6 2 3 1 1 2 1

Sample Output

YES 0 1 0 1 1 0 1 1 0 0 1 1 0 0 0 0 0 1 0 0 0 1 1 1 0 1 1 0 1 1 0 1 0 1 0 0 0 0 1 1 0 0 1 0 0 0 0 0 0 NO YES 0 1 0 0 1 0 1 0 0 1 1 0 0 0 0 0 0 1 0 1 0 0 0 0 1 1 0 0 0 0 0 0 1 0 0 0

Source

[POJ Monthly--2004.05.15 Alcyone@pku](http://poj.org/searchproblem?field=source&key=POJ+Monthly--2004.05.15+Alcyone%40pku)