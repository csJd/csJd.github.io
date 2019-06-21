---
title: POJ 3087 Shuffle'm Up（模拟）
author: Deng
tags: 
       - Algorithm

category: 
       - Other
       - OJ

date: 2014-09-02 09:51:43
---
题意 给两堆牌s1,s2交给你洗 每堆有c张 每次洗牌得到s12 其中s2的最下面一张在s12的最下面一张然后按顺序一张s1一张s2 洗好之后可以把s12下面的c张做s1 上面的c张做s2 求多少次洗牌之后可以得到输入给你的串s 不能得到输出-1

简单模拟 s1+s2!=s就一直洗牌 如果回到初始状态都没得到s就不会得到s了 得到s就可以输出洗牌次数了

```js 
#include<iostream>
#include<string>
using namespace std;
int cas, c, ans;
string s, bs, s12, s1, s2;

void shuf (string s1, string s2)
{
    if (bs == s) return;
    ++ans;
    int i = -1, j = -1, k;
    string ss1, ss2;
    for (k = 0; k < c; ++k)
        ss1 += (i == j ? s2[++i] : s1[++j]);
    for (k = 0; k < c; ++k)
        ss2 += (i == j ? s2[++i] : s1[++j]);
    s12 = ss1 + ss2;
    if (s12 == s || s12 == bs) return;
    else shuf (ss1, ss2);
}

int main()
{
    cin >> cas;
    for (int ca = 1; ca <= cas; ++ca)
    {
        ans = 0;
        cin >> c >> s1 >> s2 >> s;
        bs = s1 + s2;
        shuf (s1, s2);
        if (s12 != s) ans = -1;
        cout << ca << " " << ans << endl;
    }
    return 0;
}
```

也可以用bfs来做 每次出队入队的也只有一个 和模拟差不多

```js 
#include <cstdio>
#include <string>
#include <cstring>
using namespace std;
const int N = 105;
string q[N], ss, ts;
int n;

int bfs()
{
    string cs, s;
    int le = 0, ri = 0;
    q[ri++] = ss;
    while(le < ri)
    {
        cs = q[le++];
        if(cs == ts) return le - 1;
        s = "";
        for(int i = 0; i < n; ++i)
            s += cs[n + i], s += cs[i];
        if(s == ss) return -1;
        q[ri++] = s;
    }
    return -1;
}

int main()
{
    int cas;
    char a[N], b[N], s[2 * N];
    scanf("%d", &cas);
    for(int k = 1; k <= cas; ++k)
    {
        scanf("%d%s%s%s", &n, a, b, s);
        ts = string(s), ss = string(a) + b;
        printf("%d %d\n", k, bfs());
    }
    return 0;
}
```

附上此题数据

```js 
/*
11
4
AHAH
HAHA
HHAAAAHH
3
CDE
CDE
EEDDCC
9
ACCABCABC
DEFDEFDEF
ECECECAFAFAFDBDBDB
10
ABCDEFGHCD
AAAAAAAAAA
BDFHDAAAAAACEGCAAAAA
100
ABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCD
ABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCDABCDEFGHCD
CEGCACEGCACEGCACEGCACEGCADFHDBDFHDBDFHDBDFHDBDFHDBEGCACEGCACEGCACEGCACEGCACFHDBDFHDBDFHDBDFHDBDFHDBDGCACEGCACEGCACEGCACEGCACEHDBDFHDBDFHDBDFHDBDFHDBDFCACEGCACEGCACEGCACEGCACEGDBDFHDBDFHDBDFHDBDFHDBDFH
5
AAAAA
BBBBB
AABBBAAABB
6
AAAAAA
BBBBBB
AAABBBAAABBB
7
AAAAAAA
BBBBBBB
ABBAABBAABBAAB
8
AAAAAAAA
BBBBBBBB
BBAABBAABBAABBAA
9
AAAAAAAAA
BBBBBBBBB
BBBBAAAAABBBBBAAAA
10
AAAAAAAAAA
BBBBBBBBBB
BBAABBAABBAABBAABBAA

1 2
2 -1
3 -1
4 5
5 30
6 9
7 11
8 2
9 2
10 8
11 2
*/
```

Shuffle'm Up

Description
A common pastime for poker players at a poker table is to shuffle stacks of chips. Shuffling chips is performed by starting with two stacks of poker chips, **S1** and **S2**, each stack containing ***C***chips. Each stack may contain chips of several different colors.

The actual shuffle operation is performed by interleaving a chip from **S1** with a chip from **S2** as shown below for ***C*** = 5:
![](../images/es-3087_1-.png)

The single resultant stack, **S12**, contains 2 /* ***C*** chips. The bottommost chip of **S12** is the bottommost chip from **S2**. On top of that chip, is the bottommost chip from **S1**. The interleaving process continues taking the 2nd chip from the bottom of **S2** and placing that on **S12**, followed by the 2nd chip from the bottom of **S1** and so on until the topmost chip from **S1** is placed on top of **S12**.

After the shuffle operation, **S12** is split into 2 new stacks by taking the bottommost ***C*** chips from **S12** to form a new **S1** and the topmost ***C*** chips from **S12** to form a new **S2**. The shuffle operation may then be repeated to form a new **S12**.

For this problem, you will write a program to determine if a particular resultant stack **S12** can be formed by shuffling two stacks some number of times.

Input

The first line of input contains a single integer ***N***, (1 ≤ ***N*** ≤ 1000) which is the number of datasets that follow.

Each dataset consists of four lines of input. The first line of a dataset specifies an integer ***C***, (1 ≤ ***C*** ≤ 100) which is the number of chips in each initial stack (**S1** and **S2**). The second line of each dataset specifies the colors of each of the ***C*** chips in stack **S1**, starting with the bottommost chip. The third line of each dataset specifies the colors of each of the ***C*** chips in stack **S2**starting with the bottommost chip. Colors are expressed as a single uppercase letter (**A** through **H**). There are no blanks or separators between the chip colors. The fourth line of each dataset contains 2 /* ***C*** uppercase letters (**A** through **H**), representing the colors of the desired result of the shuffling of **S1** and **S2** zero or more times. The bottommost chip’s color is specified first.

Output

Output for each dataset consists of a single line that displays the dataset number (1 though ***N***), a space, and an integer value which is the minimum number of shuffle operations required to get the desired resultant stack. If the desired result can not be reached using the input for the dataset, display the value negative 1 (**−1**) for the number of shuffle operations.

Sample Input

2 4 AHAH HAHA HHAAAAHH 3 CDE CDE EEDDCC

Sample Output

1 2 2 -1