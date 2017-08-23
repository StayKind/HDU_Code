
<!--more-->
> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6165)

### 题目描述 ###
这题目真的是让人头疼，意思就是给你一个有向图，问是否任意两点可以到达（正反都可以）。
### 解题思路 ###
深搜遍历出所有的点可以到达的点进行一个标记，如果一点可以到达另一点，则标记可以到达，最后遍历一个n^2，判断是否两点之间无一条通路，如果有就说明这两个情侣没法见面，输出Light my fire!，否则输出I love you my love and our love save us!
### 代码部分 ###
```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e3 + 10;
typedef long long ll;
vector <int> G[maxn]; //尾插法建立链表
bool vis[maxn][maxn];
bool bj[maxn];

void dfs(int s, int e)
{
    bj[e] = true;
    for(int i = 0; i < (int)G[e].size(); ++ i)
    {
        int t = G[e][i];
        if(bj[t] == false)
        {
            vis[s][t] = true;
            dfs(s, t);
        }
    }
}
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin >> t;
    while(t --)
    {
        int n, m, u, v;
        cin >> n >> m;
        for(int i = 1; i <= n; ++ i) G[i].clear();
        for(int i = 1; i <= m; ++ i)
        {
            cin >> u >> v;
            G[u].push_back(v);
        }
        memset(vis, false, sizeof(vis));
        for(int i = 1; i <= n; ++ i)
        {
            memset(bj, false, sizeof(bj));
            dfs(i, i);
        }
        bool flag = true;
        for(int i = 1; i <= n; ++ i)
        {
            for(int j = 1; j <= n; ++ j)
            {
                if(i != j)
                {
                    if(vis[i][j] == false && vis[j][i] == false)
                    {
                        flag = false;
                        break;
                    }
                }
            }
            if(flag == false)
                break;
        }
        if(flag) puts("I love you my love and our love save us!");
        else puts("Light my fire!");
    }
    return 0;
}

```
