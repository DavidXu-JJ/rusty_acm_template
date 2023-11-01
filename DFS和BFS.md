# DFS（Deepest First Search）与 BFS（Breadth First Search）	O(n<sup>2</sup>)
## 1.洛谷P1605 迷宫	（DFS）
### 题目背景

给定一个N*M方格的迷宫，迷宫里有T处障碍，障碍处不可通过。给定起点坐标和终点坐标，问: 每个方格最多经过1次，有多少种从起点坐标到终点坐标的方案。在迷宫中移动有上下左右四种方式，每次只能移动一个方格。数据保证起点上没有障碍。

### 题目描述

无

### 输入格式

第一行N、M和T，N为行，M为列，T为障碍总数。第二行起点坐标SX,SY，终点坐标FX,FY。接下来T行，每行为障碍点的坐标。

### 输出格式

给定起点坐标和终点坐标，问每个方格最多经过1次，从起点坐标到终点坐标的方案总数。
### 输入输出样例
**输入 #1**

```
2 2 1
1 1 2 2
1 2
```

**输出 #1**

```
1
```

###说明/提示

【数据规模】

1≤N,M≤5

**DFS代码**

```c++
#include<iostream>
#include<algorithm>
#include<cmath>
#include<cstring>
#define ll long long
using namespace std;
char map [10][10];								//地图
bool is_walk[10][10];							//表示每个点是否走过
int dx[4]={1,0,-1,0};							//存储走的方法
int dy[4]={0,1,0,-1};
int bx,by,ex,ey;
int cnt;										//存储走的方案总数
void dfs(int x,int y)
{
    if(x==ex&&y==ey)							//到达终点
    {
        cnt++;
        return ;
    }
    else
    {
        for(int i=0;i<4;i++)
        {
            if(map[x+dx[i]][y+dy[i]]!='\0'&&map[x+dx[i]][y+dy[i]]!='@'
            &&is_walk[x+dx[i]][y+dy[i]]!=true)	//判断是否能走或是否之前走过
            {
                is_walk[x+dx[i]][y+dy[i]]=true;			//置为走过
                dfs(x+dx[i],y+dy[i]);					//DFS
                is_walk[x+dx[i]][y+dy[i]]=false;		//回溯
            }
        }
    }
    
}
int main()
{
    ll n,m,t;
    cin>>n>>m>>t;
    for(int i=1;i<=n;i++)
    for(int j=1;j<=m;j++)
    {
        map[i][j]='1';					//'1'表示路能走
    }
    cin>>bx>>by>>ex>>ey;
    is_walk[bx][by]=true;
    while(t--)
    {
        ll x,y;
        cin>>x>>y;
        map[x][y]='@';					//表示障碍
    }
    dfs(bx,by);
    cout<<cnt;
    return 0;
}
```

> 有些大佬会用二进制优化，能把时间复杂度降低很多，但太难看懂了。。。

## 2.洛谷P1443 马的遍历 （BFS）
### 题目描述

有一个n*m的棋盘(1<n,m<=400)，在某个点上有一个马,要求你计算出马到达棋盘上任意一个点最少要走几步

### 输入格式

一行四个数据，棋盘的大小和马的坐标

### 输出格式

一个n*m的矩阵，代表马到达某个点最少要走几步（左对齐，宽5格，不能到达则输出-1）
### 输入输出样例
**输入 #1**

```
3 3 1 1
```

**输出 #1**

```
0    3    2    
3    -1   1    
2    1    4    
```

**DFS代码**

