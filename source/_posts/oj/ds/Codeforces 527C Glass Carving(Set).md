---
title: Codeforces 527C Glass Carving(Set)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-03-18 23:26:44
---
题意 一块w/*h的玻璃 对其进行n次切割 每次切割都是垂直或者水平的 输出每次切割后最大单块玻璃的面积

用两个set存储每次切割的位置 就可以比较方便的把每次切割产生和消失的长宽存下来 每次切割后剩下的最大长宽的积就是答案了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 200005;
typedef long long LL;
set<int>::iterator i, j;
set<int> ve, ho;  //记录所有边的位置
int wi[N], hi[N];  //记录存在的边长值

void cut(set<int> &s, int *a, int p)
{
    s.insert(p), i = j = s.find(p);
    --i, ++j, --a[*j - *i];  //除掉被分开的长宽
    ++a[p - *i], ++a[*j - p];  //新产生了两个长宽
}

int main()
{
    int w, n, h, p, mw, mh;
    char s[10];
    while(~scanf("%d%d%d", &w, &h, &n))
    {
        memset(wi, 0, sizeof(wi)), memset(hi, 0, sizeof(hi));
        ve.clear(), ho.clear();
        ve.insert(0), ho.insert(0);
        ve.insert(w), ho.insert(h);
        wi[w] = hi[h] = 1;
        mw = w , mh = h;
        while(n--)
        {
            scanf("%s%d", s, &p);
            if(s[0] == 'V') cut(ve, wi, p);
            else cut(ho, hi, p);
            while(!wi[mw]) --mw;
            while(!hi[mh]) --mh;
            printf("%lld\n", LL(mw)*LL(mh));
        }
    }
    return 0;
}
```
C. Glass Carving

Leonid wants to become a glass carver (the person who creates beautiful artworks by cutting the glass). He already has a rectangular *w*mm ×  *h* mm sheet of glass, a diamond glass cutter and lots of enthusiasm. What he lacks is understanding of what to carve and how.

In order not to waste time, he decided to practice the technique of carving. To do this, he makes vertical and horizontal cuts through the entire sheet. This process results in making smaller rectangular fragments of glass. Leonid does not move the newly made glass fragments. In particular, a cut divides each fragment of glass that it goes through into smaller fragments.

After each cut Leonid tries to determine what area the largest of the currently available glass fragments has. Since there appear more and more fragments, this question takes him more and more time and distracts him from the fascinating process.

Leonid offers to divide the labor — he will cut glass, and you will calculate the area of the maximum fragment after each cut. Do you agree?
Input

The first line contains three integers *w*, *h*, *n* (2 ≤ *w*, *h* ≤ 200 000, 1 ≤ *n* ≤ 200 000).

Next *n* lines contain the descriptions of the cuts. Each description has the form *H y* or *V x*. In the first case Leonid makes the horizontal cut at the distance *y* millimeters (1 ≤ *y* ≤ *h* - 1) from the lower edge of the original sheet of glass. In the second case Leonid makes a vertical cut at distance *x* (1 ≤ *x* ≤ *w* - 1) millimeters from the left edge of the original sheet of glass. It is guaranteed that Leonid won't make two identical cuts.

Output

After each cut print on a single line the area of the maximum available glass fragment in mm2.
Sample test(s)

input 4 3 4 H 2 V 2 V 3 V 1

output 8 4 4 2
input 7 6 5 H 4 V 3 V 5 H 2 V 1

output 28 16 12 6 4

Note

Picture for the first sample test:

![](../images/eforces.com-76f76c4b91db5d63d733cd37c91bce09001259b7-.png)  Picture for the second sample test:  ![](../images/eforces.com-70c98df3646740cda1a5e6455b065203e1953191-.png)