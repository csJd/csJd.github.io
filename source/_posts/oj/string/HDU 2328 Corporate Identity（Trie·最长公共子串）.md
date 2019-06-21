---
title: HDU 2328 Corporate Identity（Trie·最长公共子串）
author: Deng
tags: 
       - Algorithm

category: 
       - String
       - OJ

date: 2015-10-05 16:03:25
---
题意 输出n个串的字典序最小的最长公共子串

可以枚举第一个串的所有子串 然后对每个串kmp匹配 比较复杂 而且慢 发现和HDU2846（求模式串在n个串中出现的次数）有类似之处 都可以用Trie来处理 一个串的子串肯定是其某个后缀的前缀 我们把第一个串的所有后缀都插入到Trie中 最长公共子串肯定是在这个Trie里面的 因为它肯定是第一个串的子串 在插入后面的串的后缀时 可以发现

1. 对于在第一个串中没有的节点是可以直接break的 在第一个串中都没有怎么会是公共的呢

2. 当前前缀在前面i个串中出现的次数小于i 那么也可以break了 因为肯定有某些串已经不包含这个前缀了

所以我们后面的插入只用基于第一次插入的Trie就行了 插入最后一个串的后缀时取插入长度最长的串就是答案了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 30005;
int trie[N][26], cnt[N], L;
char s[N];

void initTrie()
{
    L = 1;
    memset(cnt, 0, sizeof(cnt));
    memset(trie, 0, sizeof(trie));
}

int insertTrie(char s[], int id)
{
    int r = 0, i = 0, len = 0, j;
    while(s[i])
    {
        j = s[i++] - 'a';
        if(!trie[r][j])
        {
            if(id == 1) trie[r][j] = L++;
            else break;  //在第一个串中都没有的就不用管了
        }

        r = trie[r][j];
        if(cnt[r] + 1 >= id) cnt[r] = id;
        else break;  //当前前缀出现过的次数小于id说明该前缀不是前面某些串的子串 不用再往前看
        ++len;   //当前前缀的长度
    }
    return len;
}

int main()
{
    int n;
    while(scanf("%d", &n), n)
    {
        initTrie(); //初始化Trie
        int len = 0;
        char ans[N] = "IDENTITY LOST", t[N];

        for(int i = 1; i <= n; ++i)
        {
            scanf("%s", s);
            for(int j = 0; s[j]; ++j)
            {
                int k = insertTrie(s + j, i);
                if(!k || i < n) continue; //插入最后一一个串时才需要处理

                strncpy(t, s + j, k), t[k] = 0;
                if(k > len || (k == len && strcmp(ans, t) > 0))
                    strcpy(ans, t), len = k;
            }
        }

        puts(ans);
    }
    return 0;
}
//Last modified :  2015-10-05 15:50 CST
```

# Corporate Identity

Problem Description

Beside other services, ACM helps companies to clearly state their “corporate identity”, which includes company logo but also other signs, like trademarks. One of such companies is Internet Building Masters (IBM), which has recently asked ACM for a help with their new identity. IBM do not want to change their existing logos and trademarks completely, because their customers are used to the old ones. Therefore, ACM will only change existing trademarks instead of creating new ones.
After several other proposals, it was decided to take all existing trademarks and find the longest common sequence of letters that is contained in all of them. This sequence will be graphically emphasized to form a new logo. Then, the old trademarks may still be used while showing the new identity.
Your task is to find such a sequence.
Input

The input contains several tasks. Each task begins with a line containing a positive integer N, the number of trademarks (2 ≤ N ≤ 4000). The number is followed by N lines, each containing one trademark. Trademarks will be composed only from lowercase letters, the length of each trademark will be at least 1 and at most 200 characters.
After the last trademark, the next task begins. The last task is followed by a line containing zero.
Output

For each task, output a single line containing the longest string contained as a substring in all trademarks. If there are several strings of the same length, print the one that is lexicographically smallest. If there is no such non-empty string, output the words “IDENTITY LOST” instead.
Sample Input

3 aabbaabb abbababb bbbbbabb 2 xyz abc 0
Sample Output

abb IDENTITY LOST