```c++
#include<iostream>
#include<algorithm>
#include<cmath>
#include<cstring>
#define ll long long
using namespace std;
int map[410][410];
bool is_walk[410][410];
const int offset=5;
int n,m,bx,by;
int dx[8]={1,1,2,2,-1,-1,-2,-2};
int dy[8]{2,-2,1,-1,2,-2,1,-1};
void dfs(int i,int x,int y)
{
    int tri=0;
    for(tri=0;tri<8;tri++)
    {
        if(!is_walk[x+dx[tri]][y+dy[tri]]&&map[x+dx[tri]][y+dy[tri]]!=100000)
        //判断是否能走
        {
            is_walk[x+dx[tri]][y+dy[tri]]=true;
            map[x+dx[tri]][y+dy[tri]]=min(map[x+dx[tri]][y+dy[tri]],i);
            dfs(i+1,x+dx[tri],y+dy[tri]);
            is_walk[x+dx[tri]][y+dy[tri]]=false;
        }
    }
}
int main()
{
    for(int i=0;i<410;i++)
    for(int j=0;j<410;j++)
    map[i][j]=100000;		//标记边界
    cin>>n>>m>>bx>>by;
    for(int i=offset+1;i<n+offset+1;i++)
    for(int j=offset+1;j<m+offset+1;j++)
    {
        map[i][j]=0x3f;		//标记未走过的点
    }
    map[bx+offset][by+offset]=0;
    is_walk[bx+offset][by+offset]=true;
    dfs(1,bx+offset,by+offset);
    for(int i=offset+1;i<offset+n+1;i++)
    {
        for(int j=offset+1;j<offset+m+1;j++)
        if(map[i][j]==0x3f)
            cout<<"-1 ";
        else cout<<map[i][j]<<" ";
        cout<<endl;
    }
    return 0;
}
```

>DFS能够算出答案，但实在是太慢了,过不了题。。。应该使用BFS

>或许DFS能过，不过可能要剪枝

**BFS代码**

```c++
#include<iostream>
#include<algorithm>
#include<cmath>
#include<cstring>
#include<queue>
#include<cstdio>
#define ll long long
using namespace std;
struct node
{
    int x,y;
    node(int x_,int y_):x(x_),y(y_){}
};
int n,m,bx,by;
int map[401][401];					//记录每个点到的步数
bool is_walk[401][401];				//判断是否走过
int dx[8]={1,1,2,2,-1,-1,-2,-2};	//记录马走的方式
int dy[8]{2,-2,1,-1,2,-2,1,-1};
queue<node> q;						//生成队列
void bfs(int x,int y,int step)
{
    map[x][y]=step;
    is_walk[x][y]=true;
    node now(x,y);
    q.push(now);
    while(!q.empty())
    {
        node top=q.front();
        q.pop();
        for(int i=0;i<8;i++)
        {
            node newnode(top.x+dx[i],top.y+dy[i]);//按八个方式中的一种生成的一个点
            if(newnode.x<1||newnode.x>n||newnode.y<1||newnode.y>m)//判断是否过界
            {
                continue;				//若过界就判断下一个点
            }
            else
            {
                if(is_walk[newnode.x][newnode.y]==false)
                {
                    q.push(newnode);	
                    //将这个节点压入队列，在这个点出队列的时候再讨论它可以到的点
                    is_walk[newnode.x][newnode.y]=true;
                    map[newnode.x][newnode.y]=map[top.x][top.y]+1;	
                    //上一点步数加一
                }
            }
        }
    }
}
int main()
{
    cin>>n>>m>>bx>>by;
    memset(map,-1,sizeof(map));
    bfs(bx,by,0);
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            printf("%-5d",map[i][j]);	//左对齐输出
        }
        cout<<endl;
    }
}

```

百度中把DFS和BFS的原理区别分别使用栈和队列解释，蛮有意思的<https://baike.baidu.com/item/宽度优先搜索/5224802?fromtitle=BFS&fromid=542084&fr=aladdin>

## DFS

>
深度优先搜索用栈（stack）来实现，整个过程可以想象成一个倒立的树形：
>
1、把根节点压入栈中。
>
2、每次从栈中弹出一个元素，搜索所有在它下一级的元素，把这些元素压入栈中。并把这个元素记为它下一级元素的前驱。
>
3、找到所要找的元素时结束程序。
>
4、如果遍历整个树还没有找到，结束程序。

## BFS

>
广度优先搜索使用队列（queue）来实现，整个过程也可以看做一个倒立的树形：
>
1、把根节点放到队列的末尾。
>
2、每次从队列的头部取出一个元素，查看这个元素所有的下一级元素，把它们放到队列的末尾。并把这个元素记为它下一级元素的前驱。
>
3、找到所要找的元素时结束程序。
>
4、如果遍历整个树还没有找到，结束程序。
