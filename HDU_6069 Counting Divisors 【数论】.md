
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6069)

### 题目描述 ###

![](http://115.159.67.147/wp-content/uploads/2017/08/d.png)

简单来说求区间因数个数和。
### 解题思路 ###
首先我们先要知道数论之中的一个很重要的知识点。
**数的因子个数**
 `n = p1^a1 * p2^a2 * p3^a3 …… *pn^an`
其中p1,p2,p3……均为素数。n的因数个数为`(a1+1)*(a2+1)*(a2+1)*(a3+1)……*(an+1)`
如果n加一个次方  n^k的因子个数为`(a1*k+1)*(a2*k+1)*(a2*k+1)*(a3*k+1)……*(an*k+1)`
然后这道题就是让算一个区间的K次方的因数个数之和，首先先将`1e6`之内的素数打表，然后再然后枚举这些素数，在枚举 [L,R] 区间中这些素数的倍数，然后根据这些倍数进行素因子分解，其实就是统计 [L,R] 区间中有多少个枚举的素数，然后乘以 K， 在加 1 计算即可。在枚举 [L,R] 区间值的时候，我们需要先把 [L,R] 区间中的数用数组保存下来，然后通过数组进行素因子分解。 
### 代码部分 ###

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
const LL MOD = 998244353;
const LL MAXN = 1e6+5;
int prime[MAXN], cnt;//cnt代表1e6中素数的个数
LL p[MAXN];//存放素数从0开始
///筛法求素数
void isprime()
{
    memset(prime, 0, sizeof(prime));
    cnt = 0;
    prime[1] = 1;
    for(LL i=2; i<MAXN; i++)
    {
        if(!prime[i])
        {
            p[cnt++] = i;
            for(LL j=i*i; j<MAXN; j+=i)
                prime[j] = 1;
        }
    }
}

LL f[MAXN], num[MAXN];///f[i]用来存储L~R之间的数,在不断的对因数相除。累积数量
int main()
{
    isprime();
    int T;
    scanf("%d", &T);
    while(T--)
    {
        LL L, R, K;
        scanf("%lld%lld%lld", &L, &R, &K);
        LL ans = 0;
        if(L == 1) ans = 1, L++;
        int t = R-L;
        for(int i=0; i<=t; i++) f[i] = i+L, num[i] = 1;//初始化

        for(int i=0; i<cnt&&p[i]*p[i]<=R; i++)
        {
            LL tmp = L;
            if(L % p[i]) tmp = (L/p[i]+1)*p[i];//找到第一个可以整除素数p[i]的数，不进行这步会超时
            //cout << "tmp = " << tmp << endl;
            for(LL j=tmp; j<=R; j+=p[i])//素因子分解
            {
                LL cnt = 0;
                while(f[j-L]%p[i] == 0)
                {
                    cnt++;
                    f[j-L] /= p[i];
                }
                num[j-L] = num[j-L]*(cnt*K+1)%MOD;
            }
        }


        for(int i=0; i<=t; i++)//计算结果
        {
            //cout << "num = " << num[i] << "   f = " << f[i] << endl;
            if(f[i] == 1) ans = (ans + num[i]) % MOD;
            else ans = (ans + num[i]*(K + 1)) % MOD;
        }
        printf("%lld\n", ans);
    }
    return 0;
}

```
