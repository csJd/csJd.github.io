---
title: UESTC 890 Card Trick(DP 纸牌魔术)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-28 10:43:05
---
题意 给你一些牌 全部正面朝下放桌子上 你选一个起点 翻开那张牌 牌上的数字是几就向前走几步 J,Q,K 都是向前走10步 A向前走11步 知道向前走对应的步数后超过了终点 输入n m 和n个数 代表你以第m张牌为起点 依次掀开了n张牌就不能再掀了 然后同样的牌 Alice以1-10张牌中的任意一个为起点 求Alice最后的终点与你的终点相同的概率

c[i]表示第i张牌的面值 没被掀开的牌的面值都是未知的c[i]=0 可能为2-A中的任意一个 令d[i]表示从你的终点到达第i张牌的概率 那么所有掀开过的牌的概率都为1 然后从终点向前递推 当p[i]=0时 p[i]=sum{p[i+j]} j为2-A中任意一张牌 注意10,j,q,k的时候都是10 最后的答案就是1到10的结果加起来除以10了

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 1500;

int main()
{
    char s[3];
    int n, m, l;
    double p[N], ans;
    while (~scanf ("%d%d", &n, &m))
    {
        memset (p, 0, sizeof (p));
        l = m;

        for (int i = 1; i <= n; ++i)
        {
            scanf ("%s", s);
            p[l] = 1;
            if (s[0]<'A' && s[1]!='0')  l += s[0] - '0';
            else if (s[0] == 'A')  l += 11;
            else l+= 10;
        }

        ans = 0;
        for (int i = l ; i >= 1; --i)
        {
            if (p[i] == 0)
            {
                for (int j = 2; j <= 11; ++j)
                {
                    int t = (j == 10 ? 4 : 1);
                    p[i] += t * p[i + j];
                }
                p[i] /= 13;
            }
            if (i <= 10) ans += p[i];
        }

        printf ("%.8f\n", ans / 10);
    }
    return 0;
}
```

# Card Trick
##### Time Limit: 2999/999MS (Java/Others) Memory Limit: 65432/65432KB (Java/Others)

Submit   Status
I am learning magic tricks to impress my girlfriend Alice. My latest trick is a probabilistic one, i.e. it does work in most cases, but not in every case. To perform the trick, I first shuffle a set of many playing cards and put them all in one line with faces up on the table. Then Alice secretly selects one of the first ten cards (i.e. she choosesx0, a secret number between1and10inclusive) and skips cards repeatedly as follows: after having selected a card at positionxiwith a numberc(xi)on its face, she will select the card at positionxi+1=xi+c(xi). Jack (

```js 
J
```
), Queen (

```js 
Q
```
), and King (

```js 
K
```
) count as10, Ace (

```js 
A
```
) counts as11. You may assume that there are at least ten cards on the table.

Alice stops this procedure as soon as there is no card at positionxi+c(xi). I then perform the same procedure from a randomly selected starting position that may be different from the position selected by Alice. It turns out that often, I end up at the same position. Alice is very impressed by this trick.

However, I am more interested in the underlying math. Given my randomly selected starting position and the card faces of every selected card (including my final one), can you compute the probability that Alice chose a starting position ending up on the same final card? You may assume that her starting position is randomly chosen with uniform probability (between1and10inclusive). I forgot to note the cards that I skipped, so these cards are unknown. You may assume that the card face of every single of the unknown cards is independent of the other card faces and random with uniform probability out of the possible card faces (i.e.

```js 
2
```
-

```js 
10
```
,

```js 
J
```
,

```js 
Q
```
,

```js 
K
```
, and

```js 
A
```
).

![title](../images/u.cn-images-problem-890-2014052312220872010-.png)

*Illustration of first sample input: my starting position is2, so I start selecting that card. Then I keep skipping cards depending on the card's face. This process iterates until there are not enough cards to skip (in this sample:

```js 
Q
```
). The final

```js 
Q
```
card is followed by0to9unknown cards, since

```js 
Q
```
counts as10.*

## Input

For each test case:
## Output

For each test case, print one line containing the probability that Alice chooses a starting position that leads to the same final card. Your output should have an absolute error of at most10−7.

## Sample input and output

Sample Input Sample Output 5 2 2 3 5 3 Q 1 1 A 1 2 A 1 10 A 6 1 2 2 2 2 2 2 7 1 2 2 2 2 2 2 2 3 10 10 J K 0.4871377757023325348071573 0.1000000000000000000000000 0.1000000000000000000000000 0.1748923357025314239697490 0.5830713210321767445117468 0.6279229611115749556280350 0.3346565827603272001891974

﻿﻿