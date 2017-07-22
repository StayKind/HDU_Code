HDU_1599 find the mincost route【Floyd】
<!--more-->
>[原题链接](http://acm.hdu.edu.cn/showproblem.php?pid=1599)

### 题目意思： ###
给出N个景区，判断最少经过三个景区形成环的最少花费。
### 解题思路： ###
最初的想法，构成图以后，把其中一条路经给截断，然后算这条路径两点的最短路径，最后把所有这样的路径算完得出最小的答案，如果为无穷大，则输出It's impossible.否则输出最小的花费。但是就是过不了，Dijkstra 过不了，又是WA，又是RE。然后想用Floyd，但是使用方法不当，导致TLE。
好了，无限错以后，开始回归正解的思想。
Floyd本身只是比较暴力的dp求各点之前的最短路径。但是，在Floyd更新的过程中也是有顺序的。可以借助这一点来解决这个题目。
首先先说一下如何来确定环的花费最小。开始先将ans赋值为无穷大。然后我们可以先选取两个点，两点之间的最短路径为`dis[i][j]`,再找一个大于i，j的一个点k，做松弛操作;进行下面的操作`ans=min(ans,dis[i][j] + mp[i][k] + mp[k][j]);`来不断更新环的最小花费。
当然Floyd的操作在松弛操作之后，可以保证i到j之间的最短距离不经过k。这样也可以保证经过的最小花费的路径为一个环。
如果还是不太了解，可以参考一下代码部分。
### 代码部分： ###
```cpp
#include <iostream>
#define INF 1000000
#define maxn 110
using namespace std;
int n,m;
int mp[maxn][maxn];//邻接矩阵，用来存放路径
int dis[maxn][maxn];//用来存储最两点之间的最短路径
void initmp()//对mp的初始化
{
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= n; j++)
        {
            if(i == j)
                mp[i][j] = mp[j][i] = 0;
            else
                mp[i][j] = mp[j][i] = INF;
        }
}
void floyd()  //floyd函数
{
    for(int i = 1; i <= n; i++)
    {
        for(int j = 1; j<= n; j++)
        {
            dis[i][j]=mp[i][j];
        }
    }
    int ans = INF;
    for(int k = 1; k <= n; k++)
    {
        for(int i = 1; i < k; i++)
        {
            for(int j = i + 1; j < k; j++)
            {
                ans=min(ans,dis[i][j] + mp[i][k] + mp[k][j]);//在加入k更新最短路径之前进行求环最小花费的松弛操作
            }
        }
        for(int i = 1; i <= n; i++)  //进行普通的Floyd
        {
            for(int j = 1; j <= n; j++)
            {
                if(dis[i][j] > dis[i][k] + dis[k][j])
                    dis[i][j] = dis[i][k] + dis[k][j];
            }
        }
    }
    if(ans == INF)
        cout << "It's impossible." << endl;
    else
        cout << ans << endl;
}
int main()
{

    int a,b,c;
    while(cin>>n>>m)
    {
        initmp();
        for(int i = 1; i <= m; i++)
        {
            cin >> a >> b >> c;
            if(mp[a][b] > c)// 在给定的边可能会有重复，需要求最小的花费，所以需要更新重边
            {
                mp[a][b] = mp[b][a] = c;
            }
        }
        floyd();
    }

    return 0;
}

```