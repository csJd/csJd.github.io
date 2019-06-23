---
title: UVa 489 Hangman Judge(字符串)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2014-09-02 09:50:11
---
#

****

In ``Hangman Judge,'' you are to write a program that judges a series of Hangman games. For each game, the answer to the puzzle is given as well as the guesses. Rules are the same as the classic game of hangman, and are given as follows:

Your task as the ``Hangman Judge'' is to determine, for each game, whether the contestant wins, loses, or fails to finish a game.

## Input

Your program will be given a series of inputs regarding the status of a game. All input will be in lower case. The first line of each section will contain a number to indicate which round of the game is being played; the next line will be the solution to the puzzle; the last line is a sequence of the guesses made by the contestant. A round number of -1 would indicate the end of all games (and input).

## Output

The output of your program is to indicate which round of the game the contestant is currently playing as well as the result of the game. There are three possible results:

You win. You lose. You chickened out.

## Sample Input

1 cheese chese 2 cheese abcdefg 3 cheese abcdefgij -1

## Sample Output

Round 1 You win. Round 2 You chickened out. Round 3 You lose.

一个字符串的游戏 有一个你不知道的字符串 你开始有7条命 然后你每次猜一个字母 若你猜的字母在原字符串中 原字符串就去掉所有那个字母 否则你失去一条命 如果你在命耗完之前原字符串中所有的字母都被猜出 则你赢 如果你在命耗完了原字符串中还有字母没被猜出 则你输 如果你在命没耗完原字符串中也还有字母没被猜出 视为你放弃

给你另一个字符串作为你猜的顺序 判断你是否能赢：

```js 
//UVa489 Hangman
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 1000;
char a[N], b[N];
int cas, la, lb, i, j;
int main()
{
    while (scanf ("%d", &cas), cas + 1)
    {
        scanf ("%s %s", a, b);
        la = strlen (a);
        lb = strlen (b);
        int ca[26] = {0}, left = 7;
        for (i = 0; i < la; ++i)
            ++ca[a[i] - 'a'];
        for (i = 0; i < lb; ++i)
            if (ca[b[i] - 'a'] != 0)
            {
                ca[b[i] - 'a'] = 0;
                for (j = 0; j < 26; ++j)
                    if (ca[j] != 0) break;
                if (j == 26)
                {
                    printf ("Round %d\nYou win.\n", cas);
                    break;
                }
            }
            else if (--left == 0)
            {
                printf ("Round %d\nYou lose.\n", cas);
                break;
            }
        if (i == lb) printf ("Round %d\nYou chickened out.\n", cas);
    }
    return 0;
}
```