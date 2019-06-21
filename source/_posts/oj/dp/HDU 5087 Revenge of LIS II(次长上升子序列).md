---
title: HDU 5087 Revenge of LIS II(次长上升子序列)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-11-01 22:06:30
---
题意 求一个序列的所有上升子序列中第二长的那个的长度

简单的dp d[i]表示以第i个数结尾的最长上升子序列的长度 c[i]表示到达d[i]的方法数 如序列1 1 2 d[3]=2,c[3]=2 因为选1 3位置和 2 3位置的都可以得到d[3]=2 递推过程很简单 d[i]=max｛d[j]+1｝其中a[i]>a[j]&&i>j

最后看d[1~n]中最大的数出现了几次 出现了不止一次就直接输出否则就减一输出咯

```js 
#include <cstdio>
#include <algorithm>
using namespace std;
const int N = 1005;
int n, a[N];
int d[N], c[N], cas, ans;

int main()
{
    scanf("%d", &cas);
    while (cas--)
    {
        scanf("%d", &n);
        int sum = ans = 0;
        for (int i = 1; i <= n; ++i)
        {
            scanf("%d", &a[i]);
            d[i] = 1, c[i] = 1;
            for (int j = 1; j < i; ++j)
            {
                if (a[j] >= a[i]) continue;
                if (d[j] + 1 > d[i])
                {
                    d[i] = d[j] + 1;
                    c[i] = c[j];
                }
                else if (d[j] + 1 == d[i])    c[i] = 2;
            }
            if(d[i] > ans) ans = d[i];
        }
        for (int i = 1; i <= n; ++i)
            if (d[i] == ans)  sum += c[i];
        printf("%d\n", sum > 1 ? ans : ans - 1);
    }
    return 0;
}
```

# Revenge of LIS II

Problem Description

In computer science, the longest increasing subsequence problem is to find a subsequence of a given sequence in which the subsequence's elements are in sorted order, lowest to highest, and in which the subsequence is as long as possible. This subsequence is not necessarily contiguous, or unique.
---Wikipedia
Today, LIS takes revenge on you, again. You mission is not calculating the length of longest increasing subsequence, but the length of the second longest increasing subsequence.
Two subsequence is different if and only they have different length, or have at least one different element index in the same place. And second longest increasing subsequence of sequence S indicates the second largest one while sorting all the increasing subsequences of S by its length.
Input

The first line contains a single integer T, indicating the number of test cases.
Each test case begins with an integer N, indicating the length of the sequence. Then N integer Ai follows, indicating the sequence.
[Technical Specification]
1. 1 <= T <= 100
2. 2 <= N <= 1000
3. 1 <= Ai <= 1 000 000 000
Output

For each test case, output the length of the second longest increasing subsequence.
Sample Input

3 2 1 1 4 1 2 3 4 5 1 1 2 2 2
Sample Output

1 3 2

HintFor the first sequence, there are two increasing subsequence: [1], [1]. So the length of the second longest increasing subsequence is also 1, same with the length of LIS.