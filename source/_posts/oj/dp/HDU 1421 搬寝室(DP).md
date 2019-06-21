---
title: HDU 1421 搬寝室(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-09-02 09:51:24
---
题意 中文

先把物品重量从小到大排序 d[i][j]表示前i件物品选j对的最小疲劳
若选了第i个物品 那么和它一对的必是第i-1个物品 注意是前i件
i=j/*2时 没有选择 d[i][j]=d[i-2][j-1]+(w[i]-w[i-1])^2
i>j/*2时 存在第i个选或者不选之分
若选了第i个的话 那么问题就转化为在i-2个物品中选j-1个了
若不选第i个的话 问题转化为在i-1个物品中选j个了

那么就有转移方程d[i][j]=min(d[i-1][j],d[i-2][j-1]+(w[i]-w[i-1])^2)

d是初始化为无穷大的 所以i=j/*2时 d[i-1][j]是等于无穷大的 所以状态转移方程可以统一为

d[i][j]=min(d[i-1][j],d[i-2][j-1]+(w[i]-w[i-1])^2)

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 2014;
int w[N], d[N][N / 2], n, k;

int main()
{
    while (~scanf ("%d%d", &n, &k))
    {
        for (int i = 1; i <= n; ++i)
            scanf ("%d", &w[i]);
        sort (w + 1, w + n + 1);
        memset (d, 0x7f, sizeof (d));
        for (int i = 0; i <= n; ++i)  d[i][0] = 0;
        
        for (int i = 2; i <= n; ++i)
            for (int j = 1; j * 2 <= i; ++j)
                d[i][j] = min (d[i - 1][j], d[i - 2][j - 1] + (w[i] - w[i - 1]) * (w[i] - w[i - 1]));
                
        printf ("%d\n", d[n][k]);
    }
    return 0;
}
```

# 搬寝室

Problem Description

搬寝室是很累的,xhd深有体会.时间追述2006年7月9号,那天xhd迫于无奈要从27号楼搬到3号楼,因为10号要封楼了.看着寝室里的n件物品,xhd开始发呆,因为n是一个小于2000的整数,实在是太多了,于是xhd决定随便搬2/*k件过去就行了.但还是会很累,因为2/*k也不小是一个不大于n的整数.幸运的是xhd根据多年的搬东西的经验发现每搬一次的疲劳度是和左右手的物品的重量差的平方成正比(这里补充一句,xhd每次搬两件东西,左手一件右手一件).例如xhd左手拿重量为3的物品,右手拿重量为6的物品,则他搬完这次的疲劳度为(6-3)^2 = 9.现在可怜的xhd希望知道搬完这2/*k件物品后的最佳状态是怎样的(也就是最低的疲劳度),请告诉他吧.
Input

每组输入数据有两行,第一行有两个数n,k(2<=2/*k<=n<2000).第二行有n个整数分别表示n件物品的重量(重量是一个小于2^15的正整数).
Output

对应每组输入数据,输出数据只有一个表示他的最少的疲劳度,每个一行.
Sample Input

2 1 1 3
Sample Output

4