---
title: HDU 4882 ZCC Loves Codefires(贪心)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-27 10:27:40
---
﻿﻿

题意 有n个题目 完成第i个题目需要的时间为e[i] 第i个题目的系数为k[i] 你可以按任意顺序完成题目 比赛开始到完成第i个题目消耗的总时间为t[i] 那么完成第i个题目要扣掉k[i]/*t[i]分 求完成所有题目至少扣多少分

考虑任意相邻两题i,j 改变i,j时 i,j之前和之后所有的题目对结果都没有影响 只是i,j两题的扣分和由原来的(t+e[i])/*k[i]+(t+e[i]+e[j])/*k[j]变成了(t+e[i]+e[j])/*k[i]+(t+e[j])/*k[j]
比较可以发现只是e[i]/*k[j]变成了e[j]/*k[i] 所以 若e[i]/*k[j]<e[j]/*k[i] 那么i就应该在j之前完成 以此进行排序即可

```js 
<span style="font-size:14px;">#include<cstdio>  
#include<algorithm>  
using namespace std;  
typedef long long ll;  
const int N = 100050;  
ll k[N], o[N], t[N], e[N], n;  
  
bool cmp (int i, int j)  
{  
    return (e[i] * k[j] < e[j] * k[i]);  
}  
  
int main()  
{  
    while (scanf ("%I64d", &n) != EOF)  
    {  
        for (int i = 1; i <= n; ++i)  
            scanf ("%I64d", &e[i]);  
  
        for (int i = 1; i <= n; ++i)  
        {  
            o[i] = i;  
            scanf ("%I64d", &k[i]);  
        }  
        sort (o + 1, o + n + 1, cmp);  
        ll ans=0;  
  
        for (int i = 1; i <= n; ++i)  
        {  
            int j=o[i];  
            t[j]=t[o[i-1]]+e[j];  
            ans += k[j] * t[j];  
        }  
        printf ("%I64d\n", ans);  
    }  
    return 0;  
} </span>
```

Though ZCC has many Fans, ZCC himself is a crazy Fan of a coder, called "Memset137".
It was on Codefires(CF), an online competitive programming site, that ZCC knew Memset137, and immediately became his fan.
But why?
Because Memset137 can solve all problem in rounds, without unsuccessful submissions; his estimation of time to solve certain problem is so accurate, that he can surely get an Accepted the second he has predicted. He soon became IGM, the best title of Codefires. Besides, he is famous for his coding speed and the achievement in the field of Data Structures.
After become IGM, Memset137 has a new goal: He wants his score in CF rounds to be as large as possible.
What is score? In Codefires, every problem has 2 attributes, let's call them Ki and Bi(Ki, Bi＞0). if Memset137 solves the problem at Ti-th second, he gained Bi-Ki/*Ti score. It's guaranteed Bi-Ki/*Ti is always positive during the round time.
Now that Memset137 can solve every problem, in this problem, Bi is of no concern. Please write a program to calculate the minimal score he will lose.(that is, the sum of Ki/*Ti).
Input

The first line contains an integer N(1≤N≤10^5), the number of problem in the round.
The second line contains N integers Ei(1≤Ei≤10^4), the time(second) to solve the i-th problem.
The last line contains N integers Ki(1≤Ki≤10^4), as was described.
Output

One integer L, the minimal score he will lose.
Sample Input

3 10 10 20 1 2 3
Sample Output

150