HDU_6063 RXD and math 【整数快速幂】
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6063)

### 题目描述 ###
乱七八糟的莫比乌斯公式，最后迷迷糊糊，答案就是 $$n^k$$
### 解题思路 ###
签到题，数值较大，需要用到整数快速幂，注意大数取模问题
### 代码部分 ###
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long LL;
const int mod = 1e9+7;
LL Integer_Quick_Pow(LL n,LL k)//取模过程需要注意，在每次相乘都要对mod取模
{
    LL ans,res;
    ans = 1;
    res = n;
    while(k)
    {
        if(k&1)
            ans = ((ans%mod)*(res%mod))%mod;
        res = ((res%mod)*(res%mod))%mod;
        k = k>>1;
    }
    return ans;
}
int main()
{
    LL n,k;
    int Case=0;
    while(~scanf("%lld%lld",&n,&k))
    {
        printf("Case #%d: %lld\n",++Case,Integer_Quick_Pow(n,k));
    }
    return 0;
}
```