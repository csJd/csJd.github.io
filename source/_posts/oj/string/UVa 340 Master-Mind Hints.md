---
title: UVa 340 Master-Mind Hints
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2014-09-02 09:50:15
---
#

****

MasterMind is a game for two players. One of them, *Designer*, selects a secret code. The other,*Breaker*, tries to break it. A code is no more than a row of colored dots. At the beginning of a game, the players agree upon the length *N* that a code must have and upon the colors that may occur in a code.

In order to break the code, Breaker makes a number of guesses, each guess itself being a code. After each guess Designer gives a hint, stating to what extent the guess matches his secret code.

In this problem you will be given a secret code ![tex2html_wrap_inline35](../images/dge.org-external-3-340img1.gif.png) and a guess ![tex2html_wrap_inline37](../images/dge.org-external-3-340img2.gif.png) , and are to determine the hint. A hint consists of a pair of numbers determined as follows.

A *match* is a pair (*i*,*j*), ![tex2html_wrap_inline41](../images/dge.org-external-3-340img3.gif.png) and ![tex2html_wrap_inline43](../images/dge.org-external-3-340img4.gif.png) , such that ![tex2html_wrap_inline45](../images/dge.org-external-3-340img5.gif.png) . Match (*i*,*j*) is called *strong*when *i* = *j*, and is called *weak* otherwise. Two matches (*i*,*j*) and (*p*,*q*) are called *independent* when*i* = *p* if and only if *j* = *q*. A set of matches is called *independent* when all of its members are pairwise independent.

Designer chooses an independent set *M* of matches for which the total number of matches and the number of strong matches are both maximal. The hint then consists of the number of strong followed by the number of weak matches in *M*. Note that these numbers are uniquely determined by the secret code and the guess. If the hint turns out to be (*n*,0), then the guess is identical to the secret code.

## Input

The input will consist of data for a number of games. The input for each game begins with an integer specifying *N* (the length of the code). Following these will be the secret code, represented as *N* integers, which we will limit to the range 1 to 9. There will then follow an arbitrary number of guesses, each also represented as *N* integers, each in the range 1 to 9. Following the last guess in each game will be *N* zeroes; these zeroes are not to be considered as a guess.

Following the data for the first game will appear data for the second game (if any) beginning with a new value for *N*. The last game in the input will be followed by a single zero (when a value for*N* would normally be specified). The maximum value for *N* will be 1000.

## Output

The output for each game should list the hints that would be generated for each guess, in order, one hint per line. Each hint should be represented as a pair of integers enclosed in parentheses and separated by a comma. The entire list of hints for each game should be prefixed by a heading indicating the game number; games are numbered sequentially starting with 1. Look at the samples below for the *exact* format.

## Sample Input

4 1 3 5 5 1 1 2 3 4 3 3 5 6 5 5 1 6 1 3 5 1 3 5 5 0 0 0 0 10 1 2 2 2 4 5 6 6 6 9 1 2 3 4 5 6 7 8 9 1 1 1 2 2 3 3 4 4 5 5 1 2 1 3 1 5 1 6 1 9 1 2 2 5 5 5 6 6 6 7 0 0 0 0 0 0 0 0 0 0 0

## Sample Output

Game 1: (1,1) (2,0) (1,2) (1,2) (4,0) Game 2: (2,4) (3,2) (5,0) (7,0)

猜数字游戏的提示 统计猜的数字有多少个数字位置正确 有多少个数字在答案中出现但是位置不正确 每个字符只能匹配一次

```js 
#include<cstdio>
#include<algorithm>
#include<map>
using namespace std;
const int N = 1005;
int  a[N], b[N], c, d;
int main()
{
    int n, cas = 0;
    while (scanf ("%d", &n), n)
    {
        map<int, int> aa;
        printf ("Game %d:\n", ++cas);
        for (int i = 0; i < n; ++i)
        {
            scanf ("%d", &a[i]);
            ++aa[a[i]];
        }
        while (1)
        {
            map<int, int> bb;
            for (int i = c = d = 0; i < n; ++i)
            {
                scanf ("%d", &b[i]);
                ++bb[b[i]];
                if (a[i] == b[i]) c++;
            }
            if (!b[0]) break;
            for (int i = 1; i <= 9; ++i)
                d += min (aa[i], bb[i]);
            printf ("    (%d,%d)\n", c, d - c);
        }
    }
    return 0;
}
```