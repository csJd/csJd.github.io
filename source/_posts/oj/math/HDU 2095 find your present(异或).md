---
title: HDU 2095 find your present(异或)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Mathematics

date: 2014-12-01 19:16:01
---
题意 求一组数中只出现过奇数次的数 输入保证只有一个数满足

知道一个数与自己的异或等于0 与0的异或等于自己就行咯

```js 
#include<cstdio>
using namespace std;
int main()
{
    int n, t, ans;
    while(scanf("%d", &n), n)
    {
        ans = 0;
        for(int i = 1; i <= n; ++i)
        {
            scanf("%d", &t);
            ans = ans ^ t;
        }
        printf("%d\n", ans);
    }
    return 0;
}
```

# find your present (2)

Problem Description

In the new year party, everybody will get a "special present".Now it's your turn to get your special present, a lot of presents now putting on the desk, and only one of them will be yours.Each present has a card number on it, and your present's card number will be the one that different from all the others, and you can assume that only one number appear odd times.For example, there are 5 present, and their card numbers are 1, 2, 3, 2, 1.so your present will be the one with the card number of 3, because 3 is the number that different from all the others.
Input

The input file will consist of several cases.
Each case will be presented by an integer n (1<=n<1000000, and n is odd) at first. Following that, n positive integers will be given in a line, all integers will smaller than 2^31. These numbers indicate the card numbers of the presents.n = 0 ends the input.
Output

For each case, output an integer in a line, which is the card number of your present.
Sample Input

5 1 1 3 2 2 3 1 2 1 0
Sample Output

3 2