---
title: Codeforces 534D Handshakes(贪心)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Other

date: 2015-04-13 18:22:01
---
题意 房子里有n个人走进来 编号1~n 每个人走进来时房子里所有空闲的人都会和他招手 空闲的某三个人可以选择一起去打比赛 当然打比赛就变得不空闲了 给你每个人进来时和他招手的人的数量 要求输出一种可能的进房间顺序 没有可能的就输出Impossible

这题放在d就比较简单了 直接贪心就可以 把招手数量为i对应的人都保存到栈s[i]里 第一个进房间的人肯定是s[0]里的 然后让i从0开始 s[i]非空时 让s[i]的栈顶的人进入房间 然后i++ 否则让s[i-1],s[i-2],s[i-3]都出栈一个 再让i=i-3 也就是有3个人去打比赛了 一直这样循环下去直到所有人都进入房间或者i<0(对应不可能的情况) 最后输出进入房间的序列就行了

```js 
#include <bits/stdc++.h>
using namespace std;
const int N = 2e5 + 5;
int q[N], n;
stack<int> s[N];

int main()
{
    int a;
    scanf("%d", &n);
    for(int i = 1; i <= n; ++i)
        scanf("%d", &a), s[a].push(i);

    int p = 0, i = 0;
    while(1)
    {
        if(!s[i].empty())  q[p++] = s[i++].top();
        else
        {
            //出栈三个人去打比赛
            if(i < 3) break;
            s[--i].pop(), s[--i].pop(), s[--i].pop();
        }
    }

    if(p == n)
    {
        puts("Possible");
        for(int i = 0; i < n - 1; ++i)
            printf("%d ", q[i]);
        printf("%d\n", q[n - 1]);
    }
    else puts("Impossible");
    return 0;
}
//Last modified :   2015-04-13 01:57
```

D. Handshakes
On February, 30th *n* students came in the Center for Training Olympiad Programmers (CTOP) of the Berland State University. They came one by one, one after another. Each of them went in, and before sitting down at his desk, greeted with those who were present in the room by shaking hands. Each of the students who came in stayed in CTOP until the end of the day and never left.

At any time any three students could join together and start participating in a team contest, which lasted until the end of the day. The team did not distract from the contest for a minute, so when another student came in and greeted those who were present, he did not shake hands with the members of the contest writing team. Each team consisted of exactly three students, and each student could not become a member of more than one team. Different teams could start writing contest at different times.

Given how many present people shook the hands of each student, get a possible order in which the students could have come to CTOP. If such an order does not exist, then print that this is impossible.

Please note that some students could work independently until the end of the day, without participating in a team contest.

Input

The first line contains integer *n* (1 ≤ *n* ≤ 2·105) — the number of students who came to CTOP. The next line contains *n* integers*a*1, *a*2, ..., *a**n* (0 ≤ *a**i* < *n*), where *a**i* is the number of students with who the *i*-th student shook hands.
Output

If the sought order of students exists, print in the first line "Possible" and in the second line print the permutation of the students' numbers defining the order in which the students entered the center. Number *i* that stands to the left of number *j* in this permutation means that the *i*-th student came earlier than the *j*-th student. If there are multiple answers, print any of them.

If the sought order of students doesn't exist, in a single line print "Impossible".

Sample test(s)

input 5 2 1 3 0 1

output Possible 4 5 1 3 2
input 9 0 2 3 4 1 1 0 2 2

output Possible 7 5 2 1 6 8 3 4 9
input 4 0 2 1 1

output Impossible