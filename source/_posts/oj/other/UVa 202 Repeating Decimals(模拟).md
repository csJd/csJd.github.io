---
title: UVa 202 Repeating Decimals(模拟)
author: Deng
tags: 
       - Algorithm

category: 
       - Other
       - OJ

date: 2015-01-18 10:17:16
---
大水题 模拟在草稿纸上算除法的过程→_→

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 3005;
int a[N], v[N];

int main()
{
    int n, m, cnt;
    while(~scanf("%d%d", &n, &m))
    {
        cnt = 0;
        memset(v, 0, sizeof(v));
        printf("%d/%d = %d.", n, m, n / m);
        n = n % m;
        while(!v[n])
        {
            a[++cnt] = (n * 10) / m;
            v[n] = cnt;
            n = n * 10 % m;
        }
        for(int i = 1; i <= cnt && i < 51; ++i)
        {
            if(i == v[n]) printf("(");
            printf("%d", a[i]);
            if(i == 50) printf("...");
        }
        printf(")\n   %d = number of digits in repeating cycle\n\n", cnt - v[n] + 1);
    }
    return 0;
}
```

#

****

The decimal expansion of the fraction 1/33 is ![tex2html_wrap_inline43](../images/dge.org-external-2-202img1.gif.png) , where the ![tex2html_wrap_inline45](../images/dge.org-external-2-202img2.gif.png) is used to indicate that the cycle 03 repeats indefinitely with no intervening digits. In fact, the decimal expansion of every rational number (fraction) has a repeating cycle as opposed to decimal expansions of irrational numbers, which have no such repeating cycles.

Examples of decimal expansions of rational numbers and their repeating cycles are shown below. Here, we use parentheses to enclose the repeating cycle rather than place a bar over the cycle.

![tabular23](../images/dge.org-external-2-202img3.gif.png)

Write a program that reads numerators and denominators of fractions and determines their repeating cycles.

For the purposes of this problem, define a repeating cycle of a fraction to be the first minimal length string of digits to the right of the decimal that repeats indefinitely with no intervening digits. Thus for example, the repeating cycle of the fraction 1/250 is 0, which begins at position 4 (as opposed to 0 which begins at positions 1 or 2 and as opposed to 00 which begins at positions 1 or 4).

## Input

Each line of the input file consists of an integer numerator, which is nonnegative, followed by an integer denominator, which is positive. None of the input integers exceeds 3000. End-of-file indicates the end of input.

## Output

For each line of input, print the fraction, its decimal expansion through the first occurrence of the cycle to the right of the decimal or 50 decimal places (whichever comes first), and the length of the entire repeating cycle.

In writing the decimal expansion, enclose the repeating cycle in parentheses when possible. If the entire repeating cycle does not occur within the first 50 places, place a left parenthesis where the cycle begins - it will begin within the first 50 places - and place ``...)" after the 50th digit.

Print a blank line after every test case.

## Sample Input

76 25 5 43 1 397

## Sample Output

76/25 = 3.04(0) 1 = number of digits in repeating cycle 5/43 = 0.(116279069767441860465) 21 = number of digits in repeating cycle 1/397 = 0.(00251889168765743073047858942065491183879093198992...) 99 = number of digits in repeating cycle