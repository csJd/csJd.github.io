---
title: HDU 3714 Error Curves(3分)
author: Deng
tags: 
       - Algorithm

category: 
       - Mathematics
       - OJ

date: 2015-04-07 17:56:22
---
题意 求分段函数的最低点 每个点函数值为n个 a/*x^2 + b/*x +c (a>=0, |b|<5000, |c|<5000) 函数的最大值

由于a是不小于0的 所以此分段函数的函数图像只可能是类似'V'形的 可以画图观察出来 那么求最小值就可以用三分来解决了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 10005;
const double eps = 1e-9;
int a[N], b[N], c[N], n;

double getr(double x)
{
    double r = a[0] * x * x + b[0] * x + c[0], t;
    for(int i = 1; i < n; ++i)
    {
        t = a[i] * x * x + b[i] * x + c[i];
        if(t > r) r = t;
    }
    return r;
}

int main()
{
    int cas;
    scanf("%d", &cas);
    while(cas--)
    {
        scanf("%d", &n);
        for(int i = 0; i < n; ++i)
            scanf("%d%d%d", &a[i], &b[i], &c[i]);

        double l = 0, r = 1000, m, mm;
        while(l + eps < r)
        {
            m = (l + r) / 2;
            mm = (m + r) / 2;
            if(getr(m) < getr(mm)) r = mm;
            else l = m;
        }
        printf("%.4f\n", getr(l));
    }
    return 0;
}
```

# Error Curves

Problem Description

Josephina is a clever girl and addicted to Machine Learning recently. She
pays much attention to a method called Linear Discriminant Analysis, which
has many interesting properties.
In order to test the algorithm's efficiency, she collects many datasets.
What's more, each data is divided into two parts: training data and test
data. She gets the parameters of the model on training data and test the
model on test data. To her surprise, she finds each dataset's test error curve is just a parabolic curve. A parabolic curve corresponds to a quadratic function. In mathematics, a quadratic function is a polynomial function of the form f(x) = ax2 + bx + c. The quadratic will degrade to linear function if a = 0.
  ![](../images/cn-data-images-3714-1.jpg.png)  It's very easy to calculate the minimal error if there is only one test error curve. However, there are several datasets, which means Josephina will obtain many parabolic curves. Josephina wants to get the tuned parameters that make the best performance on all datasets. So she should take all error curves into account, i.e., she has to deal with many quadric functions and make a new error definition to represent the total error. Now, she focuses on the following new function's minimum which related to multiple quadric functions. The new function F(x) is defined as follows: F(x) = max(Si(x)), i = 1...n. The domain of x is [0, 1000]. Si(x) is a quadric function. Josephina wonders the minimum of F(x). Unfortunately, it's too hard for her to solve this problem. As a super programmer, can you help her?
Input

The input contains multiple test cases. The first line is the number of cases T (T < 100). Each case begins with a number n (n ≤ 10000). Following n lines, each line contains three integers a (0 ≤ a ≤ 100), b (|b| ≤ 5000), c (|c| ≤ 5000), which mean the corresponding coefficients of a quadratic function.
Output

For each test case, output the answer in a line. Round to 4 digits after the decimal point.
Sample Input

2 1 2 0 0 2 2 0 0 2 -4 2
Sample Output

0.0000 0.5000