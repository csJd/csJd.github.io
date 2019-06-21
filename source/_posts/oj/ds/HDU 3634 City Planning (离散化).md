---
title: HDU 3634 City Planning (离散化)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-08-14 09:32:37
---
题意 给你n个矩形 每个矩形都有自己的value 你可以任意改变矩形的表里关系 被覆盖的地方的value取最表层的 求总value的最大值

刚看了扫描线 感觉这个可以用扫描线做就直接写了 其实直接离散化就行了 因为最多也就20个矩形 那坐标最多也就40个 那我们对坐标进行离散化 然后将矩形按value从小到大一个个的放 暴力更新覆盖格子的value 最后直接将2n /* 2n个小格子的value加起来就行了

```js 
#include <cstdio>
#include <cstring>
#include <algorithm>

using namespace std;
typedef long long ll;
const int N = 50;
int y[N], x[N], n, m;
ll val[N][N];

struct Rect
{
    int x1, y1, x2, y2, v;
    bool operator< (const Rect &r) const{
        return v < r.v;
    }
} r[N];

int fid(int a[], int k){
    return lower_bound(a, a + m, k) - a;
}

int main()
{
    int T, x1, y1, x2, y2, cas = 0;
    scanf("%d", &T);
    while(T--)
    {
        scanf("%d", &n);
        for(int i = m = 0; i < n; ++i, m += 2)
        {
            scanf("%d%d%d%d%d", &r[i].x1, &r[i].y1, &r[i].x2, &r[i].y2, &r[i].v);
            x[m] = r[i].x1, x[m + 1] = r[i].x2;
            y[m] = r[i].y1, y[m + 1] = r[i].y2;
        }
        sort(r, r + n); //将value小的大楼放前面
        sort(x, x + m); //离散化x
        sort(y, y + m); //离散化y

        memset(val, 0, sizeof(val));
        for(int i = 0; i < n; ++i)
        {
            x1 = fid(x, r[i].x1), x2 = fid(x, r[i].x2);  //获得x离散化后的坐标
            y1 = fid(y, r[i].y1), y2 = fid(y, r[i].y2);  //获得y离散化后的坐标
            for(int j = x1; j < x2; ++j)
                for(int k = y1; k < y2; ++k)  val[j][k] = r[i].v;
        }

        ll ans = 0;
        for(int i = 0; i < m - 1; ++i)
            for(int j = 0; j < m - 1; ++j)
                ans += val[i][j] * (x[i + 1] - x[i]) * (y[j + 1] - y[j]);
        printf("Case %d: %I64d\n", ++cas, ans);
    }
    return 0;
}
```
还有扫描线a的代码 用扫描线的时候是将矩形value从大到小排序 每放置一个矩形 看面积改变了多少 然后就用value乘以这个改变的面积

```js 
#include <cstdio>
#include <cstring>
#include <algorithm>
#define lc p<<1, s, mid
#define rc p<<1|1, mid+1, e
#define mid ((s+e)>>1)
using namespace std;
typedef long long ll;
const int N = 50;
int y[N], flag[N * 2], len[N * 2];

struct Rect
{
    int x1, y1, x2, y2, v;
    bool operator< (const Rect &r) const
    {
        return v > r.v;
    }
} r[N];

struct SLine
{
    int x, y1, y2, flag;
    SLine() {}
    SLine(int xx, int a, int b, int f):
        x(xx), y1(a), y2(b), flag(f) {}
    bool operator< (const SLine &s) const
    {
        return x < s.x;
    }
} line[N];

void build()
{
    memset(flag, 0, sizeof(flag));
    memset(len, 0, sizeof(len));
}

void pushup(int p, int s, int e)
{
    if(flag[p]) len[p] = y[e] - y[s - 1];
    else if(s == e) len[p] = 0;
    else len[p] = len[p << 1] + len[p << 1 | 1];
}

void update(int p, int s, int e, int l, int r, int v)
{
    if(l <= s && e <= r)
    {
        flag[p] += v;
        pushup(p, s, e);
        return;
    }
    if(l <= mid) update(lc, l, r, v);
    if(r > mid) update(rc, l, r, v);
    pushup(p, s, e);
}

int main()
{
    int T, n, m, cas = 0;
    int x1, y1, x2, y2, v;
    ll ans;
    scanf("%d", &T);
    while(T--)
    {
        scanf("%d", &n);
        for(int i = 0; i < n; ++i)
            scanf("%d%d%d%d%d", &r[i].x1, &r[i].y1, &r[i].x2, &r[i].y2, &r[i].v);
        sort(r, r + n);
        ll ans = 0, last = 0, area;

        for(int i = m = 0; i < n; ++i)
        {
            build();
            area = 0;
            y[m] = r[i].y1, y[m + 1] = r[i].y2;
            line[m++] = SLine(r[i].x1, r[i].y1, r[i].y2, 1);
            line[m++] = SLine(r[i].x2, r[i].y1, r[i].y2, -1);
            sort(y, y + m);
            sort(line, line + m);

            for(int j = 0; j < m - 1; ++j)
            {
                int l = lower_bound(y, y + m, line[j].y1) - y;
                int r = lower_bound(y, y + m, line[j].y2) - y;
                update(1, 1, m, l + 1, r, line[j].flag);
                area += len[1] * (line[j + 1].x - line[j].x);
            }

            ans += (area - last) * r[i].v;
            last = area;
        }
        printf("Case %d: %I64d\n", ++cas, ans);
    }
    return 0;
}
```

# City Planning

Problem Description

After many years, the buildings in HDU has become very old. It need to rebuild the buildings now. So Mr dragon (the president of HDU's logistics department ) ask Mr Wan (a very famous engineer) for help.
Mr Wan only draw one building on a construction design drawings(all the buildings are rectangle and each edge of buildings' is paraller or perpendicular to others buildings' edge ). And total draw n drawings (all the drawings have same width and length . And bottomleft point is (0, 0)). Due to possible overlap of conditions, so when they build a new building, they should to remove all the overlapping part of it. And for each building, HDU have a jury evaluate the value per unit area. Now Mr dragon want to know how to arrange the order of build these buildings can make the highest value.
Input

The first line of input is a number T which indicate the number of cases . (1 < T < 3000);
Each test case will begin with a single line containing a single integer n (where 1 <= n <= 20).
Next n line will contain five integers x1, y1, x2, y2 ,value . x1,y1 is bottomleft point and x2,y2 is topright point , value is the value of the buildings' unit area.((0 <= x1, y1, x2, y2 <= 10000) (x1 < x2, && y1 < y2) (1 <= value <= 22)
Output

For each case. You just ouput the highest value in one line.
Sample Input

1 3 1 1 10 10 4 4 4 15 5 5 7 8 20 30 6
Sample Output

Case 1: 2047