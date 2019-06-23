---
title: POJ 1789 Truck History（最小生成树）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-10-24 19:50:51
---
题意 有n辆卡车 每辆卡车用7个字符表示 输入n 再输入n行字符 第i行与第j行的两个字符串有多少个对应位置的字符不同 i与j之间的距离就是几 求连接所有卡车的最短长度 题目不是这个意思 这样理解就行了

prim啦啦啦啦

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 2005;
int cost[N], dis[N][N], n, ans;

void prim()
{
    memset(cost, 0x3f, sizeof(cost));
    cost[1] = -1;
    int cur = 1, next = 0;
    for(int i = 1; i < n; ++i)
    {
        for(int j = 1; j <= n; ++j)
        {
            if(cost[j] == -1 || cur == j) continue;
            if(dis[cur][j] < cost[j]) cost[j] = dis[cur][j];
            if(cost[j] < cost[next]) next = j;
        }
        cur = next, next = 0, ans += cost[cur], cost[cur] = -1;
    }
}

int main()
{
    char s[N][10];
    while(scanf("%d", &n), n)
    {
        memset(dis, 0, sizeof(dis));
        for(int i = 1; i <= n; ++i)
            scanf("%s", s[i]);
        for(int i = 1; i <= n; ++i)
            for(int j = 1; j <= n; ++j)
                for(int k = 0; k < 7; ++k)
                    if(s[i][k] != s[j][k]) ++dis[i][j];
        ans = 0;
        prim();
        printf("The highest possible quality is 1/%d.\n", ans);
    }
}
```

Truck History

Description
Advanced Cargo Movement, Ltd. uses trucks of different types. Some trucks are used for vegetable delivery, other for furniture, or for bricks. The company has its own code describing each type of a truck. The code is simply a string of exactly seven lowercase letters (each letter on each position has a very special meaning but that is unimportant for this task). At the beginning of company's history, just a single truck type was used but later other types were derived from it, then from the new types another types were derived, and so on.
Today, ACM is rich enough to pay historians to study its history. One thing historians tried to find out is so called derivation plan -- i.e. how the truck types were derived. They defined the distance of truck types as the number of positions with different letters in truck type codes. They also assumed that each truck type was derived from exactly one other truck type (except for the first truck type which was not derived from any other type). The quality of a derivation plan was then defined as
**1/Σ(to,td)d(to,td)**
where the sum goes over all pairs of types in the derivation plan such that t o is the original type and t d the type derived from it and d(t o,t d) is the distance of the types.
Since historians failed, you are to write a program to help them. Given the codes of truck types, your program should find the highest possible quality of a derivation plan.

Input

The input consists of several test cases. Each test case begins with a line containing the number of truck types, N, 2 <= N <= 2 000. Each of the following N lines of input contains one truck type code (a string of seven lowercase letters). You may assume that the codes uniquely describe the trucks, i.e., no two of these N lines are the same. The input is terminated with zero at the place of number of truck types.

Output

For each test case, your program should output the text "The highest possible quality is 1/Q.", where 1/Q is the quality of the best derivation plan.

Sample Input

4 aaaaaaa baaaaaa abaaaaa aabaaaa 0

Sample Output

The highest possible quality is 1/3.

Source

[CTU Open 2003](http://poj.org/searchproblem?field=source&key=CTU+Open+2003)