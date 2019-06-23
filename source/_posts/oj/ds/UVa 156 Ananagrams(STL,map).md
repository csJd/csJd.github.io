---
title: UVa 156 Ananagrams(STL,map)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2014-09-02 09:50:32
---
#

****

Most crossword puzzle fans are used to *anagrams*--groups of words with the same letters in different orders--for example OPTS, SPOT, STOP, POTS and POST. Some words however do not have this attribute, no matter how you rearrange their letters, you cannot form another word. Such words are called *ananagrams*, an example is QUIZ.

Obviously such definitions depend on the domain within which we are working; you might think that ATHENE is an ananagram, whereas any chemist would quickly produce ETHANE. One possible domain would be the entire English language, but this could lead to some problems. One could restrict the domain to, say, Music, in which case SCALE becomes a *relative ananagram* (LACES is not in the same domain) but NOTE is not since it can produce TONE.

Write a program that will read in the dictionary of a restricted domain and determine the relative ananagrams. Note that single letter words are, ipso facto, relative ananagrams since they cannot be ``rearranged'' at all. The dictionary will contain no more than 1000 words.

## Input

Input will consist of a series of lines. No line will be more than 80 characters long, but may contain any number of words. Words consist of up to 20 upper and/or lower case letters, and will not be broken across lines. Spaces may appear freely around words, and at least one space separates multiple words on the same line. Note that words that contain the same letters but of differing case are considered to be anagrams of each other, thus tIeD and EdiT are anagrams. The file will be terminated by a line consisting of a single /#.

## Output

Output will consist of a series of lines. Each line will consist of a single word that is a relative ananagram in the input dictionary. Words must be output in lexicographic (case-sensitive) order. There will always be at least one relative ananagram.

## Sample input

ladder came tape soon leader acme RIDE lone Dreis peat ScAlE orb eye Rides dealer NotE derail LaCeS drIed noel dire Disk mace Rob dries /#

## Sample output

Disk NotE derail drIed eye ladder soon

题意 给你一篇文章 以"/#"号结束 按字典序求输出这篇文章中真正只出现过一次的单词 就是不能通过字母重新排列得到文章中另一个单词的单词

把每个单词的字母全部化为小写 再把这个单词中的字母按字典序排列 得到一个字符串 用map记下出现次数就行 只出现过一次的就是要输出的

```js 
#include<iostream>
#include<algorithm>
#include<string>
#include<vector>
#include<cctype>
#include<map>
using namespace std;
typedef vector<string>::iterator it;
vector<string> ans;
map<string, int> cnt, tcnt;
map<string, string> ss;
int main()
{
    string s, t;
    while (cin >> s, s != "#")
    {
        t = s;
        ans.push_back (s);
        for (int j = 0; j < t.length(); ++j)
            t[j] = tolower (t[j]);
        sort (t.begin(), t.end());
        ss[s] = t;
        ++cnt[t];
    }
    sort (ans.begin(), ans.end());
    for (it i = ans.begin(); i < ans.end(); ++i)
        if (cnt[ss[*i]] == 1)  cout << *i << endl;
    return 0;
}
```