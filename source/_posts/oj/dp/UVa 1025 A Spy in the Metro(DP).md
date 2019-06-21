---
title: UVa 1025 A Spy in the Metro(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2015-02-06 16:08:26
---
题意 某城市的地铁有n个车站 编号1到n 有m1辆车向右开 给出m1个从车站1出发的时间 m2辆车向左开 给出m2个从车站n出发的时间 t[i]为火车从第i个车站开到第i+1（或相反）个车站需要的时间 Maria在车站1 她需要恰在时刻T到达第n个车站 求她的最小总车站等待时间

基础的多阶段决策DP 令d[i][j]表示时刻j在i号车站剩下的最小总等待时间 每种状态有3种选择

1. 等一个单位时间**d[i][j+1]+1;**

2. 当前站此时有往左的车并搭乘**d[i-1][j+t[i-1]];**

3.当前站此时有往右的车并搭乘**d[i+1][j+t[i]].**

每个点三种选择选最优的就行了 边界条件为**d[n][T]=0** 其它状态均初始化为INF

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 55, M = 205;
int t[N], d[N][M];  //j时刻在i号车站剩下的最小总等待时间
bool l[N][M], r[N][M];  //j时刻在i号车站是否有往左(右)的车

int main()
{
    int n, m, ti, cur, cas = 0;

    while(~scanf("%d", &n), n)
    {
        scanf("%d", &ti);
        memset(l, 0, sizeof(l)), memset(r, 0, sizeof(r));
        for(int i = 1; i < n; ++i) scanf("%d", &t[i]);

        scanf("%d", &m);  //cur时刻车站j是否有往右的车
        for(int i = 1; i <= m; ++i)
        {
            scanf("%d", &cur);
            for(int j = 1; j <= n; ++j)
                r[j][cur] = 1, cur += t[j];
        }
        scanf("%d", &m);  //cur时刻车站j是否有往左的车
        for(int i = 1; i <= m; ++i)
        {
            scanf("%d", &cur);
            for(int j = n; j >= 1; --j)
                l[j][cur] = 1, cur += t[j - 1];
        }

        memset(d, 0x3f, sizeof(d));
        d[n][ti] = 0;
        for(int j = ti - 1; j >= 0; --j)
        {
            for(int i = 1; i <= n; ++i)
            {
                d[i][j] = d[i][j + 1] + 1;   //在i车站等1单位时间
                if(l[i][j]) d[i][j] = min(d[i][j], d[i - 1][j + t[i - 1]]);  //往左
                if(r[i][j]) d[i][j] = min(d[i][j], d[i + 1][j + t[i]]);  //往右
            }
        }

        printf("Case Number %d: ", ++cas);
        if(d[1][0] > ti) puts("impossible");
        else printf("%d\n", d[1][0]);
    }
    return 0;
}
```

Secret agent Maria was sent to Algorithms City to carry out an especially dangerous mission. After several thrilling events we find her in the first station of Algorithms City Metro, examining the time table. The Algorithms City Metro consists of a single line with trains running both ways, so its time table is not complicated.

Maria has an appointment with a local spy at the last station of Algorithms City Metro. Maria knows that a powerful organization is after her. She also knows that while waiting at a station, she is at great risk of being caught. To hide in a running train is much safer, so she decides to stay in running trains as much as possible, even if this means traveling backward and forward. Maria needs to know a schedule with minimal waiting time at the stations that gets her to the last station in time for her appointment. You must write a program that finds the total waiting time in a best schedule for Maria.

The Algorithms City Metro system has *N* stations, consecutively numbered from 1 to *N*. Trains move in both directions: from the first station to the last station and from the last station back to the first station. The time required for a train to travel between two consecutive stations is fixed since all trains move at the same speed. Trains make a very short stop at each station, which you can ignore for simplicity. Since she is a very fast agent, Maria can always change trains at a station even if the trains involved stop in that station at the same time.

![\epsfbox{p2728.eps}](../images/dge.org-external-10-p2728.gif.png)

##

The input file contains several test cases. Each test case consists of seven lines with information as follows. **Line 1.** The integer *N* ( 2![$ \le$](../images/dge.org-external-10-2728img2.gif.png)*N*![$ \le$](../images/dge.org-external-10-2728img2.gif.png)50), which is the number of stations. **Line 2.** The integer *T* ( 0![$ \le$](../images/dge.org-external-10-2728img2.gif.png)*T*![$ \le$](../images/dge.org-external-10-2728img2.gif.png)200), which is the time of the appointment. **Line 3.** *N* - 1 integers: *t*1, *t*2,..., *t*N - 1 ( 1![$ \le$](../images/dge.org-external-10-2728img2.gif.png)*t*i![$ \le$](../images/dge.org-external-10-2728img2.gif.png)70), representing the travel times for the trains between two consecutive stations: *t*1 represents the travel time between the first two stations, *t*2 the time between the second and the third station, and so on. **Line 4.** The integer *M*1 ( 1![$ \le$](../images/dge.org-external-10-2728img2.gif.png)*M*1![$ \le$](../images/dge.org-external-10-2728img2.gif.png)50), representing the number of trains departing from the first station. **Line 5.** *M*1 integers: *d*1, *d*2,..., *d*M1 ( 0![$ \le$](../images/dge.org-external-10-2728img2.gif.png)*d*i![$ \le$](../images/dge.org-external-10-2728img2.gif.png)250 and *d*i < *d*i + 1), representing the times at which trains depart from the first station. **Line 6.** The integer *M*2 ( 1![$ \le$](../images/dge.org-external-10-2728img2.gif.png)*M*2![$ \le$](../images/dge.org-external-10-2728img2.gif.png)50), representing the number of trains departing from the *N*-th station. **Line 7.** *M*2 integers: *e*1, *e*2,..., *e*M2 ( 0![$ \le$](../images/dge.org-external-10-2728img2.gif.png)*e*i![$ \le$](../images/dge.org-external-10-2728img2.gif.png)250 and *e*i < *e*i + 1) representing the times at which trains depart from the *N*-th station.

The last case is followed by a line containing a single zero.

##

For each test case, print a line containing the case number (starting with 1) and an integer representing the total waiting time in the stations for a best schedule, or the word ` impossible ' in case Maria is unable to make the appointment. Use the format of the sample output.

##

4 55 5 10 15 4 0 5 10 20 4 0 5 10 15 4 18 1 2 3 5 0 3 6 10 12 6 0 3 5 7 12 15 2 30 20 1 20 7 1 3 5 7 11 13 17 0

##

Case Number 1: 5 Case Number 2: 0 Case Number 3: impossible