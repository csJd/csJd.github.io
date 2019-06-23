---
title: UVa 10534 Wavio Sequence ( DP 二分 最长递增子序列 )
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-25 15:18:17
---
题意 求一个序列a某一位的最长递增序列(lis)和最长递减序列(lds)中最小值的最大值

开始直接用DP写了 然后就超时了 后来看到别人说要用二分把时间复杂度优化到O(n/*logn) 果然如此 用一个栈s保存长度为i的LIS的最小尾部s[i] top为栈顶即当前LIS的长度 初始s[1]=a[1] top=1 遍历整个序列 当a[i]>s[top]时 a[i]入栈 in[i]=top 否则 在栈中查找（二分）第一个大于等于a[i]的下标pos 并替换 这样就增加了LIS增长的潜力 in[i]=pos;

in[i]表示以a[i]为尾部的LIS的长度 求lds的过程也是类似

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 10005;
int a[N], in[N], de[N], s[N], m, n, top, pos;

int BinSearch (int k, int le, int ri)
{
    while (le <= ri)
    {
        m = (le + ri) >> 1;
        if (s[m] >= k) ri = m - 1;
        else le = m + 1;
    }
    return ri + 1;
}

void lis()
{
    memset (s, 0, sizeof (s));
    memset (in, 0, sizeof (in));
    s[1] = a[1];
    in[1] = top = 1;
    for (int i = 2; i <= n; ++i)
    {
        if (s[top] < a[i])
        {
            s[++top] = a[i];
            in[i] = top;
        }
        else
        {
            pos = BinSearch (a[i], 1, top);
            s[pos] = a[i];
            in[i] = pos;
        }
    }
}

void lds()
{
    memset (s, 0, sizeof (s));
    memset (de, 0, sizeof (de));
    s[1] = a[n];
    de[n] = top = 1;
    for (int i = n - 1; i >= 1; --i)
    {
        if (s[top] < a[i])
        {
            s[++top] = a[i];
            de[i] = top;
        }
        else
        {
            pos = BinSearch (a[i], 1, top);
            s[pos] = a[i];
            de[i] = pos;
        }
    }
}

int main()
{
    while (scanf ("%d", &n) != EOF)
    {
        for (int i = 1; i <= n; ++i)
            scanf ("%d", &a[i]);
        int ans = 1;
        lis();
        lds();
        for (int i = 1; i <= n; ++i)
        {
            if (min (de[i], in[i]) > ans)
                ans = min (de[i], in[i]);
        }
        printf ("%d\n", ans * 2 - 1);
    }
    return 0;
}
```
还有超时的DP版

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N=10005;
int a[N],c[N],d[N],n;

int dpde(int i)
{
    if(d[i]) return d[i];
    d[i]=1;
    for(int j=i;j<=n;++j)
    {
        if(a[i]>a[j])
            d[i]=max(d[i],dpde(j)+1);
    }
    return d[i];
}

int dpin(int i)
{
    if(c[i]) return c[i];
    c[i]=1;
    for(int j=i;j>=1;--j)
    {
        if(a[i]>a[j])
            c[i]=max(c[i],dpin(j)+1);
    }
    return c[i];
}

int main()
{
    while(scanf("%d",&n)!=EOF)
    {
        for(int i=1;i<=n;++i)
            scanf("%d",&a[i]);
        int ans=1;
        memset(d,0,sizeof(d));
        memset(c,0,sizeof(c));

        for(int i=1;i<=n;++i)
        {
            if(min(dpde(i),dpin(i))>ans)
                ans=min(d[i],c[i]);
        }

        printf("%d\n",ans*2-1);
    }
    return 0;
}
```

**Wavio Sequence**

Wavio is a sequence of integers. It has some interesting properties.

· Wavio is of odd length i.e. **L = 2/*n + 1**.

· The first **(n+1)** integers of Wavio sequence makes a strictly increasing sequence.

· The last **(n+1)** integers of Wavio sequence makes a strictly decreasing sequence.

· No two adjacent integers are same in a Wavio sequence.

For example **1, 2, 3, 4, 5, 4, 3, 2, 0** is an Wavio sequence of length **9**. But **1, 2, 3, 4, 5, 4, 3, 2, 2** is not a valid wavio sequence. In this problem, you will be given a sequence of integers. You have to find out the length of the longest Wavio sequence which is a subsequence of the given sequence. Consider, the given sequence as :

**1 2 3 2 1 2 3 4 3 2 1 5 4 1 2 3 2 2 1**.

Here the longest Wavio sequence is : **1 2 3 4 5 4 3 2 1**. So, the output will be **9**.

Input

The input file contains less than **75** test cases. The description of each test case is given below: Input is terminated by end of file.

Each set starts with a postive integer, **N(1<=N<=10000)**. In next few lines there will be **N** integers.

Output

For each set of input print the length of longest wavio sequence in a line.

# Sample Input Output for Sample Input

**10** **1 2 3 4 5 4 3 2 1 10** **19** **1 2 3 2 1 2 3 4 3 2 1 5 4 1 2 3 2 2 1** **5** **1 2 3 4 5** **** **9** **9** **1**

****

Problemsetter: Md. Kamruzzaman, Member of Elite Problemsetters' Panel