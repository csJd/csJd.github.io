---
title: HDU 3008 Warcraft (DP)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:52:04
---
题意 你去打boss 开始你的蓝和血还有boss的血都是100 每秒你先打boss一下 然后boss打你一下你减少q点血 你有n个技能 第i个技能耗蓝a[i] 对boss的伤害为b[i] 普攻伤害为1 而且你每秒回复t点蓝（恢复后不超过100） 求你最少可以多少次打死boss

你最多能打100/q或者100/q+1次 令d[i][j]表示第i秒所剩蓝量为j时boss剩下的最少血量 m为j还未恢复蓝之前的蓝量 j = min (100, m + t) 那么有 d[i][j] = min (d[i][j], d[i - 1][m + a[k]] - b[k]);

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 105;
int a[N], b[N], d[N][N];
int main()
{
    int n, t, q, ans, ti;
    while (scanf ("%d%d%d", &n, &t, &q), n)
    {
        ++n;
        ti = (100 % q) ? (100 / q + 1) : (100 / q);
        a[1] = 0, b[1] = 1;
        for (int i = 2; i <= n; ++i)
            scanf ("%d%d", &a[i], &b[i]);
            
        memset (d, 0x3f, sizeof (d));
        d[0][100] = 100; ans = 0;

        for (int i = 1; i <= ti; ++i)
            for (int m = 0; m <= 100; ++m)
            {
                int j = min (100, m + t);
                for (int k = 1; k <= n; ++k)
                    if (m + a[k] <= 100)
                        d[i][j] = min (d[i][j], d[i - 1][m + a[k]] - b[k]);
                if (d[i][j] <= 0)
                {
                    ans = i, i = ti;
                    break;
                }
            }

        if (ans == 0) printf ("My god\n");
        else printf ("%d\n", ans);
    }
    return 0;
}
```

# Warcraft

Problem Description

Have you ever played the Warcraft?It doesn't matter whether you have played it !We will give you such an experience.There are so many Heroes in it,but you could only choose one of them.Each Hero has his own skills.When such a Skill is used ,it costs some MagicValue,but hurts the Boss at the same time.Using the skills needs intellegence,one should hurt the enemy to the most when using certain MagicValue.
Now we send you to complete such a duty to kill the Boss(So cool~~).To simplify the problem:you can assume the LifeValue of the monster is 100, your LifeValue is 100,but you have also a 100 MagicValue!You can choose to use the ordinary Attack(which doesn't cost MagicValue),or a certain skill(in condition that you own this skill and the MagicValue you have at that time is no less than the skill costs),there is no free lunch so that you should pay certain MagicValue after you use one skill!But we are good enough to offer you a "ResumingCirclet"(with which you can resume the MagicValue each seconds),But you can't own more than 100 MagicValue and resuming MagicValue is always after you attack.The Boss is cruel , be careful!
Input

There are several test cases,intergers n ,t and q (0<n<=100，1<=t<=5，q>0) in the first line which mean you own n kinds of skills ,and the "ResumingCirclet" helps you resume t points of MagicValue per second and q is of course the hurt points of LifeValue the Boss attack you each time(we assume when fighting in a second the attack you show is before the Boss).Then n lines follow,each has 2 intergers ai and bi(0<ai,bi<=100).which means using i skill costs you ai MagicValue and costs the Boss bi LifeValue.The last case is n=t=q=0.
Output

Output an interger min (the minimun time you need to kill the Boss)in one line .But if you die(the LifeValue is no more than 0) ,output "My god"!
Sample Input

4 2 25 10 5 20 10 30 28 76 70 4 2 25 10 5 20 10 30 28 77 70 0 0 0
Sample Output

4 My god

*Hint* Hint: When fighting,you can only choose one kind of skill or just to use the ordinary attack in the whole second,the ordinary attack costs the Boss 1 points of LifeValue,the Boss can only use ordinary attack which costs a whole second at a time.Good Luck To You!