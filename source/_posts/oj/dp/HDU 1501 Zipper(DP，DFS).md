---
title: HDU 1501 Zipper(DP，DFS)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-09-02 09:52:00
---
题意 判断能否由字符串a,b中的字符不改变各自的相对顺序组合得到字符串c

本题有两种解法 DP或者DFS

考虑DP 令d[i][j]表示能否有a的前i个字符和b的前j个字符组合得到c的前i+j个字符 值为0或者1 那么有d[i][j]=(d[i-1][j]&&a[i]==c[i+j])||(d[i][j-1]&&b[i]==c[i+j]) a,b的下标都是从1开始的 注意0的初始化

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 205;
char a[N], b[N], c[2 * N];
bool  d[N][N];

int main()
{
    int cas;
    scanf ("%d", &cas);
    for (int k = 1; k <= cas; ++k)
    {
        scanf ("%s%s%s", a + 1, b + 1, c + 1);
        int la = strlen (a + 1), lb = strlen (b + 1), i = 1, j = 1;
        memset (d, 0, sizeof (d));

        while (a[i] == c[i] && i <= la)
            d[i++][0] = true;
        while (b[j] == c[j] && j <= lb)
            d[0][j++] = true;
        for (int i = 1; i <= la; ++i)
            for (int j = 1; j <= lb; ++j)
                d[i][j] = ( (d[i - 1][j] && a[i] == c[i + j]) || (d[i][j - 1] && b[j] == c[i + j]));

        printf ("Data set %d: ", k);
        printf (d[la][lb] ? "yes\n" : "no\n");
    }
    return 0;
}
```

下面是dfs的代码 看能否在ab中对应搜到c的每一个字母就可

```js 
//DFS版
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 205;
char a[N], b[N], c[2 * N];
bool vis[N][N], ans;
void dfs (int i, int j, int k)
{
    if (c[k] == '\0') ans = true;
    if (ans || vis[i][j])  return ;
    vis[i][j] = true;
    if (a[i] == c[k]) dfs (i + 1, j, k + 1);
    if (b[j] == c[k]) dfs (i, j + 1, k + 1);
}
int main()
{
    int cas;
    scanf ("%d", &cas);
    for (int ca = 1; ca <= cas; ++ca)
    {
        ans = false;
        memset (vis, 0, sizeof (vis));
        scanf ("%s%s%s", a, b, c);
        dfs (0, 0, 0);
        printf ("Data set %d: ", ca);
        printf (ans ? "yes\n" : "no\n");
    }
    return 0;
}
```

# Zipper

Problem Description

Given three strings, you are to determine whether the third string can be formed by combining the characters in the first two strings. The first two strings can be mixed arbitrarily, but each must stay in its original order.
For example, consider forming "tcraete" from "cat" and "tree":
String A: cat
String B: tree
String C: tcraete
As you can see, we can form the third string by alternating characters from the two strings. As a second example, consider forming "catrtee" from "cat" and "tree":
String A: cat
String B: tree
String C: catrtee
Finally, notice that it is impossible to form "cttaree" from "cat" and "tree".
Input

The first line of input contains a single positive integer from 1 through 1000. It represents the number of data sets to follow. The processing for each data set is identical. The data sets appear on the following lines, one data set per line.
For each data set, the line of input consists of three strings, separated by a single space. All strings are composed of upper and lower case letters only. The length of the third string is always the sum of the lengths of the first two strings. The first two strings will have lengths between 1 and 200 characters, inclusive.
Output

For each data set, print:
Data set n: yes
if the third string can be formed from the first two, or
Data set n: no
if it cannot. Of course n should be replaced by the data set number. See the sample output below for an example.
Sample Input

3 cat tree tcraete cat tree catrtee cat tree cttaree
Sample Output

Data set 1: yes Data set 2: yes Data set 3: no