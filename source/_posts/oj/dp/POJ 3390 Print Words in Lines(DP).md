---
title: POJ 3390 Print Words in Lines(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-09-02 09:50:19
---
Print Words in Lines

**Time Limit:** 1000MS  **Memory Limit:** 65536K **Total Submissions:** 1624  **Accepted:** 864

Description
We have a paragraph of text to print. A text is a sequence of words and each word consists of characters. When we print a text, we print the words from the text one at a time, according to the order the words appear in the text. The words are printed in lines, and we can print at most *M* characters in a line. If there is space available, we could print more than one word in a line. However, when we print more than one word in a line, we need to place exactly one space character between two adjacent words in a line. For example, when we are given a text like the following:
This is a text of fourteen words and the longest word has ten characters

Now we can print this text into lines of no more than 20 characters as the following.

This is
a text of
fourteen words
and the longest
word
has ten characters

However, when you print less than 20 characters in a line, you need to pay a penalty, which is equal to the square of the difference between 20 and the actual number of characters you printed in that line. For example in the first line we actually printed seven characters so the penalty is (20 − 7)2 = 169. The total penalty is the sum of all penalties from all lines. Given the text and the number of maximum number of characters allowed in a line, compute the minimum penalty to print the text.

Input

The first line of the input is the number of test cases (*C*). The first line of a test case is the maximum number of characters allowed in a line (*M*). The second line of a test case is the number of words in the text (*N*). The following *N* lines are the length (in character) of each word in the text. It is guaranteed that no word will have more than *M* characters, *N* is at most 10000, and *M* is at most 100.

Output

The output has *C* lines. Each line has the minimum penalty one needs to pay in order to print the text in that test case.

Sample Input

2 20 14 4 2 1 4 2 8 5 3 3 7 4 3 3 10 30 14 4 2 1 4 2 8 5 3 3 7 4 3 3 10

Sample Output

33 146

Source

[Kaohsiung 2006](http://poj.org/searchproblem?field=source&key=Kaohsiung+2006)

题意 把n个单词排版 每行最多m个字符 不同单词间有空格 每行最后一个单词后没空格 空格占一个字符 当一行的字符数与m的差为t时 就会扣t/*t分 求最少扣分

令a[i]表示第i个单词的长度 s[i]表示从第一个单词到第i个单词单词长度和d[i]表示前i个单词排版后最少扣的分

则t=m-(s[i] - s[j] + i - j - 1)表示把从第j+1个单词到第i个单词放在一行时这行的字符长度与m的差

那么当t>=0时 有转移方程 d[i]=min(d[i],d[j]+t/*t) ;

有了方程程序就好写了:

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 10005;
int s[N], d[N], a[N], t, cas, m, n;
int main()
{
    scanf ("%d", &cas);
    while (cas--)
    {
        scanf ("%d%d", &m, &n);
        for (int i = 1; i <= n; ++i)
        {
            scanf ("%d", &a[i]);
            s[i] = s[i - 1] + a[i];
        }
        memset (d, 0x3f, sizeof (d)); d[0] = 0;
        for (int i = 1; i <= n; ++i)
            for (int j = i - 1; j >= 0; --j)
            {
                t = m - (s[i] - s[j] + i - j - 1);
                if (t >= 0)  d[i] = min (d[i], d[j] + t * t);
                else break;
            }
        printf ("%d\n", d[n]);
    }
}
```