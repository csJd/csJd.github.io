---
title: Codeforces 527B Error Correct System(字符串)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2015-03-18 19:49:15
---
题意 两个长度为n的只由小写字母组成的字符串a.b 问能否同时交换两个串两个对应位置的字符 使得两个串相同位置字符不相同的数目最小

因为只能交换一次 所以只可能减少0,1或2个 用p[i][j]记录a串字符i对应位置b串字符为j的位置 不存在的为0 那么有

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 200005, M = 26;
char a[N], b[N];
int p[M][M];
int main()
{
    int n, d, u, v, flag;
    while(~scanf("%d", &n))
    {
        memset(p, 0, sizeof(p));
        scanf("%s%s", a, b);
        for(int i = d = flag = 0; i < n; ++i)
            if(a[i] != b[i]) ++d, p[a[i] - 'a'][b[i] - 'a'] = i + 1;

        for(int i = 0; i < M; ++i)
            for(int j = 0; j < M; ++j)
                if((u = p[i][j]) && (v = p[j][i])) flag = 2, i = j = M;

        if(!flag) for(int i = 0; i < M; ++i) for(int j = 0; j < M; ++j) for(int k = 0; k < M; ++k)
                      if((u = p[i][j]) && (v = p[j][k])) flag = 1, i = j = k = M;
        if(!flag) u = v = -1;

        printf("%d\n%d %d\n", d - flag, u, v);
    }
    return 0;
}
```
B. Error Correct System

Ford Prefect got a job as a web developer for a small company that makes towels. His current work task is to create a search engine for the website of the company. During the development process, he needs to write a subroutine for comparing strings *S* and *T* of equal length to be "similar". After a brief search on the Internet, he learned about the Hamming distance between two strings *S* and *T* of the same length, which is defined as the number of positions in which *S* and *T* have different characters. For example, the Hamming distance between words "permanent" and "pergament" is two, as these words differ in the fourth and sixth letters.

Moreover, as he was searching for information, he also noticed that modern search engines have powerful mechanisms to correct errors in the request to improve the quality of search. Ford doesn't know much about human beings, so he assumed that the most common mistake in a request is swapping two arbitrary letters of the string (not necessarily adjacent). Now he wants to write a function that determines which two letters should be swapped in string *S*, so that the Hamming distance between a new string *S* and string *T* would be as small as possible, or otherwise, determine that such a replacement cannot reduce the distance between the strings.

Help him do this!
Input

The first line contains integer *n* (1 ≤ *n* ≤ 200 000) — the length of strings *S* and *T*.

The second line contains string *S*.

The third line contains string *T*.

Each of the lines only contains lowercase Latin letters.

Output

In the first line, print number *x* — the minimum possible Hamming distance between strings *S* and *T* if you swap at most one pair of letters in *S*.

In the second line, either print the indexes *i* and *j* (1 ≤ *i*, *j* ≤ *n*, *i* ≠ *j*), if reaching the minimum possible distance is possible by swapping letters on positions *i* and *j*, or print "-1 -1", if it is not necessary to swap characters.

If there are multiple possible answers, print any of them.
Sample test(s)

input 9 pergament permanent

output 1 4 6
input 6 wookie cookie

output 1 -1 -1
input 4 petr egor

output 2 1 2
input 6 double bundle

output 2 4 1