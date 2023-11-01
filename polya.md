## Pólya定理 
设G={π1，π2，…，πt}是X={a1,a2,…,an}上一个置换群，用m种颜色对X中的元素进行涂色，那么不同的涂色方案数为

![avatar](/Users/davidxu/Desktop/算法笔记md/polya相关/polya.png)

其中Cyc(πk)是置换πk的循环节的个数。(大多数情况下要自己画图数)

## 求循环节

![avatar](/Users/davidxu/Desktop/算法笔记md/polya相关/求循环节.png)

```c++
#include<iostream>
#include<cmath>
#include<iomanip>
#include<vector>
#include<algorithm>
using namespace std;
int zhihuan[100];
int vis[100];
void search(int st)
{
    cout<<"("<<st;
    int chk=zhihuan[st];
    vis[st]=1;
    while(chk!=st)
    {
        vis[chk]=1;
        cout<<" "<<chk;
        chk=zhihuan[chk];
    }
    cout<<")";
}
int main()
{
    int n;
    while(cin>>n)
    {
        memset(vis, 0, sizeof(vis));
        for(int i=1;i<=n;i++)
        {
            cin>>zhihuan[i];
        }
        for(int i=1;i<=n;i++)
        {
            if(!vis[i])
            {
                search(i);
            }
        }
        cout<<endl;
    }
}

```

## 长度为n的环形串的个数
根据Pólya定理，长度为n的环形串的个数L=

![avatar](/Users/davidxu/Desktop/算法笔记md/polya相关/环形串个数.png)

 L的数目很大，实际编程时需要自己编写计算幂函数pow(m,x)的程序，可能需要用到高精度计算 

