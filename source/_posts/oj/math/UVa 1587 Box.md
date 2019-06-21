---
title: UVa 1587 Box
author: Deng
tags: 
       - Algorithm

category: 
       - Mathematics
       - OJ

date: 2015-01-17 21:55:17
---
大水题一发 弄清长方体的几个面的关系就行了

```js 
#include<cstdio>
#include<algorithm>
using namespace std;
const int N = 6;

struct rec{ int l, w;} r[N];
bool cmp(rec a, rec b)
{
    return a.w < b.w || (a.w == b.w && a.l < b.l);
}

int main()
{
    int a, b, ok;
    while(~scanf("%d%d", &r[0].w, &r[0].l))
    {
        ok = 1;
        if(r[0].w > r[0].l) swap(r[0].w, r[0].l);
        for(int i = 1; i < 6; ++i)
        {
            scanf("%d%d", &r[i].w, &r[i].l);
            if(r[i].w > r[i].l) swap(r[i].w, r[i].l);
        }

        sort(r, r + N, cmp);
        for(int i = 0; i < N; i += 2)
            if(r[i].w != r[i + 1].w || r[i].l != r[i + 1].l) ok = 0;
        if(r[0].w != r[2].w || r[0].l != r[4].w || r[2].l != r[4].l) ok = 0;

        puts(ok ? "POSSIBLE" : "IMPOSSIBLE");
    }
    return 0;
}
```
 Ivan works at a factory that produces heavy machinery. He has a simple job -- he knocks up wooden boxes of different sizes to pack machinery for delivery to the customers. Each box is a rectangular parallelepiped. Ivan uses six rectangular wooden pallets to make a box. Each pallet is used for one side of the box.

![\epsfbox{p3214.eps}](../images/dge.org-external-15-p3214.jpg.png) Joe delivers pallets for Ivan. Joe is not very smart and often makes mistakes -- he brings Ivan pallets that do not fit together to make a box. But Joe does not trust Ivan. It always takes a lot of time to explain Joe that he has made a mistake. Fortunately, Joe adores everything related to computers and sincerely believes that computers never make mistakes. Ivan has decided to use this for his own advantage. Ivan asks you to write a program that given sizes of six rectangular pallets tells whether it is possible to make a box out of them.

##

Input file contains several test cases. Each of them consists of six lines. Each line describes one pallet and contains two integer numbers *w* and *h* (  1![$ \le$](../images/dge.org-external-15-3214img2-.png)*w*, *h*![$ \le$](../images/dge.org-external-15-3214img2-.png)10 000 ) -- width and height of the pallet in millimeters respectively.

##

For each test case, print one output line. Write a single word ` POSSIBLE ' to the output file if it is possible to make a box using six given pallets for its sides. Write a single word ` IMPOSSIBLE ' if it is not possible to do so.

##

1345 2584 2584 683 2584 1345 683 1345 683 1345 2584 683 1234 4567 1234 4567 4567 4321 4322 4567 4321 1234 4321 1234

##

POSSIBLE IMPOSSIBLE