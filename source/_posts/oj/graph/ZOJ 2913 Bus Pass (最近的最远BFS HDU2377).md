---
title: ZOJ 2913 Bus Pass (最近的最远BFS HDU2377)
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2014-10-18 22:12:05
---
题意 在所有城市中找一个中心满足这个中心到所有公交站点距离的最大值最小 输出最小距离和满足最小距离编号最小的中心

最基础的BFS 对每个公交站点BFS dis[i]表示编号为i的点到所有公交站点距离的最大值 bfs完所有站点后 dis[i]最小的点就是要求的点咯

```js 
#include<cstdio>
#include<cstring>
#include<queue>
#include<set>
using namespace std;

typedef set<int>::iterator it;
const int N = 10000;
int dis[N], tdis[N], link[N][12];
queue<int> q;
set<int> zone;

void bfs(int o)
{
    memset(tdis, 0, sizeof(tdis));
    tdis[o] = 1;
    q.push(o);
    while(!q.empty())
    {
        int cur = q.front();
        q.pop();
        if(tdis[cur] > dis[cur]) dis[cur] = tdis[cur];
        for(int i = 1; i <= link[cur][0]; ++i)
        {
            int j = link[cur][i];
            if(tdis[j] == 0)  q.push(j), tdis[j] = tdis[cur] + 1;
        }
    }
}

int main()
{
    int cas, nz, nr, id, mz, mr, ans, t;
    scanf("%d", &cas);
    while(cas--)
    {
        zone.clear();
        memset(dis, 0, sizeof(dis));
        scanf("%d%d", &nz, &nr);
        for(int i = 1; i <= nz; ++i)
        {
            scanf("%d %d", &id, &mz);
            link[id][0] = mz;
            zone.insert(id);
            for(int i = 1; i <= mz; ++i)
                scanf("%d", &link[id][i]);
        }

        for(int i = 1; i <= nr; ++i)
        {
            scanf("%d", &mr);
            for(int j = 1; j <= mr; ++j)
            {
                scanf("%d", &t);
                bfs(t);
            }
        }

        it i = zone.begin();
        ans = *i;
        for(++i; i != zone.end(); ++i)
            if(dis[*i] < dis[ans]) ans = *i;
        printf("%d %d\n", dis[ans], ans);
    }
    return 0;
}
```
  Bus Pass    Time Limit:5 Seconds Memory Limit:32768 KB

You travel a lot by bus and the costs of all the seperate tickets are starting to add up.

Therefore you want to see if it might be advantageous for you to buy a bus pass.

The way the bus system works in your country (and also in the Netherlands) is as follows:

when you buy a bus pass, you have to indicate a center zone and a star value. You are allowed to travel freely in any zone which has a distance to your center zone which is less than your star value. For example, if you have a star value of one, you can only travel in your center zone. If you have a star value of two, you can also travel in all adjacent zones, et cetera.

You have a list of all bus trips you frequently make, and would like to determine the minimum star value you need to make all these trips using your buss pass. But this is not always an easy task. For example look at the following figure:

![](../images/cn-onlinejudge-showImage.do-name=0000%2F2913%2FBus_Pass_1.jpg.png)
Here you want to be able to travel from A to B and from B to D. The best center zone is 7400, for which you only need a star value of 4. Note that you do not even visit this zone on your trips!

**Input**

On the first line an integer*t*(1 <=*t*<= 100): the number of test cases. Then for each test case:

One line with two integers*nz*(2 <=*nz*<= 9 999) and*nr*(1 <=*nr*<= 10): the number of zones and the number of bus trips, respectively.

*nz* lines starting with two integers *idi* (1 <= idi <= 9 999) and *mzi* (1 <= *mzi* <= 10), a number identifying the i-th zone and the number of zones adjacent to it, followed by mzi integers: the numbers of the adjacent zones.

*nr* lines starting with one integer *mri* (1 <= *mri* <= 20), indicating the number of zones the ith bus trip visits, followed by *mri* integers: the numbers of the zones through which the bus passes in the order in which they are visited.

All zones are connected, either directly or via other zones.

**Output**

For each test case:

One line with two integers, the minimum star value and the id of a center zone which achieves this minimum star value. If there are multiple possibilities, choose the zone with the lowest number.

**Sample Input**

1
17 2
7400 6 7401 7402 7403 7404 7405 7406
7401 6 7412 7402 7400 7406 7410 7411
7402 5 7412 7403 7400 7401 7411
7403 6 7413 7414 7404 7400 7402 7412
7404 5 7403 7414 7415 7405 7400
7405 6 7404 7415 7407 7408 7406 7400
7406 7 7400 7405 7407 7408 7409 7410 7401
7407 4 7408 7406 7405 7415
7408 4 7409 7406 7405 7407
7409 3 7410 7406 7408
7410 4 7411 7401 7406 7409
7411 5 7416 7412 7402 7401 7410
7412 6 7416 7411 7401 7402 7403 7413
7413 3 7412 7403 7414
7414 3 7413 7403 7404
7415 3 7404 7405 7407
7416 2 7411 7412
5 7409 7408 7407 7405 7415
6 7415 7404 7414 7413 7412 7416

**Sample Output**

4 7400