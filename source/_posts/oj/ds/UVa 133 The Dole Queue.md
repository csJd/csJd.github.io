---
title: UVa 133 The Dole Queue
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2014-09-02 09:50:08
---
#

****

In a serious attempt to downsize (reduce) the dole queue, The New National Green Labour Rhinoceros Party has decided on the following strategy. Every day all dole applicants will be placed in a large circle, facing inwards. Someone is arbitrarily chosen as number 1, and the rest are numbered counter-clockwise up to N (who will be standing on 1's left). Starting from 1 and moving counter-clockwise, one labour official counts off k applicants, while another official starts from N and moves clockwise, counting m applicants. The two who are chosen are then sent off for retraining; if both officials pick the same person she (he) is sent off to become a politician. Each official then starts counting again at the next available person and the process continues until no-one is left. Note that the two victims (sorry, trainees) leave the ring simultaneously, so it is possible for one official to count a person already selected by the other official.

## Input

Write a program that will successively read in (in that order) the three numbers (N, k and m; k, m > 0, 0 < N < 20) and determine the order in which the applicants are sent off for retraining. Each set of three numbers will be on a separate line and the end of data will be signalled by three zeroes (0 0 0).

## Output

For each triplet, output a single line of numbers specifying the order in which people are chosen. Each number should be in a field of 3 characters. For pairs of numbers list the person chosen by the counter-clockwise official first. Separate successive pairs (or singletons) by commas (but there should not be a trailing comma).

## Sample input

10 4 3 0 0 0

## Sample output

![tex2html_wrap_inline34](../images/dge.org-external-1-133img1.gif.png) 4 ![tex2html_wrap_inline34](../images/dge.org-external-1-133img1.gif.png) 8, ![tex2html_wrap_inline34](../images/dge.org-external-1-133img1.gif.png) 9 ![tex2html_wrap_inline34](../images/dge.org-external-1-133img1.gif.png) 5, ![tex2html_wrap_inline34](../images/dge.org-external-1-133img1.gif.png) 3 ![tex2html_wrap_inline34](../images/dge.org-external-1-133img1.gif.png) 1, ![tex2html_wrap_inline34](../images/dge.org-external-1-133img1.gif.png) 2 ![tex2html_wrap_inline34](../images/dge.org-external-1-133img1.gif.png) 6, ![tex2html_wrap_inline50](../images/dge.org-external-1-133img2.gif.png) 10, ![tex2html_wrap_inline34](../images/dge.org-external-1-133img1.gif.png) 7

where ![tex2html_wrap_inline50](../images/dge.org-external-1-133img2.gif.png) represents a space.

N个人按逆时针从一到n排成一个环 官员1从1开始每次逆时针走过k个人 选出停留地方的人 官员2从n开始每次顺时针走过m个人 选出停留地方的人

若停留地方相同 则只选出一个人 求这些人被选出的顺序 直接模拟就行了;

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 22;
int a[N], n, k, m, r, t1, t2;

int main()
{
    while (scanf ("%d%d%d", &n, &k, &m), n)
    {
        memset (a, -1, sizeof (a));
        t2 = 1;
        t1 = r = n;
        while (r)
        {
            for (int i = 1; i <= k; ++i)
                do t1 = ((t1 == n) ? 1 : t1 + 1); while (a[t1] == 0);
            for (int i = 1; i <= m; ++i)
                do t2 = ((t2 == 1) ? n : t2 - 1); while (a[t2] == 0);

            r = (t1 == t2 ? r - 1 : r - 2);
            if (t1 == t2)  printf ("%3d", t1);
            else printf ("%3d%3d", t1, t2);
            a[t1] = a[t2] = 0;
            if (r)  printf (",");
        }
        printf ("\n");
    }
    return 0;
}
```