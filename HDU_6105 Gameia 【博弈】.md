HDU_6105 Gameia 【博弈】
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6105)


### 题目描述 ###
Alice和Bob玩一个游戏，一开始有一颗没有颜色的树，Bob和Alice分别对树进行染色，Alice每次将一个没有颜色的点涂成白色，Bob每次将一个没有颜色的点涂成黑色，并且可以将与涂上黑色的点直接相邻的点变为黑色，假如最后树上存在白色点，Alice赢，否则Bob赢。Bob还有一个特权，可以在任意时候，删除任意一条边。
### 解题思路 ###
只有当n为偶数且Bob可以根据他的特权将这棵树切成两两相连的时候，才可以获得胜利，否则都是Alice赢。
### 代码部分 ###
```
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e3;
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin >> t;
    while(t --)
    {
        int n, k, fa[maxn], si[maxn];
        bool flag = true;
        cin >> n >> k;
        for(int i = 2; i <= n; ++ i) cin >> fa[i];
        for(int i = 1; i <= n; ++ i) si[i] = 1;
        for(int i = n; i >= 1; -- i)
        {
            if(si[i] >= 3)
                flag = false;
            si[fa[i]] += si[i] & 1;
        }
        if(flag && n % 2 == 0 && k >= n / 2 - 1) cout << "Bob" << endl;
        else cout << "Alice" << endl;
    }
    return 0;
}

```