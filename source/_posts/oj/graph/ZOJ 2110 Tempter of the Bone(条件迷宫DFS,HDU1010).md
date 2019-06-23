---
title: ZOJ 2110 Tempter of the Bone(条件迷宫DFS,HDU1010)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-10-12 21:17:13
---
题意 一只狗要逃离迷宫 可以往上下左右4个方向走 每走一步耗时1s 每个格子只能走一次且迷宫的门只在t时刻打开一次 问狗是否有可能逃离这个迷宫

直接DFS 直道找到满足条件的路径 或者走完所有可能路径都不满足

注意剪枝 当前位置为(r,c) 终点为(ex,ey) 剩下的时间为lt 当前点到终点的直接距离为 d=(ex-r)+(ey-c) 若多走的时间rt=lt-d<0 或为奇数时 肯定是不可能的 可以自己在纸上画一下 每个点只能走一次的图 走弯路的话多走的步数一定为偶数

```js 
#include<cstdio>
#include<cmath>
using namespace std;
int dx[4] = {0, 0, -1, 1};
int dy[4] = { -1, 1, 0, 0};
const int N = 10;
char mat[N][N];
bool ans;
int t, sx, sy, ex, ey;

void dfs(int r, int c, int lt)
{
    if(mat[r][c] == 'D' && lt == 0||ans)  //满足条件或已经满足条件
    {
        ans = true;
        return;
    }
    char tc=mat[r][c];  //保存原来的可能值 有'D'和'.'两种情况
    mat[r][c] = 'X';
    int rt = lt - abs(ex - r) - abs(ey - c);  //比直线到达终点多用的时间
    if(rt >= 0 && rt % 2 == 0)   //剪枝
    for(int i = 0; i < 4; ++i)   //4个方向走
    {
        int x = r + dx[i], y = c + dy[i];
        if(mat[x][y] == '.' || mat[x][y] == 'D')
            dfs(x, y, lt - 1);
    }
    mat[r][c] = tc;  //恢复原状
}

int main()
{
    int n, m;
    while(scanf("%d%d%d", &n, &m, &t), n)
    {
        for(int i = 1; i <= n; ++i)
            scanf("%s", mat[i] + 1);
        for(int i = 1; i <= n; ++i)
            for(int j = 1; j <= m; ++j)
            {
                if(mat[i][j] == 'S') sx = i, sy = j;
                if(mat[i][j] == 'D') ex = i, ey = j;
            }

        ans = false;
        dfs(sx, sy, t);
        printf(ans ? "YES\n" : "NO\n");
    }
    return 0;
}
```
  Tempter of the Bone    Time Limit:2 Seconds Memory Limit:65536 KB

The doggie found a bone in an ancient maze, which fascinated him a lot. However, when he picked it up, the maze began to shake, and the doggie could feel the ground sinking. He realized that the bone was a trap, and he tried desperately to get out of this maze.
The maze was a rectangle with sizes N by M. There was a door in the maze. At the beginning, the door was closed and it would open at the T-th second for a short period of time (less than 1 second). Therefore the doggie had to arrive at the door on exactly the T-th second. In every second, he could move one block to one of the upper, lower, left and right neighboring blocks. Once he entered a block, the ground of this block would start to sink and disappear in the next second. He could not stay at one block for more than one second, nor could he move into a visited block. Can the poor doggie survive? Please help him.

**Input**
The input consists of multiple test cases. The first line of each test case contains three integers N, M, and T (1 < N, M < 7; 0 < T < 50), which denote the sizes of the maze and the time at which the door will open, respectively. The next N lines give the maze layout, with each line containing M characters. A character is one of the following:
'X': a block of wall, which the doggie cannot enter;
'S': the start point of the doggie;
'D': the Door; or
'.': an empty block.
The input is terminated with three 0's. This test case is not to be processed.

**Output**
For each test case, print in one line "YES" if the doggie can survive, or "NO" otherwise.

**Sample Input**
4 4 5
S.X.
..X.
..XD
....
3 4 5
S.X.
..X.
...D
0 0 0

**Sample Output**
NO
YES