HDU_6121 Build a tree 【DFS&&位运算技巧】
<!--more-->
> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6121)

### 题目描述 ###
给定一个n， k。建一颗k叉树，有n个节点。这棵树节点的值代表这棵树子节点的个数加上本身的节点总数之和。求所有节点值的异或。
Limit n和k都是 1e18 的范围。
### 解题思路 ###
根据数据的大小，可以知道肯定不是直接递归来求的答案。
分两种情况：

- k为1的时候，为求 1 ~ n的异或，如果一个一个异或肯定超时，这里有个规律，见代码部分，函数可以直接求处1 ~ n 的异或值。
- k > 1时。我们可以通过给的节点来创建树，不难发现，这棵树中只有一颗是一个不满的k叉树，这棵树左边都为满k叉树，右边的也是满k叉树，比右边的会少一层。这样我们就可以直接求出左右满k叉树的异或值然后再异或上不满足k叉树的那棵树即可。不满足k叉树的我们就可以递归进行计算。每次通过递归来计算不满k叉树的左右满k叉树的数量以及其异或值。因为当k大于1时，最多递归的次数也不会超过100次，所以时间肯定是够的。
- 最后，奇数个n异或的值相当于n，偶数个n异或为0。

### 代码部分 ###
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
///计算1 ~ n的 异或值
ll ans;
inline ll Xor(ll n)
{
    return n%4==1?1:(n%4==2?n+1:(n%4==3?0:n));
}
void dfs(ll n, ll k)
{
    if(n - 1 <= k)
    {
        if(!(n & 1)) ans ^= 1;
        return;
    }
    ll cnt = 1, sum = 1;
    while(sum + cnt * k < n)
    {
        cnt *= k;
        sum += cnt;
    }
    ll l = (n - sum - 1) / cnt;
    ll r = k - 1 - l;
    if(l & 1) ans ^= sum;
    if(r & 1) ans ^= (sum - cnt);
    n -= (1 + l * sum + r * (sum - cnt));
    ans ^= n;
    dfs(n, k);
}
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin >> t;
    while(t --)
    {
        ll n, k;
        cin >> n >> k;
        if(k == 1)
        {
            cout << Xor(n) << endl;
        }
        else
        {
            ans = n;
            dfs(n, k);
            cout << ans << endl;
        }
    }
    return 0;
}

```