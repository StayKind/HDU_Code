HDU_6078 Wavel Sequence 【dp】
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6078)

### 题目描述 ###
阅读理解般的题目。。。给定两个序列a，b，a序列需要满足波浪形的序列，即`a1<a2>a3<a4>a5<a6`b序列需要与其对应。问这样的对应关系有多少种。
### 解题思路 ###
首先先知道，一个数作为波峰，前面肯定会有数比他小，如果一个数要做波谷，那么他有可能是他自己作为波谷，还有可能是他前面有比他这个数大的数作为波峰。我们要做的就是不断地对b序列处理，累积b序列中，每个数做波峰波谷各有多少种可能性。并将其保存在sum数组之中，当需要嫁接波峰波谷的时候就可以直接拿过来使用。
可以按顺序加入a数组的数，然后来计算b序列做波峰波谷的情况分别是多少。当a数组的数ai大于b数组的数bj，那值为ai的波峰的情况就需要加上bj作为波谷情况的值。相反如果ai<bj,那值为ai的波谷的情况就需要加上bj作为波峰情况的值。
### 代码部分 ###

```cpp
#include<bits/stdc++.h>
#define CLR(A, X) memset(A, X, sizeof(A))
using namespace std;

typedef long long LL;

const LL MOD = 998244353;
const int N = 2e3+5;

int b[N], a[N];
LL sum[N][2], dp[N][2];

int main()
{

    int T, n, m;
    scanf("%d", &T);
    while(T--)
    {
        CLR(sum, 0);
        CLR(dp, 0);
        scanf("%d%d", &n, &m);
        for(int i = 1; i <= n; i++) scanf("%d", &a[i]);
        for(int i = 1; i <= m; i++) scanf("%d", &b[i]);
        LL ans = 0;
        for(int i = 1; i <= n; i++)
        {
            LL cnt1 = 1, cnt0 = 0;
            for(int j = 1; j <= m; j++)
            {
                dp[j][0] = dp[j][1] = 0;
                if(a[i] == b[j])
                {
                    dp[j][0] = cnt1;
                    dp[j][1] = cnt0;
                    ans = (ans+cnt1+cnt0)%MOD;
                }
                else if(b[j] < a[i]) (cnt0 += sum[j][0]) %= MOD;
                else (cnt1 += sum[j][1]) %= MOD;
            }
            for(int j = 1; j <= m; j++)
            {
                if(a[i] == b[j])
                {
                    (sum[j][0] += dp[j][0]) %= MOD;
                    (sum[j][1] += dp[j][1]) %= MOD;
                }
            }
        }
        printf("%lld\n", ans);
    }
    return 0;
}


```