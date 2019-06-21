---
title: UVa 10651 Pebble Solitaire (DP 卵石游戏 记忆化搜索)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-26 09:43:30
---
﻿﻿

题意 给你一个长度为12的字符串 由字符'-'和字符'o'组成 其中"-oo"和"oo-"分别可以通过一次转换变为"o--"和"--o" 可以发现每次转换o都少了一个 只需求出给你的字符串做多能转换多少次就行了

令d[s]表示字符串s最多可以转换的次数 若s可以通过一次转换变为字符串t 有d[s]=max(d[s],d[t]+1)

```js 
#include<iostream>
#include<string>
#include<map>
using namespace std;
map<string, int> d;
int n, ans;
string t, S;

int dp (string s)
{
    if (d[s] > 0) return d[s];
    d[s] = 1;
    for (int i = 0; i < 10; ++i)
    {
        if (s[i] == 'o' && s[i + 1] == 'o' && s[i + 2] == '-')
        {
            t = s;
            t[i] = t[i + 1] = '-';
            t[i + 2] = 'o';
            d[s] = max (d[s], dp (t) + 1);
        }
        if (s[i] == '-' && s[i + 1] == 'o' && s[i + 2] == 'o')
        {
            t = s;
            t[i] = 'o';
            t[i + 1] = t[i + 2] = '-';
            d[s] = max (d[s], dp (t) + 1);
        }
    }
    return d[s];
}

int main()
{
    cin >> n;
    while (n--)
    {
        ans = 1;
        cin >> S;
        for (int i = 0; i < 12; ++i)
            if (S[i] == 'o') ans++;
        ans -= dp (S);
        cout << ans << endl;
    }
    return 0;
}
```

**Pebble Solitaire**

Pebble solitaire is an interesting game. This is a game where you are given a board with an arrangement of small cavities, initially all but one occupied by a pebble each. The aim of the game is to remove as many pebbles as possible from the board. Pebbles disappear from the board as a result of a move. A move is possible if there is a straight line of three adjacent cavities, let us call them**A**,**B**, and**C**, with**B**in the middle, where**A**is vacant, but**B**and**C**each contain a pebble. The move constitutes of moving the pebble from**C**to**A**, and removing the pebble in**B**from the board. You may continue to make moves until no more moves are possible.

In this problem, we look at a simple variant of this game, namely a board with twelve cavities located along a line. In the beginning of each game, some of the cavities are occupied by pebbles. Your mission is to find a sequence of moves such that as few pebbles as possible are left on the board.

Input

The input begins with a positive integer**n**on a line of its own. Thereafter**n**different games follow. Each game consists of one line of input with exactly twelve characters, describing the twelve cavities of the board in order. Each character is either**'-'**or**'o'**(The fifteenth character of English alphabet in lowercase). A**'-'**(minus) character denotes an empty cavity, whereas a**'o'**character denotes a cavity with a pebble in it. As you will find in the sample that there may be inputs where no moves is possible.

## Output

For each of the n games in the input, output the minimum number of pebbles left on the board possible to obtain as a result of moves, on a row of its own.

# Sample InputOutput for Sample Input

**5**

**---oo-------**

**-o--o-oo----**

**-o----ooo---**

**oooooooooooo**

**oooooooooo-o**
 
**1**

**2**

**3**

**12**

**1**

****