---
title: UVa 1589 Xiangqi(模拟 HDU4121)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Other

date: 2015-01-20 14:07:37
---
题意 给你一个黑方被将军的象棋残局 判断红方是否已经把黑方将死

模拟题 注意细节就行了 看黑方的将是否四个方向都不能走

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 12;

char brd[N][N];
int dx[] = { -1, 1, 0, 0}, dy[] = {0, 0, -1, 1};
int hx[] = { -2, -1, -2, -1, 1, 2, 1, 2};  //马将军
int hy[] = { -1, -2, 1, 2, -2, -1, 2, 1};
int tx[] = { -1, -1, -1, -1, 1, 1, 1, 1};  //马蹩脚
int ty[] = { -1, -1, 1, 1, -1, -1, 1, 1};
int cr[2], cc[2];  //记录炮的坐标

int check(int r, int c)
{
    int i, j, k, tr, tc, cnt;
    if(r < 1 || r > 3 || c < 4 || c > 6) return 1;

    for (j = c - 1; j > 0; --j)   //行被车
    {
        if(brd[r][j])
            if(brd[r][j] == 'R') return 1;
            else break;
    }
    for (j = c + 1; j <= 9; ++j)
    {
        if(brd[r][j])
            if(brd[r][j] == 'R') return 1;
            else break;
    }

    for (i = r - 1; i > 0; --i)  //列被车
    {
        if(brd[i][c])
            if(brd[i][c] == 'R') return 1;
            else break;
    }
    for (i = r + 1; i <= 10; ++i) //列被车或将
    {
        if(brd[i][c])
            if(brd[i][c] == 'R' || brd[i][c] == 'G') return 1;
            else break;
    }

    for(int k = 0; k < 2; ++k)    //被炮将军
    {
        if(cr[k] == r)   //行有炮
        {
            for(j = c - 1, cnt = 0; j > cc[k]; --j) if(brd[r][j]) ++cnt;
            if(cnt == 1) return 1;
            for(j = c + 1, cnt = 0; j < cc[k]; ++j) if(brd[r][j]) ++cnt;
            if(cnt == 1) return 1;
        }
        if(cc[k] == c)   //列有炮
        {
            for(i = r - 1, cnt = 0; i > cr[k]; --i) if(brd[i][c]) ++cnt;
            if(cnt == 1) return 1;
            for(i = r + 1, cnt = 0; i < cr[k]; ++i) if(brd[i][c]) ++cnt;
            if(cnt == 1) return 1;
        }
    }

    for(int k = 0; k < 8; ++k)   //被马将军
    {
        tr = r + hx[k], tc = c + hy[k];
        if(tr < 1 || tr > 10 || tc < 1 || tc > 9) continue;
        if(brd[tr][tc] == 'H' && (!brd[r + tx[k]][c + ty[k]]))
            return 1;
    }

    return 0;
}

int main()
{
    char s[5];
    int n, r, c, x, y;
    while(scanf("%d%d%d", &n, &r, &c), n || r || c)
    {
        memset(brd, 0, sizeof(brd));
        cr[0] = cc[0] = cr[1] = cc[1] = 0;

        while(n--)
        {
            scanf("%s%d%d", s, &x, &y);
            if(s[0] == 'C')
            {
                if(cr[0]) cr[1] = x, cc[1] = y;
                else cr[0] = x, cc[0] = y;
            }
            brd[x][y] = s[0];
        }

        int cnt = 0;
        for(int i = 0; i < 4; ++i)
            cnt += check(r + dx[i], c + dy[i]);
        if(cnt < 4) puts("NO");
        else puts("YES");
    }
    return 0;
}
```

# Xiangqi

Problem Description

Xiangqi is one of the most popular two-player board games in China. The game represents a battle between two armies with the goal of capturing the enemy’s “general” piece. In this problem, you are given a situation of later stage in the game. Besides, the red side has already “delivered a check”. **Your work is to check whether the situation is “checkmate”.**
Now we introduce some basic rules of Xiangqi. Xiangqi is played on a 10×9 board and the pieces are placed on the intersections (points). The top left point is (1,1) and the bottom right point is (10,9). There are two groups of pieces marked by black or red Chinese characters, belonging to the two players separately. During the game, each player in turn moves one piece from the point it occupies to another point. No two pieces can occupy the same point at the same time. A piece can be moved onto a point occupied by an enemy piece, in which case the enemy piece is "captured" and removed from the board. When the general is in danger of being captured by the enemy player on the enemy player’s next move, the enemy player is said to have **"delivered a check"**. If the general's player can make no move to prevent the general's capture by next enemy move, the situation is called **“checkmate”**.
  ![](../images/cn-data-images-4121-5.jpg.png)  We only use 4 kinds of pieces introducing as follows:
![](../images/cn-data-images-4121-1.jpg.png)General: the generals can move and capture **one point** either vertically or horizontally and cannot leave the “**palace**” unless the situation called “**flying general**” (see the figure above). “Flying general” means that one general can “fly” across the board to capture the enemy general if they stand on the same line without intervening pieces.
![](../images/cn-data-images-4121-2.jpg.png)Chariot: the chariots can move and capture vertically and horizontally by any distance, but may not jump over intervening pieces
![](../images/cn-data-images-4121-3.jpg.png)Cannon: the cannons move like the chariots, horizontally and vertically, but capture by jumping **exactly one piece** (whether it is friendly or enemy) over to its target.
![](../images/cn-data-images-4121-4.jpg.png)Horse: the horses have 8 kinds of jumps to move and capture shown in the left figure. However, if there is any pieces lying on a point away from the horse horizontally or vertically it cannot move or capture in that direction (see the figure below), which is called “**hobbling the horse’s leg**”.
  ![](../images/cn-data-images-4121-6.jpg.png)  Now you are given a situation only containing a black general, a red general and several red chariots, cannons and horses, and the red side has delivered a check. Now it turns to black side’s move. Your job is to determine that whether this situation is “checkmate”.
Input

The input contains no more than 40 test cases. For each test case, the first line contains three integers representing the number of red pieces N (2<=N<=7) and the position of the black general. The following n lines contain details of N red pieces. For each line, there are a char and two integers representing the type and position of the piece (type char ‘G’ for general, ‘R’ for chariot, ‘H’ for horse and ‘C’ for cannon). We guarantee that the situation is legal and the red side has delivered the check.
There is a blank line between two test cases. The input ends by 0 0 0.
Output

For each test case, if the situation is checkmate, output a single word ‘YES’, otherwise output the word ‘NO’.
Sample Input

2 1 4 G 10 5 R 6 4 3 1 5 H 4 5 G 10 5 C 7 5 0 0 0
Sample Output

YES NO