HDU_2846 Repository 【字典树】
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=2846)

### 题目描述 ###
给定n个字符串然后再给出m个查询，问每次查询的字符串s是那n个字符串子串的个数。注意如果查询的为ab,存在一个字符串abab，那么在这个abab的字符串中，只能计数1而不是2，也就是说一个字符串只能计数1次。
### 解题思路 ###
Trie能很快的求出字典中以某个串为前缀的串的个数    但现在要查的是以某个串为子串的串的个数  可以发现  
一个串的任何子串肯定是这个串某个后缀的前缀  如"ri"是“Trie" 的子串  是后缀 "rie" 的前缀  
那么我们在向Trie中插入时可以把这个串的所有后缀都插入 插入时要注意来自同一个串的后缀的相同前缀只能统计一次  如 "abab" 这个串  "ab" 既是后缀 "abab" 的前缀  也是后缀 "ab" 的前缀  但是只能统计一次  这用一个id数组标记就行了  
这样最后Trie中以查询串为前缀的串的个数就是原始串中以查询串为子串的串的的个数了  
### 代码部分 ###
```cpp
#include <bits/stdc++.h>
using namespace std;

const int maxn = 5e5 + 10;
int nex[maxn][26];
int cnt[maxn], id[maxn];
int top, root, ans;
inline int idx(char c)
{
    return c - 'a';
}
inline void Init()
{
    top = 0;
    memset(nex, 0, sizeof(nex));
    memset(cnt, 0, sizeof(cnt));
    memset(id, 0, sizeof(id));
    root = top ++;
}
void Insert(string s, int x)
{
    int len = s.size();
    int now = root;
    for(int i = 0; i < len; ++ i)
    {
        int temp = idx(s[i]);
        if(!nex[now][temp]) nex[now][temp] = top ++;
        now = nex[now][temp];
        if(id[now] != x) cnt[now] ++;
        id[now] = x;
    }
}
int Search(string s)
{
    int now = root;
    int len = s.size();
    for(int i = 0; i < len; ++ i)
    {
        int temp = idx(s[i]);
        if(!nex[now][temp]) return 0;
        now = nex[now][temp];
    }
    return cnt[now];
}
int main()
{
    ios::sync_with_stdio(false);
    int n; string s;
    cin >> n;
    Init();
    for(int i = 1; i <= n; ++ i)
    {
        cin >> s;
        for(int j = 0; j < s.size(); ++ j)
        {
            string c = s;
            c.erase(c.begin(), c.begin() + j);
            Insert(c, i);
        }
    }
    cin >> n;
    while(n --)
    {
        cin >> s;
        cout << Search(s) << endl;
    }
    return 0;
}
```