---
title: POJ 1167 (DFS·剪枝·公交时刻表)
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2015-08-03 16:30:48
---
题意 你记录了[0, 59]这个时间段内到达你所在站牌的所有公交的到这个站牌的时间 对于每路公交

1. 同一路公交的到站时间间隔是相同的

2. 每路公交在这个时间段至少到达两次

3. 最多有17路公交

4. 两个不同路的公交的第一次到站时间和到站时间间隔都可能是相同滴

5. 你在这个时间段内的记录是完整的

求最少用多少路公交可以让你的记录合法

由于每路公交至少到站两次 那么第一次到站时间是肯定小于30的 而且到站时间间隔肯定要大于第一次到站的时间 那么可以根据你的记录生成所有可能合法的公交线路 最后dfs找出最少的公交线路 使这些线路刚好完全覆盖你的记录 注意dfs过程中的剪枝

```js 
#include <cstdio>
#include <cstring>
#include <algorithm>

using namespace std;
const int N = 1505;
int cnt[N], n, m, ans;

struct route //定义公交线路结构体
{
    int start, interval, times;
    route() {}
    route(int s, int i, int t): start(s), interval(i), times(t) {}
    bool operator< (const route &r) const {
        return times > r.times;
    }
} r[N];

bool ok(int time, int inter)
{
    while(time < 60)
    {
        if(!cnt[time]) return false;
        time += inter;
    }
    return true;
}

//从第k条线路开始匹配 当前已经匹配num个记录  用了sum个公交线路
void dfs(int k, int num, int sum)
{
    if(num == n)
    {
        if(sum < ans) ans = sum;
        return;
    }

    for(int i = k; i < m; ++i)
    {
        if(sum + (n - num) / r[i].times >= ans) return;
        //剪枝 r是按趟数从大到小排序的  所以最少还需要(n - num) / r[i].times个线路
        if(!ok(r[i].start, r[i].interval)) continue;
        for(int j = r[i].start; j < 60; j += r[i].interval) --cnt[j];
        dfs(i, num + r[i].times, sum + 1);
        //i之前的线路之前已经搜索过了
        for(int j = r[i].start; j < 60; j += r[i].interval) ++cnt[j]; //回溯
    }
}

int main()
{
    int t;
    while(~scanf("%d", &n))
    {
        memset(cnt, 0, sizeof(cnt));
        for(int i = 0; i < n; ++i)
            scanf("%d", &t), ++cnt[t]; //记录每个时刻出现多少公交

        m = 0;//生成以时刻i为首班  两班间隔时间为j的所有满足的公交线路
        for(int i = 0; i < 30; ++i)
            for(int j = i + 1; j < 60 - i; ++j)
                if(ok(i, j)) r[m++] = route(i, j, 1 + (59 - i) / j);

        sort(r, r + m);
        ans = 17;
        dfs(0, 0, 0);
        printf("%d\n", ans);
    }
    return 0;
}
```

The Buses

Description
A man arrives at a bus stop at 12:00. He remains there during 12:00-12:59. The bus stop is used by a number of bus routes. The man notes the times of arriving buses. The times when buses arrive are given.

Find the schedule with the fewest number of bus routes that must stop at the bus stop to satisfy the input data. For each bus route, output the starting time and the interval.

Input

Your program is to read from standard input. The input contains a number n (n <= 300) telling how many arriving buses have been noted, followed by the arrival times in ascending order.

Output

Your program is to write to standard output. The output contains one integer, which is the fewest number of bus routes.

Sample Input

17 0 3 5 13 13 15 21 26 27 29 37 39 39 45 51 52 53

Sample Output

3

Source

[IOI 1994](http://poj.org/searchproblem?field=source&key=IOI+1994)