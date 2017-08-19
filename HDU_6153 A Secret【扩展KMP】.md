HDU_6153 A Secret【扩展KMP】
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6153)


### 题目描述 ###
给定两个串，求其中一个串s的每个后缀在另一个串t中出现的次数乘以其长度之和。
### 解题思路 ###
扩展KMP模板题，将s和t串都逆序以后就变成了求前缀的问题了，扩展KMP求处从i位置开始的最长公共前缀存于数组，最后通过将数组的值不为0的进行一个等差数列和的和就可以了。
### 代码部分 ###
```cpp
#include <iostream>
#include <string>
#include <string.h>
#include <cstring>
#include <algorithm>
using namespace std;
const int maxn = 1e6 + 10;
const int mod = 1e9 + 7;
typedef long long ll;
int cnt[maxn];
/*
扩展KMP
Next[i]: P[i..m-1] 与 P[0..m-1]的最长公共前缀
ex[i]: T[i..n-1] 与 P[0..m-1]的最长公共前缀
*/
inline ll Add(ll n)
{
    ll m=((n%mod)*((n+1)%mod)/2)%mod;
    return m;
}

char T[maxn],P[maxn];
int Next[maxn],ex[maxn];

void pre_exkmp(char P[])
{
    int m=strlen(P);
    Next[0]=m;
    int j=0,k=1;
    while(j+1<m&&P[j]==P[j+1]) j++;
    Next[1]=j;
    for(int i=2; i<m; i++)
    {
        int p=Next[k]+k-1;
        int L=Next[i-k];
        if(i+L<p+1) Next[i]=L;
        else
        {
            j=max(0,p-i+1);
            while(i+j<m&&P[i+j]==P[j]) j++;
            Next[i]=j;
            k=i;
        }
    }
}

void exkmp(char P[],char T[])
{
    int m=strlen(P),n=strlen(T);
    pre_exkmp(P);
    int j=0,k=0;
    while(j<n&&j<m&&P[j]==T[j]) j++;
    ex[0]=j;
    for(int i=1; i<n; i++)
    {
        int p=ex[k]+k-1;
        int L=Next[i-k];
        if(i+L<p+1) ex[i]=L;
        else
        {
            j=max(0,p-i+1);
            while(i+j<n&&j<m&&T[i+j]==P[j]) j++;
            ex[i]=j;
            k=i;
        }
    }
}

int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin >> t;
    while(t --)
    {
        cin >> T >> P;
        int lenT = strlen(T);
        int lenP = strlen(P);
        reverse(T, T + lenT);
        reverse(P, P + lenP);
        pre_exkmp(P);
        memset(Next, 0,sizeof(Next));
        memset(ex, 0, sizeof(ex));
        exkmp(P, T);
        ll ans = 0;
        for(int i = 0; i < lenT; ++ i)
        {
            if(ex[i])
                ans = (ans + Add(ex[i]) % mod) % mod;
        }
        cout << ans % mod << endl;
    }
    return 0;
}
```