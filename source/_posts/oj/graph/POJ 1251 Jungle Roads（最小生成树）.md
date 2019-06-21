---
title: POJ 1251 Jungle Roads（最小生成树）
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2014-10-23 09:56:24
---
题意 有n个村子 输入n 然后n-1行先输入村子的序号和与该村子相连的村子数t 后面依次输入t组s和tt s为村子序号 tt为与当前村子的距离 求链接所有村子的最短路径

还是裸的最小生成树咯

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N=30,M=1000;
int par[N],n,m,ans;
struct edge{int u,v,w;} e[M];
bool cmp(edge a,edge b) {return a.w<b.w;}

int Find(int x)
{
    int r=x,tmp;
    while(par[r]>=0) r=par[r];
    while(x!=r)
    {
        tmp=par[x];
        par[x]=r;
        x=tmp;
    }
    return r;
}

void Union (int u,int v)
{
    int ru=Find(u),rv=Find(v),tmp=par[ru]+par[rv];
    if(par[ru]<par[rv])
        par[rv]=ru,par[ru]=tmp;
    else
        par[ru]=rv,par[rv]=tmp;
}

void kruskal()
{
    memset(par,-1,sizeof(par));
    int cnt=0;
    for(int i=1;i<=m;++i)
    {
        int u=e[i].u,v=e[i].v;
        if(Find(u)!=Find(v))
        {
            ++cnt;
            ans+=e[i].w;
            Union(u,v);
        }
        if(cnt>=n-1) break;
    }
}

int main()
{
    char s[2]; int t,tt;
    while(scanf("%d",&n),n)
    {
        m=0;
        for(int i=1;i<n;++i)
        {
            scanf("%s%d",s,&t);
            for(int j=1;j<=t;++j)
            {
                scanf("%s%d",s,&tt);
                e[++m].u=i,e[m].v=s[0]-'A'+1,e[m].w=tt;
            }
        }

        sort(e+1,e+m+1,cmp);
        ans=0; kruskal();
        printf("%d\n",ans);
    }
    return 0;
}
```
Jungle Roads

Description

![](../images/es-1251_1.jpg.png)
The Head Elder of the tropical island of Lagrishan has a problem. A burst of foreign aid money was spent on extra roads between villages some years ago. But the jungle overtakes roads relentlessly, so the large road network is too expensive to maintain. The Council of Elders must choose to stop maintaining some roads. The map above on the left shows all the roads in use now and the cost in aacms per month to maintain them. Of course there needs to be some way to get between all the villages on maintained roads, even if the route is not as short as before. The Chief Elder would like to tell the Council of Elders what would be the smallest amount they could spend in aacms per month to maintain roads that would connect all the villages. The villages are labeled A through I in the maps above. The map on the right shows the roads that could be maintained most cheaply, for 216 aacms per month. Your task is to write a program that will solve such problems.

Input

The input consists of one to 100 data sets, followed by a final line containing only 0. Each data set starts with a line containing only a number n, which is the number of villages, 1 < n < 27, and the villages are labeled with the first n letters of the alphabet, capitalized. Each data set is completed with n-1 lines that start with village labels in alphabetical order. There is no line for the last village. Each line for a village starts with the village label followed by a number, k, of roads from this village to villages with labels later in the alphabet. If k is greater than 0, the line continues with data for each of the k roads. The data for each road is the village label for the other end of the road followed by the monthly maintenance cost in aacms for the road. Maintenance costs will be positive integers less than 100. All data fields in the row are separated by single blanks. The road network will always allow travel between all the villages. The network will never have more than 75 roads. No village will have more than 15 roads going to other villages (before or after in the alphabet). In the sample input below, the first data set goes with the map above.

Output

The output is one integer per line for each data set: the minimum cost in aacms per month to maintain a road system that connect all the villages. Caution: A brute force solution that examines every possible set of roads will not finish within the one minute time limit.

Sample Input

9 A 2 B 12 I 25 B 3 C 10 H 40 I 8 C 2 D 18 G 55 D 1 E 44 E 2 F 60 G 38 F 0 G 1 H 35 H 1 I 35 3 A 2 B 10 C 40 B 1 C 20 0

Sample Output

216 30