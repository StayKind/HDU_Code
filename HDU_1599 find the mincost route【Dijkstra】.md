### 另种解法
如果i-j有边，先保存边的权值，然后删除这条边，再求i-j的最短路径，然后加上i-j这条边的值，在恢复i-j的边，对所有的边进行这样的操作，然后找出得到结果中的最小值。
### 代码部分
```cpp
#include <iostream>  
#include <stdio.h>  
#include <algorithm>  
#define inf 0x3f3f3f3f  
using namespace std;  
  
const int maxn = 110;  
int n,m;  
int Map[maxn][maxn];  
int dis[maxn];  
int vis[maxn];  
void init_map()  
{  
    for(int i = 1; i <= n; i++)  
        for(int j = 1; j <= n; j++)  
    {  
            if(i == j) Map[i][j]= 0;  
            else Map[i][j] = inf;  
    }  
}  
void dijkstra(int start)  
{  
    for(int i = 1; i <= n; i++)  
    {  
        dis[i] = Map[start][i];  
        vis[i] = 0;  
    }  
    vis[start] = 1;  
    int mindis,u;  
    for(int i = 1; i <= n; i++)  
    {  
        mindis = inf;  
        u = 0;    
        /**每次让u = 0,不会与其他定点编号冲突，而且下个  
        for循环执行完后，u还等于0，则程序就可以结束的，  
        在杭电写题得加上，不然的会显示RE，遇到过两次这样  
        的问题**/  
        for(int j = 1; j <= n; j++)  
        {  
            if(vis[j]==0 && dis[j]<mindis)  
            {  
                mindis = dis[j];  
                u = j;  
            }  
        }  
        if(u == 0) break;  
        vis[u] = 1;  
        for(int j = 1; j <= n; j++)  
        {  
            if(vis[j]==0)  
            {  
                if(Map[u][j]<inf && dis[u]+Map[u][j]<dis[j])  
                    dis[j] = dis[u]+Map[u][j];  
            }  
        }  
    }  
}  
int main()  
{  
    while(~scanf("%d%d",&n,&m))  
    {  
        init_map();  
        int a,b,c;  
        for(int i = 0; i < m; i++)  
        {  
            scanf("%d%d%d",&a,&b,&c);  
            if(c < Map[a][b])  
            {  
                Map[a][b] = Map[b][a] = c;  
            }  
        }  
        int ans = inf;  
        for(int i = 1; i <= n; i++)  
            for(int j = i+1; j <= n; j++)  
            {  
                if(Map[i][j]!=inf)  
                {  
                    int temp = Map[i][j];  
                    Map[i][j] = Map[j][i] = inf; ///删边求解  
                    dijkstra(i);  
                    ans = min(ans,dis[j]+temp);  
                    Map[i][j] = Map[j][i] = temp;  
                }  
            }  
        if(ans < inf)  
            printf("%d\n",ans);  
        else  
            printf("It's impossible.\n");  
    }  
    return 0;  
}  
```

>第二种方法参见队友的代码详情点链接：[温姑娘的博客]( http://blog.csdn.net/wyxeainn/article/details/75807616)