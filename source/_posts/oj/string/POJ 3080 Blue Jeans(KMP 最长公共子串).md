---
title: POJ 3080 Blue Jeans(KMP 最长公共子串)
author: Deng
tags: 
       - Algorithm

category: 
       - String
       - OJ

date: 2014-09-02 09:50:59
---
Blue Jeans

Description
The Genographic Project is a research partnership between IBM and The National Geographic Society that is analyzing DNA from hundreds of thousands of contributors to map how the Earth was populated.
As an IBM researcher, you have been tasked with writing a program that will find commonalities amongst given snippets of DNA that can be correlated with individual survey information to identify new genetic markers.
A DNA base sequence is noted by listing the nitrogen bases in the order in which they are found in the molecule. There are four bases: adenine (A), thymine (T), guanine (G), and cytosine (C). A 6-base DNA sequence could be represented as TAGACC.
Given a set of DNA base sequences, determine the longest series of bases that occurs in all of the sequences.

Input

Input to this problem will begin with a line containing a single integer n indicating the number of datasets. Each dataset consists of the following components:

Output

For each dataset in the input, output the longest base subsequence common to all of the given base sequences. If the longest common subsequence is less than three bases in length, display the string "no significant commonalities" instead. If multiple subsequences of the same longest length exist, output only the subsequence that comes first in alphabetical order.

Sample Input

3 2 GATACCAGATACCAGATACCAGATACCAGATACCAGATACCAGATACCAGATACCAGATA AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA 3 GATACCAGATACCAGATACCAGATACCAGATACCAGATACCAGATACCAGATACCAGATA GATACTAGATACTAGATACTAGATACTAAAGGAAAGGGAAAAGGGGAAAAAGGGGGAAAA GATACCAGATACCAGATACCAGATACCAAAGGAAAGGGAAAAGGGGAAAAAGGGGGAAAA 3 CATCATCATCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC ACATCATCATAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA AACATCATCATTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT

Sample Output

no significant commonalities AGATAC CATCATCAT

题意 给你n个DNA串 求它们的长度最大的公共子串 如果有多个 输出字典序最小的 长度小于3的不算

每个DNA串的长度都是60 可以从子串长度为60依次递减 并枚举所有该长度子串 当某个长度的子串也为其它n-1个串的子串时 就是我们要的答案了

判断是否为其它DNA串的子串直接kmp就行了

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 15, M = 65, L = 60;

bool kmp (char s[], char p[])
{
    int next[M];
    next[0] = -1, next[1] = 0;
    int plen = strlen (p), i = 0, j,slen = strlen (s);
    while (++i < plen - 1)
    {
        j = next[i];
        while (j != -1 && p[i] != p[j])
            j = next[j];
        next[i + 1] = j + 1;
    }

    i = j = 0;
    while (i < slen && j < plen)
    {
        if (j == -1 || s[i] == p[j]) ++i, ++j;
        else j = next[j];
    }
    return j == plen;
}

int main()
{
    int cas, n, flag;
    char s[N][M], tans[M],ans[M];
    scanf ("%d", &cas);
    while (cas--)
    {
        scanf ("%d", &n);
        for (int i = flag = 0; i < n; ++i)
            scanf ("%s", s[i]);
        for (int lans = L; lans > 2 && flag == 0; --lans)
        {
            strcpy(ans,"ZZ");
            for (int i = 0, j, k; i + lans <= L; ++i)
            {
                for (j = 0; j < lans; ++j)
                    tans[j] = s[0][i + j];
                tans[j] = '\0';

                for (k = 1; k < n; ++k)
                    if (!kmp (s[k], tans)) break;

                if (k == n)
                {
                    if(strcmp(ans,tans)>0) strcpy(ans,tans);
                    flag = 1;
                }
            }
        }

        if (flag == 1) printf ("%s\n", ans);
        else printf ("no significant commonalities\n");
    }
    return 0;
}
```