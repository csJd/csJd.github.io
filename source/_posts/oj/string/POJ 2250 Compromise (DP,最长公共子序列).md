---
title: POJ 2250 Compromise (DP,最长公共子序列)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2014-09-02 09:50:38
---
Compromise

**Time Limit:** 1000MS  **Memory Limit:** 65536K **Total Submissions:** 6440  **Accepted:** 2882  Special Judge

Description
In a few months the European Currency Union will become a reality. However, to join the club, the Maastricht criteria must be fulfilled, and this is not a trivial task for the countries (maybe except for Luxembourg). To enforce that Germany will fulfill the criteria, our government has so many wonderful options (raise taxes, sell stocks, revalue the gold reserves,...) that it is really hard to choose what to do.
Therefore the German government requires a program for the following task:
Two politicians each enter their proposal of what to do. The computer then outputs the longest common subsequence of words that occurs in both proposals. As you can see, this is a totally fair compromise (after all, a common sequence of words is something what both people have in mind).
Your country needs this program, so your job is to write it for us.

Input

The input will contain several test cases.
Each test case consists of two texts. Each text is given as a sequence of lower-case words, separated by whitespace, but with no punctuation. Words will be less than 30 characters long. Both texts will contain less than 100 words and will be terminated by a line containing a single '/#'.
Input is terminated by end of file.

Output

For each test case, print the longest common subsequence of words occuring in the two texts. If there is more than one such sequence, any one is acceptable. Separate the words by one blank. After the last word, output a newline character.

Sample Input

die einkommen der landwirte sind fuer die abgeordneten ein buch mit sieben siegeln um dem abzuhelfen muessen dringend alle subventionsgesetze verbessert werden /# die steuern auf vermoegen und einkommen sollten nach meinung der abgeordneten nachdruecklich erhoben werden dazu muessen die kontrollbefugnisse der finanzbehoerden dringend verbessert werden /#

Sample Output

die einkommen der abgeordneten muessen dringend verbessert werden

题意 求两端文本的最长公共子单词序列 直接lcs增量法可以得出 打印路劲也是直接递归就行

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 105, L = 35;
char a[N][L], b[N][L];
int pre[N][N], d[N][N], n, m, la, lb, flag;
void lcs()
{
    memset (d, 0, sizeof (d)); memset (pre, 0, sizeof (pre));
    for (int i = 1; i < la; ++i)
        for (int j = 1; j < lb; ++j)
            if (!strcmp (a[i], b[j]))
            {
                d[i][j] = d[i - 1][j - 1] + 1;
                pre[i][j] = 1;
            }
            else if (d[i - 1][j] > d[i][j - 1])
            {
                d[i][j] = d[i - 1][j];
                pre[i][j] = 2;
            }
            else
            {
                d[i][j] = d[i][j - 1];
                pre[i][j] = 3;
            }
}

void print (int i, int j)
{
    if (pre[i][j] == 1)
    {
        print (i - 1, j - 1);
        if (flag) flag = 0;
        else printf (" ");
        printf ("%s", a[i]);
    }
    else if (pre[i][j] == 2)
        print (i - 1, j);
    else if (pre[i][j] == 3)
        print (i, j - 1);
}

int main()
{
    while (scanf ("%s", a[1]) != EOF)
    {
        la = 1; lb = 0; flag = 1;
        while (scanf ("%s", a[++la]), a[la][0] != '#');
        while (scanf ("%s", b[++lb]), b[lb][0] != '#');
        lcs();
        print (la - 1, lb - 1);
        printf ("\n");
    }
    return 0;
}
```