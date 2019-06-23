---
title: POJ 1856 Sea Battle(DFS)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Search

date: 2014-09-02 09:51:39
---
题意 图中每个矩形'/#'连通块代表一艘船 若一艘船与另一艘有边相邻或有角相邻 那么认为这两艘船相撞 若图中有船相撞 输出bad 否则输出图中有多少艘船

可以把图的周围全包上一圈'.' 遍历图中每个点 可知当图中存在一下四种结构中的一个时 必有船相撞 输出并退出循环 否则则dfs这个点 若图中不存在这些结构 就可以输出连通块数量即轮船数了

```js 
<span style="font-family:Microsoft YaHei;">#include<cstdio>
#include<cstring>
using namespace std;

const int N=1005;
char mat[N][N];bool vis[N][N];
int x[4]= {-1,1,0,0},y[4]= {0,0,-1,1};

bool isBad(int i,int j)
{
    if(mat[i][j]=='.'&&mat[i-1][j]=='#'&&mat[i][j-1]=='#') return 1;
    if(mat[i][j]=='.'&&mat[i-1][j]=='#'&&mat[i][j+1]=='#') return 1;
    if(mat[i][j]=='.'&&mat[i+1][j]=='#'&&mat[i][j-1]=='#') return 1;
    if(mat[i][j]=='.'&&mat[i+1][j]=='#'&&mat[i][j+1]=='#') return 1;
    return 0;
}

int dfs(int i,int j)
{
    if (vis[i][j]||mat[i][j]=='.') return 0;
    vis[i][j]=true;
    for(int k=0; k<4; ++k)
        if(mat[i+x[k]][j+y[k]]=='#') dfs(i+x[k],j+y[k]);
    return 1;
}

int main()
{
    int i,j,n,m;
    while(scanf("%d%d",&n,&m),n)
    {
        for(i=1; i<=n; ++i)
            scanf("%s",mat[i]+1);
        for(i=0; i<=n+1; ++i)  mat[i][0]=mat[i][m+1]='.';
        for(j=0; j<=m+1; ++j)  mat[0][j]=mat[n+1][j]='.';
        int ans=0;
        memset(vis,0,sizeof(vis));
        for(i=1; i<=n; ++i)
        {
            for(j=1; j<=m; ++j)
                if(isBad(i,j)) break;
                else ans+=dfs(i,j);
            if (j<=m) break;
        }

        if(i<=n)
        {
            printf("Bad placement.\n");
            continue;
        }

        printf("There are %d ships.\n",ans);
    }
    return 0;
}
</span>
```

Sea Battle

Description
During the Summit, the armed forces will be highly active. The police will monitor Prague streets, the army will guard buildings, the Czech air space will be full of American F-16s. Moreover, the ships and battle cruisers will be sent to guard the banks of the Vltava river. Unfortunately, in the case of any incident, the Czech Admiralty have only a few captains able to control over the large sea battle. Therefore, it was decided to educate new admirals. As an excellent preparation, the game of "Sea Battle" was chosen to help with their study program.
In this well-known game, a predefined number of ships of predefined shapes are placed on the square board in such a way that they cannot contact one another even with their corners. In this task, we will consider rectangular shaped ships only. The unknown number of rectangular ships of unknown sizes are placed on a rectangular board. All the ships are full rectangles built of hash characters. Write a program that counts the total number of ships present in the field.

Input

The input consists of more scenarios. The description of each scenario begins with two integer numbers R and C separated with a single space, 1 <= R,C <= 1000. These numbers give the number of rows and columns in the game field.
After these two numbers, there are R lines, each of them containing C characters. Each character is either hash ("/#") or dot ("."). Hashes denote ships, dots water.
Then, the next scenario description begins. At the end of the input, there will be a line containing two zeros instead of the field size.

Output

Output a single line for every scenario. If the ships were placed correctly (i.e., there are only rectangles that do not touch each other even with a corner), print the sentence "There are S ships." where S is the number of ships.
Otherwise, print the sentence "Bad placement.".

Sample Input

6 6 ...../# /#/#.../# /#/#.../# ../#../# ...../# /#/#/#/#/#/# 6 8 ...../#./# /#/#...../# /#/#...../# ......./# /#....../# /#../#.../# 0 0

Sample Output

Bad placement. There are 5 ships.