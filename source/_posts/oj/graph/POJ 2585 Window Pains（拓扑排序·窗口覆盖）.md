---
title: POJ 2585 Window Pains（拓扑排序·窗口覆盖）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2015-08-19 16:11:44
---
题意 有一个4/*4的显示器 有9个程序 每个程序占2/*2个格子 他们的位置如图所示 当你运行某个程序时 这个程序就会显示在顶层覆盖其它的程序 给你某个时刻显示器的截图 判断此时电脑是否死机了（出现了不合法的覆盖关系）

拓扑排序的应用 关键是建图 当一个程序A的区域上有其它程序B时 说明A是在B之前运行的 那么我们可以建立一个A<B的拓扑关系 最后判断是否有环就行了 个人认为下标换为0操作起来比较方便 所以都还为了0

```js 
#include <cstdio>
#include <cstring>
#include <vector>
using namespace std;
const int N = 10;
int g[N][N], in[N], q[N];
vector<int> e[N];

void build() //建图
{
    memset(in, 0, sizeof(in));
    for(int k = 0; k < 9; ++k)
    {
        e[k].clear();
        int i = k / 3, j = k % 3, t; //获取程序k的左上角坐标
        for(int x = 0; x < 2; ++x)
        for(int y = 0; y < 2; ++y)
        {
            t = g[i + x][j + y];
            if(t != k) //程序k在程序t之前运行 k < t
            {
                e[k].push_back(t);
                ++in[t];
            }
        }
    }
}

bool topo()
{
    build();
    int front = 0, rear = 0, cur;
    for(int i = 0; i < 9; ++i)
        if(!in[i]) q[rear++] = i;
    while(front < rear)
    {
        cur = q[front++];
        for(int i = 0; i < e[cur].size(); ++i)
        {
            int j = e[cur][i];
            if(!(--in[j])) q[rear++] = j;
        }
    }
    return front >= 9; //front < 9 时有环
}

int main()
{
    char s[100];
    while(scanf("%s", s), s[0] != 'E')
    {
        for(int i = 0; i < 4; ++i)
            for(int j = 0; j < 4; ++j)
                scanf("%d", &g[i][j]), --g[i][j];
        scanf("%s", s);
        printf("THESE WINDOWS ARE ");
        puts(topo() ? "CLEAN" : "BROKEN");
    }
    return 0;
}
```

Window Pains

Description
Boudreaux likes to multitask, especially when it comes to using his computer. Never satisfied with just running one application at a time, he usually runs nine applications, each in its own window. Due to limited screen real estate, he overlaps these windows and brings whatever window he currently needs to work with to the foreground. If his screen were a 4 x 4 grid of squares, each of Boudreaux's windows would be represented by the following 2 x 2 windows:
1 1 . . 1 1 . . . . . . . . . . . 2 2 . . 2 2 . . . . . . . . . . . 3 3 . . 3 3 . . . . . . . . . . . . 4 4 . . 4 4 . . . . . . . . . . . 5 5 . . 5 5 . . . . . . . . . . . 6 6 . . 6 6 . . . . . . . . . . . . 7 7 . . 7 7 . . . . . . . . . . . 8 8 . . 8 8 . . . . . . . . . . . 9 9 . . 9 9 When Boudreaux brings a window to the foreground, all of its squares come to the top, overlapping any squares it shares with other windows. For example, if window 1 *and then*window 2 were brought to the foreground, the resulting representation would be: 1 2 2 ? 1 2 2 ? ? ? ? ? ? ? ? ? If window 4 were then brought to the foreground: 1 2 2 ? 4 4 2 ? 4 4 ? ? ? ? ? ? . . . and so on . . .
Unfortunately, Boudreaux's computer is very unreliable and crashes often. He could easily tell if a crash occurred by looking at the windows and seeing a graphical representation that should not occur if windows were being brought to the foreground correctly. And this is where you come in . . .

Input

Input to this problem will consist of a (non-empty) series of up to 100 data sets. Each data set will be formatted according to the following description, and there will be no blank lines separating data sets.
A single data set has 3 components:

After the last data set, there will be a single line:
ENDOFINPUT
Note that each piece of visible window will appear only in screen areas where the window could appear when brought to the front. For instance, a 1 can only appear in the top left quadrant.

Output

For each data set, there will be exactly one line of output. If there exists a sequence of bringing windows to the foreground that would result in the graphical representation of the windows on Boudreaux's screen, the output will be a single line with the statement:
THESE WINDOWS ARE CLEAN
Otherwise, the output will be a single line with the statement:
THESE WINDOWS ARE BROKEN

Sample Input

START 1 2 3 3 4 5 6 6 7 8 9 9 7 8 9 9 END START 1 1 3 3 4 1 3 3 7 7 9 9 7 7 9 9 END ENDOFINPUT

Sample Output

THESE WINDOWS ARE CLEAN THESE WINDOWS ARE BROKEN