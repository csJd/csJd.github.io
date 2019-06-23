---
title: HDU 1058 Humble Numbers（DP,数）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:51:30
---
题意 所有只能被2，3，5，7这4个素数整除的数称为Humble Number 输入n 输出第n个Humble Number

1是第一个humble number 对于一个Humble Number a 有2/*a,3/*a,5/*a,7/*a都是Humble Number 可以以1为基数 依次展开即可得到一定范围内的Humble Number 用i,j,k,l分别记录 2，3，5，7分别乘到了第几个Humble Number 当前在计算第cnt个Humble Number 那么有 hum[cnt] = min ( hum[i] /* 2, hum[j] /* 3, hum[k] /* 5, hum[l] /* 7) 然后对应min的i或j或k或l就加1 当cnt到达了n 结果就出来了

```js 
#include<cstdio>
#include<algorithm>
using namespace std;
const int N = 5843;
int hum[N], cnt, n;
int main()
{
    int i = 1, j = 1, k = 1, l = hum[1] = 1;
    for (cnt = 2; cnt < N; ++cnt)
    {
        hum[cnt] = min ( min(hum[i] * 2, hum[j] * 3), min (hum[k] * 5, hum[l] * 7));
        if (hum[cnt] == hum[i] * 2) ++i;
        if (hum[cnt] == hum[j] * 3) ++j;
        if (hum[cnt] == hum[k] * 5) ++k;
        if (hum[cnt] == hum[l] * 7) ++l;
    }
    while (scanf ("%d", &n), n)
    {
        printf ("The %d", n);
        if (n % 100 != 11 && n % 10 == 1) printf ("st ");
        else if (n % 100 != 12 && n % 10 == 2) printf ("nd ");
        else if (n % 100 != 13 && n % 10 == 3) printf ("rd ");
        else printf ("th ");
        printf ("humble number is %d.\n", hum[n]);
    }
    return 0;
}
```

# Humble Numbers

Problem Description

A number whose only prime factors are 2,3,5 or 7 is called a humble number. The sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 12, 14, 15, 16, 18, 20, 21, 24, 25, 27, ... shows the first 20 humble numbers.
Write a program to find and print the nth element in this sequence
Input

The input consists of one or more test cases. Each test case consists of one integer n with 1 <= n <= 5842. Input is terminated by a value of zero (0) for n.
Output

For each test case, print one line saying "The nth humble number is number.". Depending on the value of n, the correct suffix "st", "nd", "rd", or "th" for the ordinal number nth has to be used like it is shown in the sample output.
Sample Input

1 2 3 4 11 12 13 21 22 23 100 1000 5842 0
Sample Output

The 1st humble number is 1. The 2nd humble number is 2. The 3rd humble number is 3. The 4th humble number is 4. The 11th humble number is 12. The 12th humble number is 14. The 13th humble number is 15. The 21st humble number is 28. The 22nd humble number is 30. The 23rd humble number is 32. The 100th humble number is 450. The 1000th humble number is 385875. The 5842nd humble number is 2000000000.