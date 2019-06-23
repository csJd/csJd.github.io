---
title: URAL 1762 Search for a Hiding-Place(数学·模拟)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Mathematics

date: 2015-03-11 21:32:21
---
题意 你在一个n/*m个白色正方形格子组成的矩形的某个顶点格子 你沿着45度角的方向走 到了边界就改变方向90度 每次经过一个格子都改变他原来的颜（白或灰） 求你走到另一个顶点格子时矩形中有多少格子是灰色的

这题可以用公式也可以直接模拟 模拟就是一直向右移n-1位 每次超过边界就可以把边界向右移m-1位 用cnt记录超过边界的次数 那么每次都会经过cnt个已经走过的格子 (每个格子最多经过2次) 答案也就减去2/*cnt 直到刚好到达边界

```js 
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    ll n, m, cur, cnt, ans, k;
    while(~scanf("%lld%lld", &n, &m))
    {
        if (n > m) swap(n, m);
        cur = ans = 1, cnt = 0, k = m;
        while(cur != k)
        {
            cur += n - 1, ans += n - 1 - 2 * cnt;
            if(cur > k) k += m - 1, ++cnt;
        }
        printf("%lld\n", ans);
    }
    return 0;
}
```

## Search for a Hiding-Place

![Problem illustration](../images/-image-get.aspx-76e9e33b-e7d0-476b-b059-544383bb9e8d.png)

Scooby-Doo is fond of adventures. This time he wanted to find a hiding-place in a vampire castle. After a long search, Scooby ended up in a huge rectangular hall with four entrances, one in each corner, through one of which he had entered. The floor was paved with white square tiles. Scooby thought that the hiding-place was under one of these tiles. He started searching for it by turning the tiles over, the grey side up. He began his search from a corner moving at an angle of 45° to the walls. Each time he came to a wall, he made a 90° turn. If he stepped on a grey tile, he turned it back the white side up. The search went on until Scooby reached an entrance at one of the corners. Then, not having found the hiding-place, the tired dog sighed and went out to have a snack.
Given the dimensions of the hall, calculate the total number of tiles that were turned the grey side up at the end of the search.

The only input line contains integers *n* and *m* separated with a space. They are the length and width of the hall measured in tiles (2 ≤ *n*, *m* ≤ 1 000 000).

In the only line output the number of grey tiles in the hall after Scooby-Doo's search.

input output 7 5 11 2 3 3