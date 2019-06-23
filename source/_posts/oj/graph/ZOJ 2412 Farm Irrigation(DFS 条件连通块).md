---
title: ZOJ 2412 Farm Irrigation(DFS 条件连通块)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-10-13 09:52:14
---
题意 两块农田里面的管道可以直接连接的话 他们就可以共用一个水源 有11种农田 上面的管道位置是一定的 给你一个农田矩阵 问至少需要多少水源

DFS的连通块问题 两个相邻农田的管道可以直接连接的话他们就属于一个连通块 题目就是求连通块个数

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 55;
char mat[N][N];
int type[11][4] = {   //对应11种水管类型 按顺时针方向有管道的为1否则为0
    {1, 0, 0, 1}, {1, 1, 0, 0}, {0, 0, 1, 1}, {0, 1, 1, 0},
    {1, 0, 1, 0}, {0, 1, 0, 1}, {1, 1, 0, 1}, {1, 0, 1, 1},
    {0, 1, 1, 1}, {1, 1, 1, 0}, {1, 1, 1, 1}
};

int dfs(int r, int c)
{
    int cur = mat[r][c] - 'A';
    if(cur < 0 || cur > 10) return 0;
    mat[r][c] = 0;           //标记已经访问 0-'A'是小于0的
    int up = mat[r - 1][c] - 'A', dw = mat[r + 1][c] - 'A',
        le = mat[r][c - 1] - 'A', ri = mat[r][c + 1] - 'A';  //4个相邻块的管道类型
    if(up > -1 && up < 11 && type[up][2] && type[cur][0])  dfs(r - 1, c);
    if(dw > -1 && dw < 11 && type[dw][0] && type[cur][2])  dfs(r + 1, c);
    if(le > -1 && le < 11 && type[le][1] && type[cur][3])  dfs(r, c - 1);
    if(ri > -1 && ri < 11 && type[ri][3] && type[cur][1])  dfs(r, c + 1);
    return 1;     //一个连通块中只有一个返回1
}

int main()
{
    int n, m, ans;
    while (scanf("%d%d", &n, &m), n >= 0)
    {
        ans = 0;
        memset(mat, 0, sizeof(mat));
        for(int i = 1; i <= n; ++i)
            scanf("%s", mat[i] + 1);
        for(int i = 1; i <= n; ++i)
            for(int j = 1; j <= m; ++j)
                ans += dfs(i, j);
        printf("%d\n", ans);
    }
    return 0;
}
```

Farm Irrigation    Time Limit:2 Seconds Memory Limit:65536 KB

Benny has a spacious farm land to irrigate. The farm land is a rectangle, and is divided into a lot of samll squares. Water pipes are placed in these squares. Different square has a different type of pipe. There are 11 types of pipes, which is marked from A to K, as Figure 1 shows.
![](../images/cn-onlinejudge-showImage.do-name=0000%2F2412%2F1.gif.png)
Figure 1

Benny has a map of his farm, which is an array of marks denoting the distribution of water pipes over the whole farm. For example, if he has a map

ADC FJK IHE then the water pipes are distributed like  ![](../images/cn-onlinejudge-showImage.do-name=0000%2F2412%2F2.gif.png)
Figure 2

Several wellsprings are found in the center of some squares, so water can flow along the pipes from one square to another. If water flow crosses one square, the whole farm land in this square is irrigated and will have a good harvest in autumn.

Now Benny wants to know at least how many wellsprings should be found to have the whole farm land irrigated. Can you help him?

Note: In the above example, at least 3 wellsprings are needed, as those red points in Figure 2 show.
Input

There are several test cases! In each test case, the first line contains 2 integers M and N, then M lines follow. In each of these lines, there are N characters, in the range of 'A' to 'K', denoting the type of water pipe over the corresponding square. A negative M or N denotes the end of input, else you can assume 1 <= M, N <= 50.

**Output**

For each test case, output in one line the least number of wellsprings needed.

**Sample Input**
2 2 DK HF 3 3 ADC FJK IHE -1 -1 Sample Output 2 3