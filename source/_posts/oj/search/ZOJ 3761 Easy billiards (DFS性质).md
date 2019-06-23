---
title: ZOJ 3761 Easy billiards (DFS性质)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Search

date: 2015-07-10 08:57:51
---
题意 桌上有n个球 a球撞击b球时 a球停在b球位置 b球保持a球运动 若b球前面再没有球 b球就会掉下桌子 给你n个球的坐标 你可以多次选择某个撞击方向前面还有球的球撞击 问最后桌上至少还剩多少球 并输出你的撞击过程

可以把x坐标或y坐标相同的点当作是连通的 因为可以通过撞击一个球使另一个球掉下桌面

那么容易发现 一个连通块内的m个球总可以经过m-1次撞击后变成只剩一个球 这个球可以是当前连通块中的任意一个 有多少个连通块最后就最少剩下多少球 然后就可以通过一次dfs解决问题 这里利用了dfs的性质 在搜索树中子节点的球总可以通过一次朝父节点撞击使自己位置没有球 撞击方向只用比较坐标就行了 然后从叶子到根输出就行了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 2005;
int x[N], y[N], vis[N];
int dir[N], p[N], n, m;

void dfs(int i)
{
    vis[i] = 1;
    for(int j = 0; j < n; ++j)
    {
        if(!vis[j] && (x[j] == x[i] || y[j] == y[i]))
        {
            dfs(j);    //j和i属于一个连通块且j还没被访问 对j深搜
            if(x[j] == x[i])  //记录路径 上下左右分别用0，1，2，3表示
                dir[j] = y[j] < y[i] ? 0 : 1;
            else dir[j] = x[j] < x[i] ? 3 : 2;
            p[m++] = j;
        }
    }
}

int main()
{
    while(~scanf("%d", &n))
    {
        for(int i = 0; i < n; ++i)
            scanf("%d%d", &x[i], &y[i]);

        memset(vis, 0, sizeof(vis));
        int cnt = m = 0;
        for(int i = 0; i < n; ++i)
            if(!vis[i]) ++cnt, dfs(i);

        printf("%d\n", cnt);
        char sdir[][10] = {"UP", "DOWN", "LEFT", "RIGHT"};
        for(int i = 0; i < m; ++i)
        {
            int j = p[i];
            printf("(%d, %d) %s\n", x[j], y[j], sdir[dir[j]]);
        }
    }

    return 0;
}
//Last modified :   2015-07-10 08:26
```
  Easy billiards    Time Limit:2 Seconds Memory Limit:65536 KB Special Judge

Edward think a game of billiards is too long and boring. So he invented a new game called Easy billiards.

Easy billiards has N balls on a brimless rectangular table in the beginning, and your goal is try to make the number of balls on the table as least as possible by several hit under the following rules:

1: The direction you hit the balls should parallel to the tables border.

2: If ball A crashed into ball B, ball B will moves in the same direction of ball A before the crashing, and ball A will stop in the place of ball B before the crashing.

3: If ball C is moving and there are no balls in front of ball C, it will runs out of the tables border, that means ball C is out of the table.

4: You can choose arbitrary ball on the table to hit, but on a hit, you can't let the ball you choose to hit runs out of the tables border. In another word, a ball could runs out of the table if and only if it was crashed by another ball in a hitting.

Now, Edward wants to know the least number of balls remained on the table after several hits, and how.

There are multiple test cases. For each test cases, in the first line, there is an integer N, which means the number of the balls on the table. There are following N lines, each line contains two integers Xi and Yi, which means the coordinate of ball I. (0<=N<=2000, 0<=Xi, Yi<=10^8)

#### Output

For each test cases, you should output the least number of balls on the first line. And you should output several lines to show the order of hits following the first line, each line should contains the coordinate of the ball you choose to hit and the direction you hit. (LEFT,RIGHT,UP,DOWN).

#### Sample Input

4 0 0 2 0 4 0 2 2 9 1 1 2 1 3 1 1 2 2 2 3 2 1 3 2 3 3 3

#### Sample output

1 (2, 2) DOWN (4, 0) LEFT (2, 0) LEFT 1 (1, 3) DOWN (1, 2) DOWN (2, 3) DOWN (2, 2) DOWN (3, 3) DOWN (3, 2) DOWN (3, 1) LEFT (2, 1) LEFT