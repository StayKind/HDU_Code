HDU_6090 Rikka with Graph 【贪心】

<!--more-->
> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6090)

### 题目描述 ###
给定n个点，m条边，让你安排点和边构成一个无向图。dis(i,j)，表示i到j最小的边数，如果无法到达，dis(i,j)为n,问每个点到其他所有的点的dis之和。
### 解题思路 ###
贪心思想，构图的时候一个点当中点，其他的点都围绕在周围，有多少个边连多少个点。
分情况考虑。
- 当任意两点都联通，也就是$$ `m >= n*(n-1)/2` $$。得到的答案就直接为$$ `n*(n-1)` $$
- 再有就是将 m = n - 1 为边界，分情况考虑即可
- 当 m≤n−1 时，我们需要最小化距离为 n 的点对数，所以肯定是连出一个大小为 m+1 的联通块，剩下的点都是孤立点。在这个联通块中，为了最小化内部的距离和，肯定是连成一个菊花的形状，即一个点和剩下所有点直接相邻。
- 当 m>n−1 时，肯定先用最开始 n−1 条边连成一个菊花，这时任意两点之间距离的最大值是 2。因此剩下的每一条边唯一的作用就是将一对点的距离缩减为 1。

### 代码部分 ###
```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin >> t;
    while(t--)
    {
        ll n, m, ans;
        cin >> n >> m;
        ans = (n - 1) * (n - 1) * 2;
        if(m >= n * (n - 1) / 2) {cout << n * (n - 1) << endl; continue;}//当每个点都可以相互链接时，距离和直接为 n * (n - 1)
        if(m < n - 1) cout << 2 * m * m + n * (n + m) * (n - m - 1) << endl; //另一种情况就是以m = n - 1 为边界考虑
        else
        {
            ll cnt = m - n + 1;
            cout << ans - cnt * 2 << endl;
        }
    }
    return 0;
}
```