---
title: POJ 2299 Ultra-QuickSort(归并排序·树状数组·逆序对)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-04-10 20:27:30
---
题意 给你一个数组求其中逆序对**(i<j&&a[i]>a[j])**的个数

我们来看一个归并排序的过程:
给定的数组为[2, 4, 5, 3, 1]，二分后的数组分别为[2, 4, 5], [1, 3]，假设我们已经完成了子过程，现在进行到该数组的“并”操作：
a: [2, 4, 5]  b: [1, 3]  result:[1]  选取b数组的1 a: [2, 4, 5]  b: [3]  result:[1, 2]  选取a数组的2 a: [4, 5]  b: [3]  result:[1, 2, 3]  选取b数组的3 a: [4, 5]  b: []  result:[1, 2, 3, 4]  选取a数组的4 a: [5]  b: []  result:[1, 2, 3, 4, 5]  选取a数组的5 在执行[2, 4, 5]和[1, 3]合并的时候我们可以发现，当我们将a数组的元素k放入result数组时，result中存在的b数组的元素一定比k小。
在原数组中，b数组中的元素位置一定在k之后，也就是说k和这些元素均构成了逆序对。
那么在放入a数组中的元素时，我们通过计算result中b数组的元素个数，就可以计算出对于k来说，b数组中满足逆序对的个数。
又因为递归的过程中，a数组中和k满足逆序对的数也计算过。则在该次递归结束时，[2, 4, 5, 3, 1]中所有k的逆序对个数也就都统计了。
同理对于a中其他的元素也同样有这样的性质。

```js 
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 500005;
int a[N], t[N], n;
long long cnt;

void merge(int l, int m, int r)
{
    int pl = l, pr = m + 1, p = 0;
    while(pl <= m && pr <= r)
    {
        if(a[pl] <= a[pr]) t[p++] = a[pl++];
        else
        {
            t[p++] = a[pr++];
            cnt += m + 1 - pl;
        }
    }
    while(pl<=m) t[p++] = a[pl++];
    while(pr<=r) t[p++] = a[pr++];
    memcpy(a + l, t, sizeof(int)*p);
}

void mergeSort(int l, int r)
{
    if(l >= r) return;
    int	m = (l + r) >> 1;
    mergeSort(l, m);
    mergeSort(m + 1, r);
    merge(l, m, r);
}

int main()
{
    while(scanf("%d", &n), n)
    {
        for(int i = 0; i < n; ++i)
            scanf("%d", &a[i]);
        cnt = 0;
        mergeSort(0, n - 1);
        printf("%lld\n", cnt);
    }
    return 0;
}
```

当然逆序对也可以通过树状数组或者线段树来求 插入每个时 计算一下有多少个比这个数大的数已经插入了就行了 a[i]的值可能很大 需要离散化 这个离散化只需要排个序

```js 
#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;
const int N = 500005;
int a[N], r[N], n;

bool cmp(int i, int j) //离散化排序用的比较函数
{
    return a[i] < a[j];
}

void add(int x, int v)
{
    while(x <= n)
    {
        a[x] += v;
        x += x & -x;
    }
}

int sum(int x)
{
    int ret = 0;
    while(x)
    {
        ret += a[x];
        x -= x & -x;
    }
    return ret;
}

int main()
{
    while(scanf("%d", &n), n)
    {
        for(int i = 1; i <= n; ++i)
            scanf("%d", &a[i]), r[i] = i;
        sort(r + 1, r + n + 1, cmp); //离散化 用数排序后的下标代替数

        long long ans = 0;
        memset(a, 0, sizeof(a));
        for(int i = 1; i <= n; ++i)
        {
            ans += sum(n) - sum(r[i]);
            add(r[i], 1);
        }
        printf("%lld\n", ans);
    }

    return 0;
}
```

Ultra-QuickSort

Description
![](../images/es-2299_1.jpg.png)In this problem, you have to analyze a particular sorting algorithm. The algorithm processes a sequence of n distinct integers by swapping two adjacent sequence elements until the sequence is sorted in ascending order. For the input sequence
9 1 0 5 4 ,
Ultra-QuickSort produces the output
0 1 4 5 9 .
Your task is to determine how many swap operations Ultra-QuickSort needs to perform in order to sort a given input sequence.

Input

The input contains several test cases. Every test case begins with a line that contains a single integer n < 500,000 -- the length of the input sequence. Each of the the following n lines contains a single integer 0 ≤ a[i] ≤ 999,999,999, the i-th input sequence element. Input is terminated by a sequence of length n = 0. This sequence must not be processed.

Output

For every input sequence, your program prints a single line containing an integer number op, the minimum number of swap operations necessary to sort the given input sequence.

Sample Input

5 9 1 0 5 4 3 1 2 3 0

Sample Output

6 0