---
title: POJ 1128 Frame Stacking（拓扑排序·打印字典序）
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2015-08-20 10:29:13
---
题意 给你一些矩形框堆叠后的俯视图 判断这些矩形框的堆叠顺序 每个矩形框满足每边都至少有一个点可见 输入保证至少有一个解 按字典序输出所有可行解

和上一题有点像 只是这个要打印所有的可行方案 建图还是类似 因为每个矩形框的四边都有点可见 所以每个矩形框的左上角和右下角的坐标是可以确定的 然后一个矩形框上有其它字符时 就让这个矩形框对应的字符和那个其它字符建立一个小于关系 由于要打印方案 所以在有多个入度为0的点时需要用DFS对每种选择都进行一遍拓扑排序

```js 
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 50;
char ans[N], g[N][N], tp[N][N];
int x1[N], y1[N], x2[N], y2[N];
//(x1,y1)为对应字母的左上角坐标  (x2,y2)为右下
int in[N], n;

void addTopo(int i, int j, int c)
{
    int t = g[i][j] - 'A';
    if(t != c && !tp[c][t])
    {
        ++in[t];
        tp[c][t] = 1;
    }
}

void build()
{
    memset(tp, 0, sizeof(tp)); //tp[i][j] = 1表示有i < j的关系
    for(int c = n = 0; c < 26; ++c)
    {
        if(in[c] < 0) continue;
        for(int i = x1[c]; i <= x2[c]; ++i)
        {
            addTopo(i, y1[c], c);
            addTopo(i, y2[c], c);
        }
        for(int j = y1[c]; j <= y2[c]; ++j)
        {
            addTopo(x1[c], j, c);
            addTopo(x2[c], j, c);
        }
        ++n;//统计出现了多少个字符
    }
}

void topoSort(int k)
{
    if(k == n)
    {
        ans[k] = 0;
        puts(ans);
        return;
    }

    //从前往后找入度为0的点保证升序
    for(int i = 0; i < 26; ++i)
    {
        if(in[i] == 0)
        {
            ans[k] = i + 'A'; //这一位放i
            in[i] = -1;
            for(int j = 0; j < 26; ++j)
                if(tp[i][j]) --in[j];

            topoSort(k + 1); //找下一位

            in[i] = 0; //回溯
            for(int j = 0; j < 26; ++j)
                if(tp[i][j]) ++in[j];
        }
    }
}

int main()
{
    int h, w, c;
    while(~scanf("%d%d", &h, &w))
    {
        for(int i = 0; i < 26; ++i)
        {
            x1[i] = y1[i] = N;
            x2[i] = y2[i] = 0;
        }

        memset(in, -1, sizeof(in));
        for(int i = 0; i < h; ++i)
        {
            scanf("%s", g[i]);
            for(int j = 0; j < w; ++j)
            {
                if((c = g[i][j] - 'A') < 0) continue; //g[i][j] ='.'
                if(i < x1[c]) x1[c] = i;
                if(i > x2[c]) x2[c] = i;
                if(j < y1[c]) y1[c] = j;
                if(j > y2[c]) y2[c] = j;
                in[c] = 0;  //出现过的字母in初始为0  否则为-1
            }
        }
        build();
        topoSort(0);
    }
    return 0;
}
```

Frame Stacking

Description
Consider the following 5 picture frames placed on an 9 x 8 array.
........ ........ ........ ........ .CCC.... EEEEEE.. ........ ........ ..BBBB.. .C.C.... E....E.. DDDDDD.. ........ ..B..B.. .C.C.... E....E.. D....D.. ........ ..B..B.. .CCC.... E....E.. D....D.. ....AAAA ..B..B.. ........ E....E.. D....D.. ....A..A ..BBBB.. ........ E....E.. DDDDDD.. ....A..A ........ ........ E....E.. ........ ....AAAA ........ ........ EEEEEE.. ........ ........ ........ ........ 1 2 3 4 5
Now place them on top of one another starting with 1 at the bottom and ending up with 5 on top. If any part of a frame covers another it hides that part of the frame below.
Viewing the stack of 5 frames we see the following.
.CCC.... ECBCBB.. DCBCDB.. DCCC.B.. D.B.ABAA D.BBBB.A DDDDAD.A E...AAAA EEEEEE..
In what order are the frames stacked from bottom to top? The answer is EDABC.
Your problem is to determine the order in which the frames are stacked from bottom to top given a picture of the stacked frames. Here are the rules:
1. The width of the frame is always exactly 1 character and the sides are never shorter than 3 characters.
2. It is possible to see at least one part of each of the four sides of a frame. A corner shows two sides.
3. The frames will be lettered with capital letters, and no two frames will be assigned the same letter.

Input

Each input block contains the height, h (h<=30) on the first line and the width w (w<=30) on the second. A picture of the stacked frames is then given as h strings with w characters each.
Your input may contain multiple blocks of the format described above, without any blank lines in between. All blocks in the input must be processed sequentially.

Output

Write the solution to the standard output. Give the letters of the frames in the order they were stacked from bottom to top. If there are multiple possibilities for an ordering, list all such possibilities in alphabetical order, each one on a separate line. There will always be at least one legal ordering for each input block. List the output for all blocks in the input sequentially, without any blank lines (not even between blocks).

Sample Input

9 8 .CCC.... ECBCBB.. DCBCDB.. DCCC.B.. D.B.ABAA D.BBBB.A DDDDAD.A E...AAAA EEEEEE..

Sample Output

EDABC