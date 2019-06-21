---
title: UVa 714 Copying Books(贪心 二分)
author: Deng
tags: 
       - Algorithm

category: 
       - Other
       - OJ

date: 2015-02-03 19:10:03
---
题意 把m数分成k组 使每组数的和的最大值最小 如果有多种分法 靠前的组的和尽量小

关键是找出那个最小的最大值 可以通过二分来找出 开始左端点为m个数中最大的数 右端点为m个数的和 若中点能将m个数分为小于等于k组 比它大的肯定都是可以的 中点变为右端点 否则中点变成左端点

然后就可以贪心逆向模拟了 从后往前每组选择尽量多的数直到剩下的数等于组数

```js 
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 505;
int a[N], d[N], m, k, maxi;
ll s[N], le, ri, mid, mins, t;

int divs()
{
    int last = 0, cnt = 1;
    for(int i = 1; i <= m; ++i)
    {
        if(s[i] - s[last] > mid)
            last = i - 1, ++cnt;
    }
    return cnt;
}

int main()
{
    int cas;
    scanf("%d", &cas);
    while(cas--)
    {
        scanf("%d %d", &m, &k);
        for(int i = maxi = 1; i <= m; ++i)
        {
            scanf("%d", &a[i]);
            s[i] = s[i - 1] + a[i];
            if(a[i] > a[maxi]) maxi = i;
        }

        le = a[maxi], ri = s[m];
        while(le <= ri)
        {
            mid = (le + ri) >> 1;
            if(divs() <= k) mins = mid, ri = mid - 1;
            else le = mid + 1;
        }

        t = 0;
        memset(d, 0, sizeof(d));
        for(int i = m; i > 0; --i)
        {
            if(t + a[i] > mins)
                d[i] = k--, t = 0;
            if(k >= i) while(i > 0) d[--i] = 1;
            t = t + a[i];
        }

        for(int i = 1; i <= m; ++i)
        {
            printf("%d%c", a[i], i < m ? ' ' : '\n');
            if(d[i]) printf("/ ");
        }
    }
    return 0;
}
```

#

****

Before the invention of book-printing, it was very hard to make a copy of a book. All the contents had to be re-written by hand by so called *scribers*. The scriber had been given a book and after several months he finished its copy. One of the most famous scribers lived in the 15th century and his name was Xaverius Endricus Remius Ontius Xendrianus (*Xerox*). Anyway, the work was very annoying and boring. And the only way to speed it up was to hire more scribers.

Once upon a time, there was a theater ensemble that wanted to play famous Antique Tragedies. The scripts of these plays were divided into many books and actors needed more copies of them, of course. So they hired many scribers to make copies of these books. Imagine you have *m* books (numbered ![$1, 2, \dots, m$](../images/dge.org-external-7-714img1.gif.png)) that may have different number of pages ( ![$p_1, p_2, \dots, p_m$](../images/dge.org-external-7-714img2.gif.png)) and you want to make one copy of each of them. Your task is to divide these books among *k* scribes, ![$k \le m$](../images/dge.org-external-7-714img3.gif.png). Each book can be assigned to a single scriber only, and every scriber must get a continuous sequence of books. That means, there exists an increasing succession of numbers ![$0 = b_0 <b_1 < b_2, \dots < b_{k-1} \le b_k = m$](../images/dge.org-external-7-714img4.gif.png)such that *i*-th scriber gets a sequence of books with numbers between *b**i*-1+1 and *b**i*. The time needed to make a copy of all the books is determined by the scriber who was assigned the most work. Therefore, our goal is to minimize the maximum number of pages assigned to a single scriber. Your task is to find the optimal assignment.

##

The input consists of *N* cases. The first line of the input contains only positive integer *N*. Then follow the cases. Each case consists of exactly two lines. At the first line, there are two integers*m* and *k*, ![$1 \le k \le m \le 500$](../images/dge.org-external-7-714img5.gif.png). At the second line, there are integers ![$p_1, p_2, \dots p_m$](../images/dge.org-external-7-714img6.gif.png) separated by spaces. All these values are positive and less than 10000000.

##

For each case, print exactly one line. The line must contain the input succession ![$p_1, p_2, \dots p_m$](../images/dge.org-external-7-714img6.gif.png)divided into exactly *k* parts such that the maximum sum of a single part should be as small as possible. Use the slash character (`/') to separate the parts. There must be exactly one space character between any two successive numbers and between the number and the slash.

If there is more than one solution, print the one that minimizes the work assigned to the first scriber, then to the second scriber etc. But each scriber must be assigned at least one book.

##

2 9 3 100 200 300 400 500 600 700 800 900 5 4 100 100 100 100 100

##

100 200 300 400 500 / 600 700 / 800 900 100 / 100 / 100 / 100 100

#

****

Before the invention of book-printing, it was very hard to make a copy of a book. All the contents had to be re-written by hand by so called *scribers*. The scriber had been given a book and after several months he finished its copy. One of the most famous scribers lived in the 15th century and his name was Xaverius Endricus Remius Ontius Xendrianus (*Xerox*). Anyway, the work was very annoying and boring. And the only way to speed it up was to hire more scribers.

Once upon a time, there was a theater ensemble that wanted to play famous Antique Tragedies. The scripts of these plays were divided into many books and actors needed more copies of them, of course. So they hired many scribers to make copies of these books. Imagine you have *m* books (numbered ![$1, 2, \dots, m$](../images/dge.org-external-7-714img1.gif.png)) that may have different number of pages ( ![$p_1, p_2, \dots, p_m$](../images/dge.org-external-7-714img2.gif.png)) and you want to make one copy of each of them. Your task is to divide these books among *k* scribes, ![$k \le m$](../images/dge.org-external-7-714img3.gif.png). Each book can be assigned to a single scriber only, and every scriber must get a continuous sequence of books. That means, there exists an increasing succession of numbers !['$0 = b_0 <](../images/dge.org-external-7-714img4.gif.png)such that *i*-th scriber gets a sequence of books with numbers between *b**i*-1+1 and *b**i*. The time needed to make a copy of all the books is determined by the scriber who was assigned the most work. Therefore, our goal is to minimize the maximum number of pages assigned to a single scriber. Your task is to find the optimal assignment.

##

The input consists of *N* cases. The first line of the input contains only positive integer *N*. Then follow the cases. Each case consists of exactly two lines. At the first line, there are two integers*m* and *k*, ![$1 \le k \le m \le 500$](../images/dge.org-external-7-714img5.gif.png). At the second line, there are integers ![$p_1, p_2, \dots p_m$](../images/dge.org-external-7-714img6.gif.png) separated by spaces. All these values are positive and less than 10000000.

##

For each case, print exactly one line. The line must contain the input succession ![$p_1, p_2, \dots p_m$](../images/dge.org-external-7-714img6.gif.png)divided into exactly *k* parts such that the maximum sum of a single part should be as small as possible. Use the slash character (`/') to separate the parts. There must be exactly one space character between any two successive numbers and between the number and the slash.

If there is more than one solution, print the one that minimizes the work assigned to the first scriber, then to the second scriber etc. But each scriber must be assigned at least one book.

##

2 9 3 100 200 300 400 500 600 700 800 900 5 4 100 100 100 100 100

##

100 200 300 400 500 / 600 700 / 800 900 100 / 100 / 100 / 100 100