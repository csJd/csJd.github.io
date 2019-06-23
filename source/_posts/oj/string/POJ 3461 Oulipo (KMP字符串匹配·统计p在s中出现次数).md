---
title: POJ 3461 Oulipo (KMP字符串匹配·统计p在s中出现次数)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2014-09-02 09:51:32
---
题意 给你两个字符串p和s 求p在s中出现的次数 很裸的kmp

因为不止匹配一次 每次找到后还要循环j=next[j]的过程 知道到达s的终点

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 10005, M = 1000005;
int next[N], ans, n;
char p[N], s[M];

void kmp ()
{
    int lp = strlen (p), ls = strlen (s);
    int i, j;

    //求next数组
    i = 0;
    j = next[0] = -1;
    while (i < lp)
    {
        if(j == -1 || p[i] == p[j])
            next[++i] = ++j;
        else j = next[j];
    }

    //kmp匹配
    i = j = 0;
    while (i < ls)
    {
        if (j == -1 || s[i] == p[j])
        {
            ++i, ++j;
            if (j == lp) ++ans, j = next[j];
        }
        else j = next[j];
    }
}

int main()
{
    scanf ("%d", &n);
    while (n--)
    {
        ans = 0;
        scanf ("%s%s", p, s);
        kmp ();
        printf ("%d\n", ans);
    }
    return 0;
}
```

Oulipo

Description
The French author Georges Perec (1936–1982) once wrote a book, La disparition, without the letter 'e'. He was a member of the Oulipo group. A quote from the book:
Tout avait Pair normal, mais tout s’affirmait faux. Tout avait Fair normal, d’abord, puis surgissait l’inhumain, l’affolant. Il aurait voulu savoir où s’articulait l’association qui l’unissait au roman : stir son tapis, assaillant à tout instant son imagination, l’intuition d’un tabou, la vision d’un mal obscur, d’un quoi vacant, d’un non-dit : la vision, l’avision d’un oubli commandant tout, où s’abolissait la raison : tout avait l’air normal mais…

Perec would probably have scored high (or rather, low) in the following contest. People are asked to write a perhaps even meaningful text on some subject with as few occurrences of a given “word” as possible. Our task is to provide the jury with a program that counts these occurrences, in order to obtain a ranking of the competitors. These competitors often write very long texts with nonsense meaning; a sequence of 500,000 consecutive 'T's is not unusual. And they never use spaces.

So we want to quickly find out how often a word, i.e., a given string, occurs in a text. More formally: given the alphabet {'A', 'B', 'C', …, 'Z'} and two finite strings over that alphabet, a word *W* and a text *T*, count the number of occurrences of *W* in *T*. All the consecutive characters of W must exactly match consecutive characters of *T*. Occurrences may overlap.

Input

The first line of the input file contains a single number: the number of test cases to follow. Each test case has the following format:

Output

For every test case in the input file, the output should contain a single number, on a single line: the number of occurrences of the word *W* in the text *T*.

Sample Input

3 BAPC BAPC AZA AZAZAZA VERDI AVERDXIVYERDIAN

Sample Output

1 3 0