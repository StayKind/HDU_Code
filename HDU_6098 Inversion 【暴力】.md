HDU_6098 Inversion 【暴力】

<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6098)

### 题目描述 ###
首先先普及一下'a|b'代表a是b的因子，即b%a=0，'a∤b'也就是b%a!=0
给定一个序列，求i从2开始不是i倍数的序列中的最大值，依次输出
### 解题思路 ###
暴力大法好，首先用一个结构体数组保存值与下标。然后对值进行从大到小的排序，然后依此对每个下标i找出对i取余不为零的输出。
### 代码部分 ###
```cpp
#include<bits/stdc++.h>
typedef long long ll;
using namespace std;
const int maxn = 1e5+10;
struct node
{
    ll m;
    int x;
}a[maxn];
inline bool cmp(node a, node b)
{
    return a.m > b.m;
}
int main()
{
    int t;
    scanf("%d", &t);
    while(t--)
    {
        int n;
        ll maxn,mm;
        scanf("%d", &n);
        for(int i = 1; i <= n; i++) scanf("%lld", &mm),a[i].x = i,a[i].m = mm;
        maxn = 0;
        for(int i = 1; i <= n; i+=2)
        {
            maxn = max(maxn, a[i].m);
        }
        printf("%lld", maxn);
        sort(a + 1,a + n + 1, cmp);
        for(int i = 3; i <= n; i++)
        {
            for(int j = 1; j <= n; j++)
            {
                if(a[j].x % i != 0) {printf(" %lld",a[j].m);break;}
            }
        }
        printf("\n");
    }
    return 0;
}
```