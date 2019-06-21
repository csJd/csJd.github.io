---
title: UVa 12657 Boxes in a Line(双向链表的应用)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2014-09-02 09:50:55
---
# 题意 开始有n个盒子按1到n的顺序排列 对这些盒子进行m次操作 每次为把x移到y的左边 右边 交换x,y 颠倒顺序中的一个

求操作完成后所有奇数位原盒子序号的和;

直接模拟肯定会超时 用stl中的链表也超时 只能用数组自己模拟一个双向链表了 le[i],ri[i]分别表示第i个盒子左边盒子的序号和右边盒子的序号 代码中有注释

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 100005;
int le[N], ri[N], n, m;
typedef long long ll;

void link (int l, int r)              //连接l和r，l在左边
{
    le[r] = l;  ri[l] = r;
}

int main()
{
    int cas = 0, op, x, y, t;
    while (scanf ("%d%d", &n, &m) != EOF)
    {
        for (int i = 1; i <= n; ++i)
            ri[i] = i + 1, le[i] = i - 1;
        ri[n] = 0, le[0] = n, ri[0] = 1;
        int flag = 0;                      //判断是否翻转
        while (m--)
        {
            scanf ("%d", &op);
            if (op == 4) flag = !flag;
            else
            {
                scanf ("%d%d", &x, &y);
                if (flag && op != 3) op = 3 - op; //翻转后移动操作就相反了
                if (ri[y] == x && op == 3)        //方便后面判断交换是否相邻
                    t = x, x = y, y = t;
                if ( (op == 1 && le[y] == x) || (op == 2 && ri[y] == x))  continue;

                if (op == 1)                      //x移到y右边
                    link (le[x], ri[x]), link (le[y], x), link (x, y);
                else if (op == 2)                 //x移到y左边
                    link (le[x], ri[x]), link (x, ri[y]), link (y, x);
                else  if (y == ri[x])             //op==3&&x,y相邻
                    link (le[x], y), link (x, ri[y]), link (y, x);
                else                              //不相邻
                {
                    int ry = ri[y], ly = le[y];
                    link (le[x], y), link (y, ri[x]), link (ly, x), link (x, ry);
                }
            }
        }

        t = 0; ll ans = 0;
        for (int i = 1; i <= n; ++i)
        {
            t = ri[t];
            if (i % 2) ans += t;
        }
        if (n % 2 == 0 && flag)    //n为偶数且翻转过 故求的恰为偶数位的和
            ans = (ll) n / 2 * (1 + n) - ans;
        printf ("Case %d: %lld\n", ++cas, ans);
    }
    return 0;
}
```

# Boxes in a Line

You have n boxes in a line on the table numbered 1 . . . n from left to right. Your task is to simulate 4
kinds of commands:
• 1 X Y : move box X to the left to Y (ignore this if X is already the left of Y )
• 2 X Y : move box X to the right to Y (ignore this if X is already the right of Y )
• 3 X Y : swap box X and Y
• 4: reverse the whole line.
Commands are guaranteed to be valid, i.e. X will be not equal to Y .
For example, if n = 6, after executing 1 1 4, the line becomes 2 3 1 4 5 6. Then after executing
2 3 5, the line becomes 2 1 4 5 3 6. Then after executing 3 1 6, the line becomes 2 6 4 5 3 1.
Then after executing 4, then line becomes 1 3 5 4 6 2

## Input

There will be at most 10 test cases. Each test case begins with a line containing 2 integers n, m
(1 ≤ n, m ≤ 100, 000). Each of the following m lines contain a command.

## Output

For each test case, print the sum of numbers at odd-indexed positions. Positions are numbered 1 to n
from left to right.

## Sample Input

6 4
1 1 4
2 3 5
3 1 6
4
6 3
1 1 4
2 3 5
3 1 6
100000 1
4

## Sample Output

Case 1: 12
Case 2: 9
Case 3: 2500050000