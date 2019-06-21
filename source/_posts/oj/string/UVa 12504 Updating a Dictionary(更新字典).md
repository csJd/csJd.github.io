---
title: UVa 12504 Updating a Dictionary(更新字典)
author: Deng
tags: 
       - Algorithm

category: 
       - String
       - OJ

date: 2015-01-22 16:16:49
---
题意 比较两个字典 按字典序输出所有添加 删除 修改的项 如果没有任何更新 输出 No changes

STL map的应用 对比两个字典 注意开始字符串的处理和字典可以为空

```js 
#include<bits/stdc++.h>
using namespace std;
map<string, string> d[2];
map<string, string>::iterator it;
const int N = 105;
string s, a, b, t[N];

void print(char c, int n)
{
    sort(t, t + n), cout << c << t[0];
    for(int i = 1; i < n; ++i) cout << ',' << t[i];
    puts("");
}

int main()
{
    int cas, n, c1, c2, c3;
    cin >> cas;
    while(cas--)
    {
        d[0].clear(), d[1].clear();
        for(int i = 0; i < 2; ++i)
        {
            cin >> s;
            int j = 1, l = s.size();
            while(l > 2 && j < l)
            {
                while(s[j] != ':') a += s[j++]; ++j;
                while(s[j] != ',' && s[j] != '}') b += s[j++]; ++j;
                d[i][a] = b, a = b = "";
                //cout << a << " : " << b << endl;
            }
        }

        c1 = c2 = c3 = 0;
        for(it = d[1].begin(); it != d[1].end(); ++it)
            if(!d[0].count(it->first)) t[c1++] = it->first;
        if(c1) print('+', c1);

        for(it = d[0].begin(); it != d[0].end(); ++it)
            if(!d[1].count(it->first)) t[c2++] = it->first;
        if(c2) print('-', c2);

        for(it = d[1].begin(); it != d[1].end(); ++it)
            if(d[0].count(it->first) && d[0][it->first] != it->second) t[c3++] = it->first;
        if(c3) print('*', c3);

        if(!(c1 || c2 || c3)) puts("No changes");
        puts("");
    }
    return 0;
}
```

#

****

In this problem, a dictionary is collection of key-value pairs, where keys are lower-case letters, and values are non-negative integers. Given an old dictionary and a new dictionary, find out what were changed.

Each dictionary is formatting as follows:

{*key*:*value*,*key*:*value*,...,*key*:*value*}

Each key is a string of lower-case letters, and each value is a non-negative integer without leading zeros or prefix `+'. (i.e. -4, 03 and +77 are illegal). Each key will appear at most once, but keys can appear in any order.

##

The first line contains the number of test cases *T* (  *T*![$ \le$](../images/dge.org-external-125-12504img1-.png)1000 ). Each test case contains two lines. The first line contains the old dictionary, and the second line contains the new dictionary. Each line will contain at most 100 characters and will not contain any whitespace characters. Both dictionaries could be empty.

WARNING: there are no restrictions on the lengths of each key and value in the dictionary. That means keys could be really long and values could be really large.

##

For each test case, print the changes, formatted as follows:

If the two dictionaries are identical, print `No changes' (without quotes) instead.

Print a blank line after each test case.

##

3 {a:3,b:4,c:10,f:6} {a:3,c:5,d:10,ee:4} {x:1,xyz:123456789123456789123456789} {xyz:123456789123456789123456789,x:1} {first:1,second:2,third:3} {third:3,second:2}

##

+d,ee -b,f /*c No changes -first