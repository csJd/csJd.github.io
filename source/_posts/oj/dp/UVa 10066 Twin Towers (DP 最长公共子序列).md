---
title: UVa 10066 Twin Towers (DP 最长公共子序列)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-22 15:50:08
---
题意 求两串数字最长公共子序列的长度

裸的lcs没啥说的

```js 
#include<cstdio>  
#include<cstring>  
#include<algorithm>  
using namespace std;  
const int maxn=105;  
int a[maxn],b[maxn],d[maxn][maxn],na,nb;  
void lcs()  
{  
    memset(d,0,sizeof(d));  
    for(int i=1; i<=na; ++i)  
        for(int j=1; j<=nb; ++j)  
            if(a[i]==b[j]) d[i][j]=d[i-1][j-1]+1;  
            else d[i][j]=max(d[i-1][j],d[i][j-1]);  
}  
  
int main()  
{  
    int k=1;  
    while(scanf("%d%d",&na,&nb),na||nb)  
    {  
        for(int i=1; i<=na; ++i)  
            scanf("%d",&a[i]);  
        for(int i=1; i<=nb; ++i)  
            scanf("%d",&b[i]);  
        lcs();  
        printf("Twin Towers #%d\nNumber of Tiles : %d\n\n",k++,d[na][nb]);  
  
    }  
    return 0;  
}
```

**The Twin Towers**

**Input:**standard input****

**Output:**standard output

Once upon a time, in an ancient Empire, there were two towers of dissimilar shapes in two different cities. The towers were built byputting circular tiles one upon another. Each of the tiles was of the same height and had integral radius. It is no wonder that though the two towers were of dissimilar shape, they had many tiles in common.

However, more than thousand years after they were built, the Emperor ordered his architects to remove some of the tiles from the two towers so that they have exactly the same shape and size, and at the same time remain as high as possible. The order of the tiles in the new towers must remain the same as they were in the original towers. The Emperor thought that, in this way the two towers might be able to stand as the symbol of harmony and equality between the two cities. He decided to name them the*Twin Towers*.

Now, about two thousand years later, you are challenged with an even simpler problem: given the descriptions of two dissimilar towers you are asked only to find out the number of tiles in the highest twin towers that can be built from them.

**Input**

The input file consists of several data blocks. Each data block describes a pair of towers.

The first line of a data block contains two integers N1 and N2 (1 <= N1, N2 <= 100) indicating the number of tiles respectively in the two towers. The next line contains N1 positive integers giving the radii of the tiles (from top to bottom) in the first tower. Then follows another line containing N2 integers giving the radii of the tiles (from top to bottom) in the second tower.

The input file terminates with two zeros for N1 and N2.

**Output**

For each pair of towers in the input first output the twin tower number followed by the number of tiles (in one tower) in the highest possible twin towers that can be built from them. Print a blank line after the output of each data set.****

****

**Sample Input**

7 6
20 15 10 15 25 20 15
15 25 10 20 15 20
8 9
10 20 20 10 20 10 20 10
20 10 20 10 10 20 10 10 20
0 0

**Sample Output**

Twin Towers /#1
Number of Tiles : 4
Twin Towers /#2
Number of Tiles : 6
﻿﻿