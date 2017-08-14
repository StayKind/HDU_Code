HDU_6096 String 【字典树】
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6096)

### 题目描述 ###
给定n个字符串，给m个前缀后缀，问这样的前缀后缀在字符串中出现的次数。
### 解题思路 ###
字典树问题，静态数组构建字典树。动态会爆TLE。
**建树步骤：**首先说一下树的数据结构：cnt：表示这样的前缀有多少种。nu数组：存放这样的前后缀有匹配的字符长度，作用就是排除前后缀长度大于字符串长度。接着就是常规的nex二维数组，建树专用。然后再说一下字符串的处理。给定字符串 如cde -> ceddec ,两个指针分别指向字符的首和尾然后一个向前，一个向后延伸。这样建树可以对给定的前缀后缀处理后就可以直接查询。
**对前缀后缀处理：**和建树类似，也是首尾两两结合，如不足补`'*'`如 `ce f -> cfe*`。
这样对字符处理后，就变成普通的查询了。注意如果再查询中遇到了'*',需要对该节点的所有情况都考虑一下。
### 难点解析 ###

- 给处理后的字符可能现字符长度小于前后缀的长度和，这种情况是不符合的。
- 对遇到`*`号的处理。需要把`*`的所有情况的进行考虑。

### 代码部分 ###
```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 5e5 + 10;
int nex[maxn << 1][26];//用来构建字典树，表明下个节点
int cnt[maxn << 1]; // 用来存放该前缀后缀经过的次数。
vector <int> nu[maxn << 1]; //存该前缀后缀的字符长度。
int top, root;
/// 字母转换为数字
inline int idx(char c)
{
    return c - 'a';
}
///建树需要转换的字符串
inline string Idx(string c)
{
    string s = "";
    for(int i = 0, j = c.size() - 1; i < c.size(); ++ i, -- j)
    {
        char c1 = c[i], c2 = c[j];
        s += c1, s += c2;
    }
    return s;
}
///查前后缀需要转换的字符串
inline string IDX(string c1, string c2)
{
    string s = "";
    int len1 = c1.size(), len2 = c2.size();
    reverse(c2.begin(), c2.end());
    int len = max(len1, len2);
    for(int i = 0; i < len; ++ i)
    {
        char c;
        if(i + 1 > len1)
            s += '*';
        else
            c = c1[i], s += c;
        if(i + 1 > len2)
            s += '*';
        else
            c = c2[i], s += c;
    }
    return s;
}
///添加字典树节点
inline int CreatTrie()
{
    for(int i = 0; i < 26; ++ i) nex[top][i] = 0;
    cnt[top] = 0;
    return top ++;
}
///初始化字典树
inline void Init()
{
    top = 0;
    memset(nex, 0, sizeof(nex));
    for(int i = 0; i < 2 * maxn; ++ i) nu[i].clear();
    root = CreatTrie();
}
///插入字符串建树
void Insert(string c)
{
    int len = c.size();
    int now = root;
    for(int i = 0; i < len; ++ i)
    {
        int temp = idx(c[i]);
        if(!nex[now][temp])
        {
            nex[now][temp] = CreatTrie();
        }
        now = nex[now][temp];
        nu[now].push_back(len / 2);
        cnt[now] ++;
    }
}
///对节点所包含的字符长度进行排序
void dfs(int x)
{
    sort(nu[x].begin(),nu[x].end());
    for(int i = 0; i < 26; ++ i)
        if(nex[x][i]) dfs(nex[x][i]);
}
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin >> t;
    while(t --)
    {
        int m, n;
        string s;
        cin >> m >> n;
        Init();
        for(int i = 1; i <= m; ++ i)
        {
            cin >> s;
            Insert(Idx(s));
        }
        dfs(0);
        while(n --)
        {
            string s1, s2;
            cin >> s1 >> s2;
            int ret = s1.size() + s2.size();
            s = IDX(s1, s2);
            queue <int> Q[2];
            int tmp = 0;
            Q[tmp].push(0);
            int ans = 0;
            for(int i = 0; i < s.size(); ++ i)
            {
                tmp = 1-tmp;
                int id = idx(s[i]);
                while(!Q[1-tmp].empty())
                {
                    int now = Q[1-tmp].front();
                    Q[1-tmp].pop();
                    if(s[i] == '*')
                    {
                        for(int j = 0; j < 26; ++ j)
                        {
                            if(nex[now][j])
                            {
                                Q[tmp].push(nex[now][j]);
                            }
                        }
                    }
                    else
                    {
                        if(nex[now][id])
                        {
                            Q[tmp].push(nex[now][id]);
                        }
                    }
                }
            }
            while(!Q[tmp].empty())
            {
                int now = Q[tmp].front();
                Q[tmp].pop();
                int temp = lower_bound(nu[now].begin(),nu[now].end(),ret) - nu[now].begin();
                ans -= temp;
                ans += cnt[now];
            }
            printf("%d\n",ans);
        }
    }
    return 0;
}

```