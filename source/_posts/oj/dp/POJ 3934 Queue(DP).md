---
title: POJ 3934 Queue(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-09-02 09:51:03
---
Queue

Description
Linda is a teacher in ACM kindergarten. She is in charge of n kids. Because the dinning hall is a little bit far away from the classroom, those n kids have to walk in line to the dinning hall every day. When they are walking in line, if and only if two kids can see each other, they will talk to each other. Two kids can see each other if and only if all kids between them are shorter then both of them, or there are no kids between them. Kids do not only look forward, they may look back and talk to kids behind them. Linda don’t want them to talk too much (for it’s not safe), but she also don’t want them to be too quiet(for it’s boring), so Linda decides that she must form a line in which there are exactly m pairs of kids who can see each other. Linda wants to know, in how many different ways can she form such a line. Can you help her?
Note: All kids are different in height.

Input

Input consists of multiple test cases. Each test case is one line containing two integers. The first integer is n, and the second one is m. (0 < n <= 80, 0 <= m <= 10000).
Input ends by a line containing two zeros.

Output

For each test case, output one line containing the reminder of the number of ways divided by 9937.

Sample Input

1 0 2 0 3 2 0 0

Sample Output

1 0 4

题意 linda在一个幼儿园当老师 他要把n个学生排成一列 使只有m对学生能够讲话 当两个学生相邻或者他们之间的所有人都比他们矮时 他们就能够讲话

每个学生的身高都不同

令d[i][j]表示把i个学生排成一列使j对学生能够讲话的方法数

可以把i个学生分成i-1个学生和一个最矮的学生 把这个学生放在i-1个学生中任意两个学生之间都不会影响原来的结果 但是能讲话的学生对数增加了2 有i-2种放法

或者把这个最矮的学生放在两边 这样能讲话的对数只增加了1 有两种放法

所以有转移方程d[i][j]=d[i-1][j-2]/*i-2+d[i-1][j-1]/*2(j>=2)

j<2时只有d[1][0]=1（学生数大于1就有相邻的就能讲话了） 和d[2][1]=2(学生数大于2肯定不止一对能讲话了)两个有值 可以作为初始条件

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 81, M = 10001;
int d[N][M], n, m;
int main()
{
    d[1][0] = 1, d[2][1] = 2;
    for (int i = 1; i < N; ++i)
        for (int j = 2; j < M; ++j)
            d[i][j] = (d[i - 1][j - 2] * (i - 2) + 2 * d[i - 1][j - 1]) % 9937;
            
    while (scanf ("%d%d", &n, &m), n)
        printf ("%d\n", d[n][m]);
        
    return 0;
}
```