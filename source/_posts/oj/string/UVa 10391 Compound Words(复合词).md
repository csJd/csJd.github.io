---
title: UVa 10391 Compound Words(复合词)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2015-01-21 17:49:37
---
题意 输出所有输入单词中可以由另两个单词的组成的词

STL set的应用 枚举每个单词的所有可能拆分情况 看拆开的两个单词是否都存在 都存在的就可以输出了

```js 
#include <bits/stdc++.h>
using namespace std;
string a, b;
set<string> s;
set<string>::iterator i;

int main()
{
    int l;
    while(cin >> a) s.insert(a);
    for(i = s.begin(); i != s.end(); ++i)
    {
        l = i->length();
        for(int j = 1; j < l - 1; ++j)
        {
            a = i->substr(0, j);
            b = i->substr(j);
            if(s.count(a) && s.count(b))
            {
                cout << (*i) << endl;
                break;
            }
        }
    }
    return 0;
}
```

# Compound Words

You are to find all the two-word compound words in a dictionary. A two-word compound word is a word in the dictionary that is the concatenation of exactly two other words in the dictionary.

## Input

Standard input consists of a number of lowercase words, one per line, in alphabetical order. There will be no more than 120,000 words.

## Output

Your output should contain all the compound words, one per line, in alphabetical order.

## Sample Input

a alien born less lien never nevertheless new newborn the zebra

## Sample Output

alien newborn