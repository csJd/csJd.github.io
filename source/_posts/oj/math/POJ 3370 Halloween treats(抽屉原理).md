---
title: POJ 3370 Halloween treats(抽屉原理)
author: Deng
tags: 
       - Algorithm

category: 
       - Mathematics
       - OJ

date: 2014-09-02 09:52:10
---
题意 有c个小孩 n个大人万圣节搞活动 当小孩进入第i个大人家里时 这个大人就会给小孩a[i]个糖果 求小孩去哪几个大人家可以保证得到的糖果总数是小孩数c的整数倍 多种方案满足输出任意一种

用s[i]表示前i个打人给糖果数的总和 令s[0]=0 那么s[i]共有n+1种不同值 而s[i]%c最多有c种不同值 题目说了c<=n 所以s[i]%c肯定会有重复值了

这就是抽屉原理了 n个抽屉放大于n个苹果 至少有一个抽屉有大于等于2个苹果

就把s[i]%c的取值个数(c)看作是抽屉 然后s[i]的取值个数看作是苹果 就有上面的结论了

然后当s[a]%c==s[b]%c的时候 可以知道(s[b]-s[a])%c是等于0的 也就是说得到了一个方案 选从a+1到第b个打人就可以平均分糖果了

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 100010;
int s[N], vis[N];

int main()
{
    int n, c, i, j;
    while (~scanf ("%d%d", &c, &n), c)
    {
        memset (vis, 0, sizeof (vis));
        vis[0] = 1;
        for (i = 1; i <= n; ++i)
        {
            scanf ("%d", &s[i]);
            s[i] = (s[i] + s[i - 1]) % c;
        }

        for (i = 1; i <= n; ++i)
        {
            if (vis[s[i]]) break;
            else
                vis[s[i]] = i+1;
        }

        for (j = vis[s[i]]; j <= i; ++j)
        {
            if (j != i) printf ("%d ", j);
            else printf ("%d\n", j);
        }
    }
    return 0;
}
```

Halloween treats

Description
Every year there is the same problem at Halloween: Each neighbour is only willing to give a certain total number of sweets on that day, no matter how many children call on him, so it may happen that a child will get nothing if it is too late. To avoid conflicts, the children have decided they will put all sweets together and then divide them evenly among themselves. From last year's experience of Halloween they know how many sweets they get from each neighbour. Since they care more about justice than about the number of sweets they get, they want to select a subset of the neighbours to visit, so that in sharing every child receives the same number of sweets. They will not be satisfied if they have any sweets left which cannot be divided.

Your job is to help the children and present a solution.

Input

The input contains several test cases.
The first line of each test case contains two integers ***c*** and ***n*** (*1 ≤ c ≤ n ≤ 100000*), the number of children and the number of neighbours, respectively. The next line contains ***n***space separated integers ***a*1 , ... , *a**n*** (*1 ≤ ai ≤ 100000*), where ***a**i*** represents the number of sweets the children get if they visit neighbour ***i***.

The last test case is followed by two zeros.

Output

For each test case output one line with the indices of the neighbours the children should select (here, index ***i*** corresponds to neighbour ***i*** who gives a total number of ***a*i** sweets). If there is no solution where each child gets at least one sweet print "no sweets" instead. Note that if there are several solutions where each child gets at least one sweet, you may print any of them.

Sample Input

4 5 1 2 3 7 5 3 6 7 11 2 5 13 17 0 0

Sample Output

3 5 2 3 4