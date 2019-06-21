---
title: HDU 1087 Super Jumping! Jumping! Jumping!(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-09-02 09:51:09
---
# Super Jumping! Jumping! Jumping!

Problem Description

Nowadays, a kind of chess game called “Super Jumping! Jumping! Jumping!” is very popular in HDU. Maybe you are a good boy, and know little about this game, so I introduce it to you now.
![](../images/cn-data-images-1087-1.jpg.png)
The game can be played by two or more than two players. It consists of a chessboard（棋盘）and some chessmen（棋子）, and all chessmen are marked by a positive integer or “start” or “end”. The player starts from start-point and must jumps into end-point finally. In the course of jumping, the player will visit the chessmen in the path, but everyone must jumps from one chessman to another absolutely bigger (you can assume start-point is a minimum and end-point is a maximum.). And all players cannot go backwards. One jumping can go from a chessman to next, also can go across many chessmen, and even you can straightly get to end-point from start-point. Of course you get zero point in this situation. A player is a winner if and only if he can get a bigger score according to his jumping solution. Note that your score comes from the sum of value on the chessmen in you jumping path.
Your task is to output the maximum value according to the given chessmen list.
Input

Input contains multiple test cases. Each test case is described in a line as follow:
N value_1 value_2 …value_N
It is guarantied that N is not more than 1000 and all value_i are in the range of 32-int.
A test case starting with 0 terminates the input and this test case is not to be processed.
Output

For each case, print the maximum according to rules, and one line one case.
Sample Input

3 1 3 2 4 1 2 3 4 4 3 3 2 1 0
Sample Output

4 10 3

题意 求n个数字的和最大的递增子序列

基础的dp题目 令d[i]表示以第i个数字结尾的和最大的递增子序列 有d[i]=max(d[i],d[j]+a[i]) j为1到a之间的数 且a[i]>a[j]

```js 
#include<cstdio>
#include<algorithm>
using namespace std;
const int N = 1005;
int a[N], d[N];
int main()
{
    int ans,n;
    while (scanf ("%d", &n), n)
    {
        ans=0;
        for (int i = 1; i <= n; ++i)
        {
            scanf ("%d", &a[i]);
            d[i]=a[i];
            for(int j=1;j<i;++j)
                if(a[i]>a[j]) d[i]=max(d[i],d[j]+a[i]);
            ans=max(ans,d[i]);
        }
        printf("%d\n",ans);
    }
    return 0;
}
```