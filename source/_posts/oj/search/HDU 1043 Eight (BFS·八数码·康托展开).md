---
title: HDU 1043 Eight (BFS·八数码·康托展开)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Search

date: 2015-07-05 17:00:28
---
题意 输出八数码问题从给定状态到12345678x的路径

用康托展开将排列对应为整数 即这个排列在所有排列中的字典序 然后就是基础的BFS了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 5e5, M = 9;
int x[4] = { -1, 1, 0, 0};
int y[4] = {0, 0, -1, 1};
int fac[] = {1, 1, 2, 6, 24, 120, 720, 5040, 40320};
int puz[N][M], nex[N], dir[N], vis[N], q[N];

int getCantor(int a[])  //康托展开  将排列转化为整数
{
    int ret = 0;
    for(int i = 0; i < M; ++i)
    {
        for(int j = i + 1; j < M; ++j)
            if(a[j] < a[i]) ret += fac[M - i - 1];
    }
    return ret;
}

void bfs()
{
    int t[M] = {1, 2, 3, 4, 5, 6, 7, 8, 0};
    int id = getCantor(t);
    dir[id] = -1;

    memcpy(puz[id], t, sizeof(t));
    memset(vis, 0, sizeof(vis));

    int r, c, k, nr, nc, nk, nid;
    int front = 0, rear = 0;
    q[rear++] = id;
    vis[id] = 1;

    while(front < rear)
    {
        int id = q[front++];
        memcpy(t, puz[id], sizeof(t));
        for(k = 0; t[k]; ++k);  //找0的位置
        r = k / 3, c = k % 3;  //一维转二维

        for(int i = 0; i < 4; ++i)
        {
            nr = r + x[i], nc = c + y[i], nk = nr * 3 + nc;

            if(nr < 0 || nr > 2 || nc < 0 || nc > 2) continue;
            swap(t[k], t[nk]);
            nid = getCantor(t);
            memcpy(puz[nid], t, sizeof(t));
            swap(t[k], t[nk]);

            if(vis[nid]) continue;
            vis[nid] = 1;
            q[rear++] = nid;
            nex[nid] = id;
            dir[nid] = i;
        }
    }
}

int main()
{
    char t[5], sdir[] = "durl";
    int s[M], id;
    bfs();

    while(~scanf("%s", t))
    {
        s[0] = t[0] == 'x' ? 0 : t[0] - '0';
        for(int i = 1; i < M; ++i)
        {
            scanf("%s", t);
            s[i] = t[0] == 'x' ? 0 : t[0] - '0';
        }

        id = getCantor(s);
        if(!vis[id]) puts("unsolvable");
        else
        {
            while(dir[id] >= 0)
            {
                printf("%c", sdir[dir[id]]);
                id = nex[id];
            }
            puts("");
        }
    }
    return 0;
}
//Last modified :   2015-07-05 11:15
```

# Eight

Problem Description

The 15-puzzle has been around for over 100 years; even if you don't know it by that name, you've seen it. It is constructed with 15 sliding tiles, each with a number from 1 to 15 on it, and all packed into a 4 by 4 frame with one tile missing. Let's call the missing tile 'x'; the object of the puzzle is to arrange the tiles so that they are ordered as: 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 x
where the only legal operation is to exchange 'x' with one of the tiles with which it shares an edge. As an example, the following sequence of moves solves a slightly scrambled puzzle:
1 2 3 4 1 2 3 4 1 2 3 4 1 2 3 4 5 6 7 8 5 6 7 8 5 6 7 8 5 6 7 8 9 x 10 12 9 10 x 12 9 10 11 12 9 10 11 12 13 14 11 15 13 14 11 15 13 14 x 15 13 14 15 x r-> d-> r->
The letters in the previous row indicate which neighbor of the 'x' tile is swapped with the 'x' tile at each step; legal values are 'r','l','u' and 'd', for right, left, up, and down, respectively.
Not all puzzles can be solved; in 1870, a man named Sam Loyd was famous for distributing an unsolvable version of the puzzle, and
frustrating many people. In fact, all you have to do to make a regular puzzle into an unsolvable one is to swap two tiles (not counting the missing 'x' tile, of course).
In this problem, you will write a program for solving the less well-known 8-puzzle, composed of tiles on a three by three
arrangement.
Input

You will receive, several descriptions of configuration of the 8 puzzle. One description is just a list of the tiles in their initial positions, with the rows listed from top to bottom, and the tiles listed from left to right within a row, where the tiles are represented by numbers 1 to 8, plus 'x'. For example, this puzzle
1 2 3
x 4 6
7 5 8
is described by this list:
1 2 3 x 4 6 7 5 8
Output

You will print to standard output either the word ``unsolvable'', if the puzzle has no solution, or a string consisting entirely of the letters 'r', 'l', 'u' and 'd' that describes a series of moves that produce a solution. The string should include no spaces and start at the beginning of the line. Do not print a blank line between cases.
Sample Input

2 3 4 1 5 x 7 6 8
Sample Output

ullddrurdllurdruldr