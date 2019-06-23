---
title: POJ 2240 Arbitrage(最短路 套汇)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-10-29 10:17:53
---
题意 给你n种币种之间的汇率关系 判断能否形成套汇现象 即某币种多次换为其它币种再换回来结果比原来多

基础的最短路 只是加号换为了乘号

```js 
#include<cstdio>
#include<cstring>
#include<string>
#include<map>
using namespace std;
map<string, int> na;
const int N = 31;
double d[N], rate[N][N], r;
int n, m, ans;

int bellman(int s)
{
    memset(d, 0, sizeof(d));
    d[s] = 1.0;
    for(int k = 1; k <= n; ++k)
    {
        for(int i = 1; i <= n; ++i)
        {
            for(int j = 1; j <= n; ++j)
                if(d[i] < d[j]*rate[j][i])
                    d[i] = d[j] * rate[j][i];
        }
    }
    return d[s] > 1.0;
}

int main()
{
    int cas = 0;
    char s[100], a[100], b[100];
    while(scanf("%d", &n), n)
    {
        na.clear();
        ans = 0;
        memset(rate, 0, sizeof(rate));
        for(int i = 1; i <= n; ++i)
        {
            rate[i][i] = 1.0;
            scanf("%s", s);
            na[s] = i;
        }

        scanf("%d", &m);
        for(int i = 1; i <= m; ++i)
        {
            scanf("%s%lf%s", a, &r, b);
            rate[na[a]][na[b]] = r;
        }

        for(int i = 1; i <= n; ++i)
        {
            if(bellman(i))
            {
                ans = 1; break;
            }
        }
        printf("Case %d: %s\n", ++cas, ans ? "Yes" : "No");
    }
    return 0;
}
```

Arbitrage

Description
Arbitrage is the use of discrepancies in currency exchange rates to transform one unit of a currency into more than one unit of the same currency. For example, suppose that 1 US Dollar buys 0.5 British pound, 1 British pound buys 10.0 French francs, and 1 French franc buys 0.21 US dollar. Then, by converting currencies, a clever trader can start with 1 US dollar and buy 0.5 /* 10.0 /* 0.21 = 1.05 US dollars, making a profit of 5 percent.
Your job is to write a program that takes a list of currency exchange rates as input and then determines whether arbitrage is possible or not.

Input

The input will contain one or more test cases. Om the first line of each test case there is an integer n (1<=n<=30), representing the number of different currencies. The next n lines each contain the name of one currency. Within a name no spaces will appear. The next line contains one integer m, representing the length of the table to follow. The last m lines each contain the name ci of a source currency, a real number rij which represents the exchange rate from ci to cj and a name cj of the destination currency. Exchanges which do not appear in the table are impossible.
Test cases are separated from each other by a blank line. Input is terminated by a value of zero (0) for n.

Output

For each test case, print one line telling whether arbitrage is possible or not in the format "Case case: Yes" respectively "Case case: No".

Sample Input

3 USDollar BritishPound FrenchFranc 3 USDollar 0.5 BritishPound BritishPound 10.0 FrenchFranc FrenchFranc 0.21 USDollar 3 USDollar BritishPound FrenchFranc 6 USDollar 0.5 BritishPound USDollar 4.9 FrenchFranc BritishPound 10.0 FrenchFranc BritishPound 1.99 USDollar FrenchFranc 0.09 BritishPound FrenchFranc 0.19 USDollar 0

Sample Output

Case 1: Yes Case 2: No