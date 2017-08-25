HDU_6170 Numbers
<!--more-->
> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6168)

### 题目描述 ###
给入n个数，里面有a序列的数和b序列的数，b序列的数为a序列两两组合得到。最后从小到大输出a序列。
### 解题思路 ###
根据题目要求得到a序列的数目n (n + n * (n - 1) / 2) = m;)
然后将输入的每个数都存到map当中，找出输入的最小的两个肯定是a序列的前两个，然后根据得到的两个数可以排除一个b，让后让其在map中计数减一，迭代出map中最小的就是a序列的其中一个，然后接着排除b，最后得到n个数即可。
### 代码部分 ###
```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e4;
const int inf = 1e9;
typedef long long ll;
ll a[maxn];
int main()
{
    ll num, m;
    while(~scanf("%lld", &m))
    {
        map <ll, ll> M;
        ll min1 = inf, min2 = inf + 10;
        ll n = (sqrt(8 * m + 1) - 1) / 2; //解方程求出n的个数
        for(ll i = 0; i < m; ++ i) //求出第一小和第二小的数存放在min1,min2
        {
            scanf("%lld", &num);
            if(num < min1)
            {
                min1 = min1;
                min1 = num;
            }
            else if(num < min2)
            {
                min2 = num;
            }
            M[num] ++;
        }
        M[min1] --;
        M[min2] --;
        a[0] = min1;
        a[1] = min2;
        ll ans = 1;
        while(ans != n - 1)
        {
            for(ll i = 0; i < ans; ++ i)
            {
                M[a[i] + a[ans]] --;
            }
            map<ll, ll>::iterator it = M.begin();
            while(it !=  M.end())
            {
                if(it -> second == ll(0))
                {
                    M.erase(it ++);
                }
                else
                {
                    break;
                }
            }
            a[++ ans] = M.begin() -> first;
            M[a[ans]] --;
        }
        printf("%lld\n", n);
        printf("%lld", a[0]);
        for(int i = 1; i < n; ++ i)
            printf("% lld", a[i]);
        printf("\n");
    }
    return 0;
}

```
