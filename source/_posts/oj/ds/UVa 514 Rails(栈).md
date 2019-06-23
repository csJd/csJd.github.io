---
title: UVa 514 Rails(栈)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2014-09-02 09:50:30
---
题意 有n辆火车 按1到n的顺序进站 最后进站的车可以在任何时候出去 判断给定的出站序列是否可能

火车只有两种状态 从A进站 或者从站到B 模拟栈的操作就行了

令A表示A中当前待进站的第一辆火车 tar[B]表示出站序列中当前应该出站的火车 sta为火车站

当A==tar[B]的时候 A进站马上出战 否则当站中最后一辆==tar[B]时 这辆车出站 都不满足就只能A中的最前面的火车进站

当n辆火车全部进站 而站中还有火车是 给定的出战序列就是不可能的

```js 
#include<cstdio>
#include<stack>
using namespace std;
const int N = 1005;
int n, tar[N], A, B;
int main()
{
    while (scanf ("%d", &n), n)
    {
        while (scanf ("%d", &tar[1]), tar[1])
        {
            for (int i = 2; i <= n; ++i)
                scanf ("%d", &tar[i]);
            stack<int> sta;
            A = B = 1;
            bool ok = true;
            while (B <= n)
            {
                if (A == tar[B])
                {   ++A; ++B;   }
                else if (!sta.empty() && sta.top() == tar[B])
                {   sta.pop();  ++B;  }
                else if (A <= n)
                    sta.push (A++);
                else
                {   ok = false;  break;  }
            }
            printf (ok ? "Yes\n" : "No\n");
        }
        printf("\n");
    }
    return 0;
}
```

#

****

There is a famous railway station in PopPush City. Country there is incredibly hilly. The station was built in last century. Unfortunately, funds were extremely limited that time. It was possible to establish only a surface track. Moreover, it turned out that the station could be only a dead-end one (see picture) and due to lack of available space it could have only one track.

![\begin{picture}(6774,3429)(0,-10)\put(1789.500,1357.500){\arc{3645.278}{4.7247}......tFigFont{14}{16.8}{\rmdefault}{\mddefault}{\updefault}Station}}}}}\end{picture}](../images/dge.org-external-5-p514.gif.png)

The local tradition is that every train arriving from the direction A continues in the direction B with coaches reorganized in some way. Assume that the train arriving from the direction A has![$N \leŸ 1000$](../images/dge.org-external-5-514img2.gif.png) coaches numbered in increasing order ![$1, 2, \dots, N$](../images/dge.org-external-5-514img3.gif.png). The chief for train reorganizations must know whether it is possible to marshal coaches continuing in the direction B so that their order will be ![$a_1. a_2, \dots, a_N$](../images/dge.org-external-5-514img4.gif.png). Help him and write a program that decides whether it is possible to get the required order of coaches. You can assume that single coaches can be disconnected from the train before they enter the station and that they can move themselves until they are on the track in the direction B. You can also suppose that at any time there can be located as many coaches as necessary in the station. But once a coach has entered the station it cannot return to the track in the direction A and also once it has left the station in the direction B it cannot return back to the station.

##

The input file consists of blocks of lines. Each block except the last describes one train and possibly more requirements for its reorganization. In the first line of the block there is the integer N described above. In each of the next lines of the block there is a permutation of ![$1, 2, \dots, N$](../images/dge.org-external-5-514img3.gif.png) The last line of the block contains just 0.

The last block consists of just one line containing 0.

##

The output file contains the lines corresponding to the lines with permutations in the input file. A line of the output file contains Yes if it is possible to marshal the coaches in the order required on the corresponding line of the input file. Otherwise it contains No . In addition, there is one empty line after the lines corresponding to one block of the input file. There is no line in the output file corresponding to the last ``null'' block of the input file.

##

5 1 2 3 4 5 5 4 1 2 3 0 6 6 5 4 3 2 1 0 0

##

Yes No Yes

#

****

There is a famous railway station in PopPush City. Country there is incredibly hilly. The station was built in last century. Unfortunately, funds were extremely limited that time. It was possible to establish only a surface track. Moreover, it turned out that the station could be only a dead-end one (see picture) and due to lack of available space it could have only one track.

![\begin{picture}(6774,3429)(0,-10)\put(1789.500,1357.500){\arc{3645.278}{4.7247}......tFigFont{14}{16.8}{\rmdefault}{\mddefault}{\updefault}Station}}}}}\end{picture}](../images/dge.org-external-5-p514.gif.png)

The local tradition is that every train arriving from the direction A continues in the direction B with coaches reorganized in some way. Assume that the train arriving from the direction A has![$N \leŸ 1000$](../images/dge.org-external-5-514img2.gif.png) coaches numbered in increasing order ![$1, 2, \dots, N$](../images/dge.org-external-5-514img3.gif.png). The chief for train reorganizations must know whether it is possible to marshal coaches continuing in the direction B so that their order will be ![$a_1. a_2, \dots, a_N$](../images/dge.org-external-5-514img4.gif.png). Help him and write a program that decides whether it is possible to get the required order of coaches. You can assume that single coaches can be disconnected from the train before they enter the station and that they can move themselves until they are on the track in the direction B. You can also suppose that at any time there can be located as many coaches as necessary in the station. But once a coach has entered the station it cannot return to the track in the direction A and also once it has left the station in the direction B it cannot return back to the station.

##

The input file consists of blocks of lines. Each block except the last describes one train and possibly more requirements for its reorganization. In the first line of the block there is the integer N described above. In each of the next lines of the block there is a permutation of ![$1, 2, \dots, N$](../images/dge.org-external-5-514img3.gif.png) The last line of the block contains just 0.

The last block consists of just one line containing 0.

##

The output file contains the lines corresponding to the lines with permutations in the input file. A line of the output file contains Yes if it is possible to marshal the coaches in the order required on the corresponding line of the input file. Otherwise it contains No . In addition, there is one empty line after the lines corresponding to one block of the input file. There is no line in the output file corresponding to the last ``null'' block of the input file.

##

5 1 2 3 4 5 5 4 1 2 3 0 6 6 5 4 3 2 1 0 0

##

Yes No Yes