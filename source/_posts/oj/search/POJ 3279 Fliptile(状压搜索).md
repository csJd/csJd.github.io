---
title: POJ 3279 Fliptile(状压搜索)
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2015-03-27 11:21:29
---
题意 把一个n/*m矩阵中的1全部变成0所需的最少步数 改变某个格子时其相邻的4个格子也一起改变

只要知道第一行改变了哪些后面的就都确定了 因为上一行对应位置是1的位置必须改变 上一行是0的一定不能改变

所以可以根据第一行的状态推出后面所有行的状态 直到最后一行结束 如果最后一行不是全0的话 这种状态就是不可行的

那么我们只用枚举第一行的所有状态就够了 m<=16 用二进制可以比较方便的枚举 对应位是1就表示改变状态 0就不改变 最后打印改变状态次数最少的可行结果就行了

```js 
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 16;
int g[N][N], t[N][N], f[N][N];
int cnt, n, m;
int x[4] = {0, 0, -1, 1};
int y[4] = { -1, 1, 0, 0};

void flip(int i, int j)
{
    ++cnt, f[i][j] = 1;
    t[i][j] = !t[i][j];
    for(int k = 0; k < 4; ++k)
        if(i + x[k] > -1 && j + y[k] > -1)
            t[i + x[k]][j + y[k]] ^= 1;
}

bool ok(int k)
{
    cnt = 0;
    memcpy(t, g, sizeof(t));
    for(int j = 0; j < m; ++j)
        if(k & (1 << (m - 1 - j))) flip(0, j);

    for(int i = 1; i < n; ++i)
        for(int j = 0; j < m; ++j)
            if(t[i - 1][j]) flip(i, j);

    for(int j = 0; j < m; ++j)
        if(t[n - 1][j]) return false;
    return true;
}

int main()
{
    int ans, p;
    while(~scanf("%d%d", &n, &m))
    {
        for(int i = 0; i < n; ++i)
            for(int j = 0; j < m; ++j)
                scanf("%d", &g[i][j]);
        ans = n * m + 1, p = -1;
        for(int i = 0; i < (1 << m); ++i)
            if(ok(i) && cnt < ans) ans = cnt, p = i;

        memset(f, 0, sizeof(f));
        if(p >= 0)
        {
            ok(p);
            for(int i = 0; i < n; ++i)
                for(int j = 0; j < m; ++j)
                    printf("%d%c", f[i][j], j < m - 1 ? ' ' : '\n');
        }
        else puts("IMPOSSIBLE");
    }
    return 0;
}
```

Fliptile

Description
Farmer John knows that an intellectually satisfied cow is a happy cow who will give more milk. He has arranged a brainy activity for cows in which they manipulate an *M* × *N* grid (1 ≤ *M* ≤ 15; 1 ≤ *N* ≤ 15) of square tiles, each of which is colored black on one side and white on the other side.

As one would guess, when a single white tile is flipped, it changes to black; when a single black tile is flipped, it changes to white. The cows are rewarded when they flip the tiles so that each tile has the white side face up. However, the cows have rather large hooves and when they try to flip a certain tile, they also flip all the adjacent tiles (tiles that share a full edge with the flipped tile). Since the flips are tiring, the cows want to minimize the number of flips they have to make.

Help the cows determine the minimum number of flips required, and the locations to flip to achieve that minimum. If there are multiple ways to achieve the task with the minimum amount of flips, return the one with the least lexicographical ordering in the output when considered as a string. If the task is impossible, print one line with the word "IMPOSSIBLE".

Input

Line 1: Two space-separated integers: *M* and *N*
Lines 2..*M*+1: Line *i*+1 describes the colors (left to right) of row i of the grid with *N* space-separated integers which are 1 for black and 0 for white

Output

Lines 1..*M*: Each line contains *N* space-separated integers, each specifying how many times to flip that particular location.

Sample Input

4 4 1 0 0 1 0 1 1 0 0 1 1 0 1 0 0 1

Sample Output

0 0 0 0 1 0 0 1 1 0 0 1 0 0 0 0