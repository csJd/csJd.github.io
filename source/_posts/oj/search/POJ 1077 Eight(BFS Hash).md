---
title: POJ 1077 Eight(BFS Hash)
author: Deng
tags: 
       - Algorithm

category: 
       - Search
       - OJ

date: 2014-12-18 00:36:52
---
题意 八数码问题

还是八数码问题 只是要输出路径了

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int M = 1000003;
int e[9] = {1, 2, 3, 4, 5, 6, 7, 8, 0};
int x[4] = { -1, 1, 0, 0}, y[4] = {0, 0, -1, 1};
int dir[M], pre[M], h[M], s[M][9];
char pri[5] = "udlr";

int aton(int a[])
{
    int t = 0;
    for(int i = 0; i < 9; ++i)
        t = t * 10 + a[i];
    return t;
}

int search_hash(int a[])
{
    int t = aton(a), p = t % M;
    while(h[p])
    {
        if(h[p] == t) return p;
        ++p;
        if(p >= M) p = 0;
    }
    return p;
}

void print(int p)
{
    if(pre[p])
    {
        print(pre[p]);
        printf("%c", pri[dir[p]]);
    }
}

int bfs()
{
    memset(h, 0, sizeof(h));
    h[aton(s[1]) % M] = aton(s[1]);
    int fro = 1, rear = 2, r, c, k = 0, nr, nc, nk, p;
    int t[9], tmp, cur;

    while(fro < rear)
    {
        memcpy(t, s[fro], sizeof(t));
        cur = search_hash(t);
        if(memcmp(t, e, sizeof(t)) == 0) return cur;
        for(k = 0; t[k];) ++k;
        r = k / 3, c = k % 3;
        for(int i = 0; i < 4; ++i)
        {
            memcpy(t, s[fro], sizeof(t));
            nr = r + x[i], nc = c + y[i], nk = nr * 3 + nc;
            if(nr < 0 || nr > 2 || nc < 0 || nc > 2) continue;
            tmp = t[nk];
            t[nk] = 0;
            t[k] = tmp;
            p = search_hash(t);
            if(h[p] == 0)
            {
                h[p] = aton(t);
                dir[p] = i;
                pre[p] = cur;
                memcpy(s[rear], t, sizeof(t));
                ++rear;
            }
        }
        ++fro;
    }
    return 0;
}

int main()
{
    char c;
    while(~scanf("%d", &s[1][0]))
    {
        memset(dir, 0, sizeof(dir));
        memset(pre, 0, sizeof(pre));
        for(int i = 1; i < 9; ++i)
        {
            scanf(" %c", &c);
            if(c == 'x') s[1][i] = 0;
            else s[1][i] = c - '0';
        }
        int ans = bfs();
        if(ans) print(ans);
        else  printf("unsolvable");
        printf("\n");
    }
    return 0;
}
/*
2 3 4 1 5 0 7 6 8
*/
```

Eight

Description
The 15-puzzle has been around for over 100 years; even if you don't know it by that name, you've seen it. It is constructed with 15 sliding tiles, each with a number from 1 to 15 on it, and all packed into a 4 by 4 frame with one tile missing. Let's call the missing tile 'x'; the object of the puzzle is to arrange the tiles so that they are ordered as:
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 x
where the only legal operation is to exchange 'x' with one of the tiles with which it shares an edge. As an example, the following sequence of moves solves a slightly scrambled puzzle:
1 2 3 4 1 2 3 4 1 2 3 4 1 2 3 4 5 6 7 8 5 6 7 8 5 6 7 8 5 6 7 8 9 x 10 12 9 10 x 12 9 10 11 12 9 10 11 12 13 14 11 15 13 14 11 15 13 14 x 15 13 14 15 x r-> d-> r->
The letters in the previous row indicate which neighbor of the 'x' tile is swapped with the 'x' tile at each step; legal values are 'r','l','u' and 'd', for right, left, up, and down, respectively.
Not all puzzles can be solved; in 1870, a man named Sam Loyd was famous for distributing an unsolvable version of the puzzle, and
frustrating many people. In fact, all you have to do to make a regular puzzle into an unsolvable one is to swap two tiles (not counting the missing 'x' tile, of course).
In this problem, you will write a program for solving the less well-known 8-puzzle, composed of tiles on a three by three
arrangement.

Input

You will receive a description of a configuration of the 8 puzzle. The description is just a list of the tiles in their initial positions, with the rows listed from top to bottom, and the tiles listed from left to right within a row, where the tiles are represented by numbers 1 to 8, plus 'x'. For example, this puzzle
1 2 3 x 4 6 7 5 8
is described by this list:
1 2 3 x 4 6 7 5 8

Output

You will print to standard output either the word ``unsolvable'', if the puzzle has no solution, or a string consisting entirely of the letters 'r', 'l', 'u' and 'd' that describes a series of moves that produce a solution. The string should include no spaces and start at the beginning of the line.

Sample Input

2 3 4 1 5 x 7 6 8

Sample Output

ullddrurdllurdruldr

Source

[South Central USA 1998](http://poj.org/searchproblem?field=source&key=South+Central+USA+1998)