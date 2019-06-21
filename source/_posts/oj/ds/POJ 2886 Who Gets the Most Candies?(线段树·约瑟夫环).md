---
title: POJ 2886 Who Gets the Most Candies?(线段树·约瑟夫环)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-07-13 20:23:47
---
题意 n个人顺时针围成一圈玩约瑟夫游戏 每个人手上有一个数val[i] 开始第k个人出队 若val[k] < 0 下一个出队的为在剩余的人中向右数 -val[k]个人 val[k] > 0 时向左数val[k]个 第m出队的人可以得到m的约数个数个糖果 问得到最多糖果的人是谁

约瑟夫环问题 n比较大 直接模拟会超时 通过线段树可以让每次出队在O(logN)时间内完成 类似上一道插队的题 线段树维护对应区间还有多少个人没出队 那么当我们知道出队的人在剩余人中排第几个也就可以通过线段树知道他在原始环中排第几个了

至于第几个出队的人糖果最多就是求1...n中约数最多的数 可以利用反素数相关知识

对于任何正整数x 其约数的个数记做g(x) 例如g(1)=1 g(6)=4 如果某个正整数x满足 对于任意i(0<i<x) 都有g(i)<g(x) 则称x为反素数

我直接用的别人的反素数表

```js 
#include <cstdio>
#define lc p<<1, s, mid
#define rc p<<1|1, mid + 1, e
#define mid ((s+e)>>1)
using namespace std;
const int N = 5e5 + 5;
int tot[N * 4], val[N];
//tot维护对应区间还有多少人没出去
char name[N][20];

int ip[] = {1, 2, 4, 6, 12, 24, 36, 48, 60, 120, 180, 240, 360, 720,
            840, 1260, 1680, 2520, 5040, 7560, 10080, 15120, 20160,
            25200, 27720, 45360, 50400, 55440, 83160, 110880,
            166320, 221760, 277200, 332640, 498960, 500001
           }; //反素数

int div[] = {1, 2, 3, 4, 6, 8, 9, 10, 12, 16, 18, 20, 24, 30, 32, 36,
             40, 48, 60, 64, 72, 80, 84, 90, 96, 100, 108, 120,
             128, 144, 160, 168, 180, 192, 200
            };//反素数对应的约数个数

void pushup(int p)
{
    tot[p] = tot[p << 1] + tot[p << 1 | 1];
}

void build(int p, int s, int e)
{
    if(s == e)
    {
        tot[p] = 1;
        return;
    }
    build(lc);
    build(rc);
    pushup(p);
}

int update(int p, int s, int e, int x)
{
    int ret;
    if(s == e)
    {
        tot[p] = 0;
        return s;
    }
    if(x <= tot[p << 1]) ret = update(lc, x);
    else ret = update(rc, x - tot[p << 1]);
    pushup(p);
    return ret;
}

int main()
{
    int n, k, m, r, ans, pos;
    while(scanf("%d%d", &n, &k) != EOF)
    {
        build(1, 1, n);
        for(int i = 1; i <= n; ++i)
            scanf("%s%d", name[i], &val[i]);

        for(int i = 0; ip[i] <= n; ++i)
        {
            m = ip[i];   //m为小于等于n的第一个反素数
            ans = div[i];  //ans对应m的约数个数
        }

        r = n; //还剩r个人
        for(int i = 0; i < m; ++i)
        {
            r--;
            pos = update(1, 1, n, k);
            //pos为剩余序列中排第k的人在原始队列中的位置
            if(!r) break;
            if(val[pos] >= 0)   //顺时针
                k = (k - 1 - 1 + val[pos]) % r + 1;
            //第一个-1是把1开始转换为0开始
            //第二个是删除第k个后现在位于第k-1个 要前进val[pos]步
            //后面的+1是把0开始换回1开始
            else                //逆时针
                k = ((k - 1 + val[pos]) % r + r) % r + 1;
            //逆时针删除第k个后现在还位于第k个 要后退-val[pos]步
        }
        printf("%s %d\n", name[pos], ans);
    }

    return 0;
}
//Last modified :   2015-07-13 19:13
```

Who Gets the Most Candies?

Description
*N* children are sitting in a circle to play a game.

The children are numbered from 1 to *N* in clockwise order. Each of them has a card with a non-zero integer on it in his/her hand. The game starts from the *K*-th child, who tells all the others the integer on his card and jumps out of the circle. The integer on his card tells the next child to jump out. Let *A* denote the integer. If *A* is positive, the next child will be the *A*-th child to the left. If *A* is negative, the next child will be the (−*A*)-th child to the right.

The game lasts until all children have jumped out of the circle. During the game, the *p*-th child jumping out will get *F*(*p*) candies where *F*(*p*) is the number of positive integers that perfectly divide *p*. Who gets the most candies?

Input

There are several test cases in the input. Each test case starts with two integers *N* (0 < *N* ≤ 500,000) and *K* (1 ≤ *K* ≤ *N*) on the first line. The next *N*lines contains the names of the children (consisting of at most 10 letters) and the integers (non-zero with magnitudes within 108) on their cards in increasing order of the children’s numbers, a name and an integer separated by a single space in a line with no leading or trailing spaces.

Output

Output one line for each test case containing the name of the luckiest child and the number of candies he/she gets. If ties occur, always choose the child who jumps out of the circle first.

Sample Input

4 2 Tom 2 Jack 4 Mary -1 Sam 1

Sample Output

Sam 3