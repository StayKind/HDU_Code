HDU_6092 Rikka with Subset 【dp】
<!--more-->

> [原题链接](http://acm.hdu.edu.cn/showproblem.php?pid=6092)

### 题目描述 ###
有一个数列 a[] ,长度（n<=50）。b[i] 表示元素和为i的集合个数。给你一个数列 b[] ，长度（m<=10000），让你求 a[]，并按照其字典序最小输出。
### 解题思路 ###
设想a子集中肯定会有一个集合为空集，所以b[0]，为空集的个数。那么除了空集以外b[i]如果不为零，则i为a集合中最小的值，并想办法将其在b集合中去除。
这里用了dp的思想，从i下表开始之后的动态转移方程 `b[i] -= b[i - w]`  w为最小的不为零的b数组的下标。
### 代码部分 ###
```cpp
#include<bits/stdc++.h>
using namespace std;
const int maxn = 1e4 + 10;
long long b[maxn];
int m, n;
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin >> t;
    while(t--)
    {
        cin >> n >> m;
        for(int i = 0; i <= m ; i++) cin >> b[i];
        while(n--)
        {
            int w;
            for(int i = 1; i <= m; i++) if(b[i]) {w = i, cout << w; break;}
            if(n) cout << " ";
            for(int i = w; i <= m; i++) b[i] -= b[i - w];
        }
        cout << endl;
    }
    return 0;
}

``` 