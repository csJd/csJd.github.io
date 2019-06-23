---
title: HDU 3080 The plan of city rebuild(除点最小生成树)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-11-16 23:11:50
---
题意 一个城市原来有l个村庄 e1条道路 又增加了n个村庄 e2条道路 后来后销毁了m个村庄 与m相连的道路也销毁了 求使所有未销毁村庄相互连通最小花费 不能连通输出what a pity!

还是很裸的最小生成树 把销毁掉的标记下 然后prim咯 结果是无穷大就是不能连通的

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 300;
int mat[N][N], des[N], d[N], ans, n, m;

void prim()
{
    memset(d, 0x3f, sizeof(d));
    int s = 0;  while(des[s]) ++s;
    d[s] = -1;
    int cur = s, next = n;
    for(int k = 1; k < n - m; ++k)
    {
        for(int i = 0; i < n; ++i)
        {
            if(des[i] || d[i] < 0) continue;
            d[i] = min(d[i], mat[cur][i]);
            if(d[i] < d[next]) next = i;
        }
        ans += d[next], d[next] = -1, cur = next, next = n;
    }
}

int main()
{
    int cas, l, e1, e2, a, b, c;
    scanf("%d", &cas);
    while(cas--)
    {
        memset(mat, 0x3f, sizeof(mat));
        memset(des, 0, sizeof(des));
        scanf("%d %d", &l, &e1);
        for(int i = 0; i < e1; ++i)
        {
            scanf("%d%d%d", &a, &b, &c);
            if(c < mat[a][b]) mat[a][b] = mat[b][a] = c;
        }

        scanf("%d %d", &n, &e2);
        for(int i = 0; i < e2; ++i)
        {
            scanf("%d%d%d", &a, &b, &c);
            if(c < mat[a][b]) mat[a][b] = mat[b][a] = c;
        }

        n = n + l;
        scanf("%d", &m);
        for(int i = 0; i < m; ++i)
        {
            scanf("%d", &a);
            des[a] = 1;
        }

        ans = 0;  prim();
        if(ans < d[n]) printf("%d\n", ans);
        else printf("what a pity!\n");
    }
    return 0;
}
```

# The plan of city rebuild

Problem Description

News comes!~City W will be rebuilt with the expectation to become a center city. There are some villages and roads in the city now, however. In order to make the city better, some new villages should be built and some old ones should be destroyed. Then the officers have to make a new plan, now you , as the designer, have the task to judge if the plan is practical, which means there are roads(direct or indirect) between every two villages(of course the village has not be destroyed), if the plan is available, please output the minimum cost, or output"what a pity!".
Input

Input contains an integer T in the first line, which means there are T cases, and then T lines follow.
Each case contains three parts. The first part contains two integers l(0<l<100), e1, representing the original number of villages and roads between villages(the range of village is from 0 to l-1), then follows e1 lines, each line contains three integers a, b, c (0<=a, b<l, 0<=c<=1000), a, b indicating the village numbers and c indicating the road cost of village a and village b . The second part first contains an integer n(0<n<100), e2, representing the number of new villages and roads(the range of village is from l to l+n-1), then follows e2 lines, each line contains three integers x, y, z (0<=x, y<l+n, 0<=z<=1000), x, y indicating the village numbers and z indicating the road cost of village x and village y. The third part contains an integer m(0<m<l+n), representing the number of deserted villages, next line comes m integers, p1,p2,…,pm,(0<=p1,p2,…,pm<l+n) indicating the village number.
Pay attention: if one village is deserted, the roads connected are deserted, too.
Output

For each test case, If all villages can connect with each other(direct or indirect), output the minimum cost, or output "what a pity!".
Sample Input

2 4 5 0 1 10 0 2 20 2 3 40 1 3 10 1 2 70 1 1 4 1 60 2 2 3 3 3 0 1 20 2 1 40 2 0 70 2 3 0 3 10 1 4 90 2 4 100 0
Sample Output

70 160