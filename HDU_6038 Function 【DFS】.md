### 1006. Function ###

>[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6038)

#### 题目意思： ####
给两个序列a，b；问其满足关系式`f(i) = b[f(a[i])]`;有多少种。

#### 解题思路： ####
若想满足关系式，则a与a[i]需要成环，b与b[i]需要成环，并且a与b需要同时成环时，又因为a环包着b环，则当b环长度为a环长度的约数的时候就可以满足关系式。
其中a环和b环是不会相交的。
若a的其中一个循环节长度为l，那么b循环节肯定为l的因数，把满足条件的b环将其长度与数量相乘再与a循环节个数相乘。答案累加就可以了。
#### 难点： ####
1. 为什么a与a[i]是环，b与b[i]是环。
2. 怎么去求a环和b环

- 由关系式可以看出当a有这样的关系的时候才会满足关系式。`i->a[i]->a[a[i]]->……i`；将关系式`f(i) = b[f(a[i])]`往下扩展一下`f(i) = b[f(a[i])] = b[b[f(a[i])]]`，由此可以看出b与b[i]有同样的关系。
- 通过一个标记数组和递归进行求，从上面环的解释可以看出，a环与b环的求解过程是一样的。

#### 代码部分： ####

```cpp
#include <bits/stdc++.h>
using namespace std;
#define ll long long
#define M 1000000007  ///取模

const int maxn = 100100;
int n, m, cal[2][maxn];///存放长度为l时a环的个数和b环的个数
int a[maxn], b[maxn], ans = 1;///存放a，b数组的值
bool vis[maxn];///标记数组，判断是否在其他的环中

///递归求环长为l时的个数
void dfs(int t, int l, int *a, int k)
{
    if(vis[t])
    {
        cal[k][l]++;
        return;
    }
    vis[t] = 1;
    dfs(a[t], l + 1, a, k);
}

int main()
{
    int ca = 0;
    while(~scanf("%d%d", &n, &m))
    {
        for(int i = 0; i < n; i++)
            scanf("%d", a + i);
        for(int i = 0; i < m; i++)
            scanf("%d", b + i);
        memset(cal, 0, sizeof(cal));
        ///递归求存在的b环的个数
        memset(vis, 0, sizeof(vis));
        for(int i = 0; i < m; i++)
            if (!vis[i])
                dfs(i, 0, b, 0);
        ///递归求存在的a环的个数
        memset(vis, 0, sizeof(vis));
        for(int i = 0; i < n; i++)
            if (!vis[i])
                dfs(i, 0, a, 1);
        ///计算a，b互成环数量
        ///已知b环为a环的约数
        ///将所有的a环都乘以环长度的约束的所有的b环得出答案
        ans = 1;
        for(int i = 1; i <= n; i++)
            if(cal[1][i])  //如果a环在长度为i的时候存在环
            {
                int lim = (int)sqrt(i + 0.5), ta = 0;//ta 用来累加当a环存在的时候b环的总数
                for(int j = 1; j <= lim; j++)
                    if(i % j == 0) //如果是a环长度的约数
                    {
                        (ta += (ll)cal[0][j] % M * j % M) %= M;
                        if (j * j != i)
                            (ta += (ll)cal[0][i / j] % M * (i / j) % M) %= M;
                    }
                for(int j = 1; j <= cal[1][i]; j++)
                    ans = (ll)ans * ta % M;
            }
        printf("Case #%d: %d\n", ++ca, ans);
    }
    return 0;
}



```

