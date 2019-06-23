---
title: POJ 1300 Door Man（欧拉通路）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Graph

date: 2014-11-11 15:55:31
---
题目描述：

你是一座大庄园的管家。庄园有很多房间，编号为 0、1、2、3，...。你的主人是一个心不在 焉的人，经常沿着走廊随意地把房间的门打开。多年来，你掌握了一个诀窍：沿着一个通道，穿 过这些大房间，并把房门关上。你的问题是能否找到一条路径经过所有开着门的房间，并使得： 1) 通过门后立即把门关上； 2) 关上了的门不再打开； 3) 后回到你自己的房间（房间 0），并且所有的门都已经关闭了。 在本题中，给定房间列表，及连通房间的、开着的门，并给定一个起始房间，判断是否存在 这样的一条路径。不需要输出这样的路径，只需判断是否存在。假定任意两个房间之间都是连通 的（可能需要经过其他房间）。

输入描述：

输入文件包含多个（多可达 100 个）测试数据，每个测试数据之间没有空行隔开。 每个测试数据包括 3部分： 起始行－格式为“START M N”，其中 M 为管理员起始所处的房间号，N 为房间的总数（1 ≤N≤20）； 房间列表－一共 N行，每行列出了一个房间通向其他房间的房间号（只需列出比它的号码大 的房间号，可能有多个，按升序排列），比如房间 3有门通向房间 1、5、7，则房间 3的信息行内 容为“5 7”，第一行代表房间 0，后一行代表行间 N-1。有可能有些行为空行，当然后一行肯 定是空行，因为 N-1 是大的房间号；两个房间之间可能有多扇门连通。 终止行－内容为"END"。 输入文件后一行是"ENDOFINPUT"，表示输入结束。

输出描述：

每个测试数据对应一行输出，如果能找到一条路关闭所有的门，并且回到房间 0，则输出"YES X"，X是他关闭的门的总数，否则输出"NO"。

就是判断能否构成欧拉通路·咯

无向图存在欧拉通路的充要条件：

1. 是连通图

2. 奇度节点个数为0或2,其中为0时为欧拉回路,为2时是以这两个点节点为端点的欧拉通路
```js 
#include<cstdio>
#include<cstring>
#include<cctype>
const int N = 21;
using namespace std;

int main()
{
    int door, cnt, m, n, deg[N];
    char s[N], c;
    while(~scanf("%s", s), strcmp(s, "ENDOFINPUT"))
    {
        memset(deg, 0, sizeof(deg));
        door = cnt = 0;
        scanf("%d %d\n", &m, &n);
        for(int i = 0; i < n; ++i)
        {
            while(scanf("%c", &c))
            {
                if(isdigit(c)) ++door, ++deg[i], ++deg[c - '0'];
                else if(c == ' ') continue;
                else break;
            }
            if(deg[i] % 2) ++cnt;
        }

        scanf("%s", s);
        bool ok = (cnt == 0 && m == 0 ) || (m && cnt == 2 && deg[m] % 2 );
        if(ok) printf("YES %d\n", door);
        else printf("NO\n");
    }
    return 0;
}
```

Door Man

Description
You are a butler in a large mansion. This mansion has so many rooms that they are merely referred to by number (room 0, 1, 2, 3, etc...). Your master is a particularly absent-minded lout and continually leaves doors open throughout a particular floor of the house. Over the years, you have mastered the art of traveling in a single path through the sloppy rooms and closing the doors behind you. Your biggest problem is determining whether it is possible to find a path through the sloppy rooms where you:
  Always shut open doors behind you immediately after passing through
 Never open a closed door
 End up in your chambers (room 0) with all doors closed
 In this problem, you are given a list of rooms and open doors between them (along with a starting room). It is not needed to determine a route, only if one is possible.

Input

Input to this problem will consist of a (non-empty) series of up to 100 data sets. Each data set will be formatted according to the following description, and there will be no blank lines separating data sets.
A single data set has 3 components:
  Start line - A single line, "START M N", where M indicates the butler's starting room, and N indicates the number of rooms in the house (1 <= N <= 20).
 Room list - A series of N lines. Each line lists, for a single room, every open door that leads to a room of higher number. For example, if room 3 had open doors to rooms 1, 5, and 7, the line for room 3 would read "5 7". The first line in the list represents room 0. The second line represents room 1, and so on until the last line, which represents room (N - 1). It is possible for lines to be empty (in particular, the last line will always be empty since it is the highest numbered room). On each line, the adjacent rooms are always listed in ascending order. It is possible for rooms to be connected by multiple doors!
 End line - A single line, "END"
 Following the final data set will be a single line, "ENDOFINPUT".
Note that there will be no more than 100 doors in any single data set.

Output

For each data set, there will be exactly one line of output. If it is possible for the butler (by following the rules in the introduction) to walk into his chambers and close the final open door behind him, print a line "YES X", where X is the number of doors he closed. Otherwise, print "NO".

Sample Input

START 1 2 1 END START 0 5 1 2 2 3 3 4 4 END START 0 10 1 9 2 3 4 5 6 7 8 9 END ENDOFINPUT

Sample Output

YES 1 NO YES 10