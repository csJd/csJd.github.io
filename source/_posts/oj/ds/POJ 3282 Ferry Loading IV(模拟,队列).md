---
title: POJ 3282 Ferry Loading IV(模拟,队列)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2014-09-02 09:51:45
---
题意 汽车通过渡船过河 渡船开始在左边 输入按车辆来的顺序输入河两岸的车 渡船每次运输的汽车的总长度不能超过渡船自己本身的长度 先来的车先走 求轮船至少跨河多少次才能将所有的车辆都运完

简单模拟 建两个队列 分别装左边的车 和右边的车 算出两边各至少需要运输多少次就行了

```js 
#include<cstdio>
#include<cstring>
#include<queue>
using namespace std;
int main()
{
    int cas, lcnt, rcnt, on,n,m,l;
    char s[10];
    scanf ("%d", &cas);
    while (cas--)
    {
        queue<int> le, ri;
        scanf ("%d%d", &n, &m);
        n *= 100;
        while (m--)
        {
            scanf ("%d%s", &l, s);
            if (s[0] == 'l') le.push (l);
            else ri.push (l);
        }
        lcnt = on = 0;
        while (!le.empty())
        {
            while (!le.empty() && on + le.front() < n)
                on += le.front(), le.pop();
            ++lcnt, on = 0;
        }
        rcnt = on = 0;
        while (!ri.empty())
        {
            while (!ri.empty() && on + ri.front() < n)
                on += ri.front(), ri.pop();
            ++rcnt, on = 0;
        }
        if (lcnt > rcnt) printf ("%d\n", 2 * lcnt - 1);
        else printf ("%d\n",2 * rcnt);
    }
    return 0;
}
```

Ferry Loading IV

Description
Before bridges were common, ferries were used to transport cars across rivers. River ferries, unlike their larger cousins, run on a guide line and are powered by the river's current. Cars drive onto the ferry from one end, the ferry crosses the river, and the cars exit from the other end of the ferry.

There is an *l*-meter-long ferry that crosses the river. A car may arrive at either river bank to be transported by the ferry to the opposite bank. The ferry travels continuously back and forth between the banks so long as it is carrying a car or there is at least one car waiting at either bank. Whenever the ferry arrives at one of the banks, it unloads its cargo and loads up cars that are waiting to cross as long as they fit on its deck. The cars are loaded in the order of their arrival; ferry's deck accommodates only one lane of cars. The ferry is initially on the left bank where it broke and it took quite some time to fix it. In the meantime, lines of cars formed on both banks that await to cross the river.

Input

The first line of input contains *c*, the number of test cases. Each test case begins with *l, m*. *m* lines follow describing the cars that arrive in this order to be transported. Each line gives the length of a car (in centimeters), and the bank at which the car arrives ("left" or "right").

Output

For each test case, output one line giving the number of times the ferry has to cross the river in order to serve all waiting cars.

Sample Input

4 20 4 380 left 720 left 1340 right 1040 left 15 4 380 left 720 left 1340 right 1040 left 15 4 380 left 720 left 1340 left 1040 left 15 4 380 right 720 right 1340 right 1040 right

Sample Output

3 3 5 6