### 1003. Colorful Tree ###
> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6035)

#### 题目意思： ####
给出n个点的颜色，n-1条路径，任意两点之间路径的权值为路径中不同颜色的数目，求所有路径的权值之和
#### 解题思路： ####
逆序思维，这道题就可以转换为，假设所有的路径都经过所有的颜色，然后将其减去i颜色没有经过的路径之和，就可以算出与题目所求相同的结果。
#### 解题实现： ####
所有路径都经过所有颜色，这时候权值可以为颜色的数量乘以总的路径数即 num((n-1)n/2)。
这道题目的难点就是过来求颜色为i的时候，没有经过颜色i的路径之和为多少。
没有经过颜色i的分两种：
1. 在子树中，部分节点k个都没有经过i颜色，那么路径的条数就应该加上(k-1)k/2。该过程在dfs的函数中实现。
2. 再算子树的过程中，都是算的没有经过根节点的路径，这时候就可以借助一个sum数组来求出剩下的所有没有经过的i颜色的路径条数。

在dfs的过程中，如果找到子树中k个点都没有经过i颜色，就把这几个就应该把路径条数给算上，然后将其颜色模拟成和i颜色相同（sum[i] += k，模拟过程只是改变sum[i]数组的值，并未改变其本身的颜色）。
这样在将根节点包含进去的时候，就不会重复计算子节点中的颜色不同的路径了。经过dfs最后得到的sum[]就表示，整棵树中，模拟过的i颜色的点的个数有几个，用ct = n-sum[i]表示没有经过颜色i的点的个数有多少个，进而可以推出路径的条数。
#### 代码实现： ####

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 4e5+100;
typedef long long ll;
int vis[N];///c[i]表示第i个点的颜色，vis[i]表示的是i这个颜色有没有出现

//DFS 中需要用到的
vector<int> e[N];///存储这个图呢
ll c[N],sum[N],size[N];///size[i]表示的是以i为根的点的个数，c[x]可以表示为x的颜色。sum[i]模拟颜色改变后i颜色在整个树中的个数。
ll ans;//表示没有经过i颜色的路径个数（i : 1~n）

void dfs(int x, int y)//x代表当前节点y代表上一个节点
{

    size[x] = 1;/// 指的是x为根的树的点的个数
    sum[c[x]] ++;
    ll pre = sum[c[x]];
    for(int i = 0; i < e[x].size(); i++)
    {

        if(e[x][i] == y)
            continue;
        dfs(e[x][i], x);


        size[x] += size[e[x][i]];/// 以x为根的树的点的总个数，当前的这个点还要加上他的子数上的点
        ll count = size[e[x][i]] - (sum[c[x]] - pre);//count 表示子树中无x点颜色的个数，用来求子树中没有包含x的路径的个数。
        ans = ans + (1LL * count * (count - 1)) / 2;//算出子树中的无某种颜色的个数
        sum[c[x]] += count;//将算入路径中的颜色模拟成x的颜色。这里注意，如果子树中无x颜色的点的个数为1时，也是构不成路径，但是也要算在sum[x]里面，因为这样的节点在整棵树中也是无法构成路径的
        pre = sum[c[x]];
    }
}

int main()
{
    int n,cas=1;
    while(scanf("%d",&n)!=EOF)
    {
        int num=0;
        ans=0;
        memset(sum,0,sizeof(sum));
        memset(vis,0,sizeof(vis));
        for(int i=1; i<=n; i++)
        {
            e[i].clear();
            scanf("%d",&c[i]);
            if(!vis[c[i]])
            {
                vis[c[i]]=1;///标记这个颜色出现
                num++;///不同的颜色数
            }
        }
        for(int i=1; i<n; i++)
        {
            int u,v;
            scanf("%d%d",&u,&v);
            e[u].push_back(v);
            e[v].push_back(u);
        }
        dfs(1,0);

        ll ANS = 1LL*num*((1LL)*n*(n-1))/2;
        /// 这里要算的就是整棵树中没有经过颜色i的路径的个数（除去子树中路径的个数）
        for(int i=1; i<=n; i++)
        {
            if(vis[i])
            {
                ll ct=n-sum[i];//ct ?
                ans+=ct*(ct-1)/2;
            }
        }
        printf("Case #%d: %lld\n", cas++, ANS-ans);
    }
}
```