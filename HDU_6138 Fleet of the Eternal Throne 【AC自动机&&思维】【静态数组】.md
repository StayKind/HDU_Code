HDU_6138 Fleet of the Eternal Throne 【AC自动机&&思维】【静态数组】
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6138)

### 题目描述 ###
给定n个字符，m个查询，每个查询包括两个数字，要求从下标为这两个字符串中找到一个最长公共子串，并且这个子串为这n个字符的前缀。
### 解题思路 ###
AC自动机模板稍作修改。写这种题目各种绝望，链表写着写着发现不行，然后就创建静态链表，接着又是一阵GG，最后还是数组写的比较好，该题解写完命已呜呼。
步入正题，我们先根据给定的n个字符创建线段树，节点之中保存这该前缀的长度。然后就是常规的建立fail指针。
然后我们将要查询的第一个字符先跑一遍AC自动机，并在中留下标记，表明这是第一个字符串的子串并且为这n个字符的前缀，紧接着跑第二个字符串，如果遍历到某个节点，这个节点已经被第一个标记过了，那么就表明该节点就是这两个串的公共子串并且是这n个字符的某个前缀，现在的目标就是应该不断刷新找出最大值。
### 代码部分 ###
```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 10;
const int en = 26;
int nex[maxn][en];//用来构建字典树，表明下个节点
int cnt[maxn]; // 用来存放该前缀后缀经过的次数。
bool vis[maxn][2];//用来做标记
int fail[maxn];//存放失败指针
int id, root, ans;
string A[maxn];
inline int idx(char c) {return c - 'a';}
inline int CreatTrie()
{
    for(int i = 0; i < en; ++ i)
        nex[id][i] = -1;
    fail[id] = -1;
    cnt[id] = 0;
    vis[id][0] = vis[id][1] = false;
    return id ++;
}
inline void Init()
{
    id = 0;
    root = CreatTrie();
}
void Insert(string c)
{
    int len = c.size();
    int p = root;
    for(int i = 0; i < len; ++ i)
    {
        int t = idx(c[i]);
        if(nex[p][t] == -1) nex[p][t] = CreatTrie();
        cnt[nex[p][t]] = cnt[p] + 1;
        p = nex[p][t];
    }
}
void Bfail()
{
    int p = root;
    queue <int> que;
    que.push(p);
    while(!que.empty())
    {
        p = que.front();
        que.pop();
        for(int i = 0; i < en; ++ i)
        {
            if(nex[p][i] != -1)
            {
                if(p == 0) fail[nex[p][i]] = 0;
                else
                {
                    int q = fail[p];
                    while(q != -1)
                    {
                        if(nex[q][i] != -1)
                        {
                            fail[nex[p][i]] = nex[q][i];
                            break;
                        }
                        q = fail[q];
                    }
                    if(q == -1) fail[nex[p][i]] = 0;
                }
                que.push(nex[p][i]);
            }
        }
    }
}
void ACauto(string c, int cas)
{
    ans = 0;
    int p = root;
    int len = c.size();
    for(int i = 0; i < len; ++ i)
    {
        int t = idx(c[i]);
        while(nex[p][t] == -1 && p != 0) p = fail[p];
        p = nex[p][t];
        if(p == -1) p = 0;
        int q = p;
        while(q != 0 && vis[q][cas] == false)
        {
            vis[q][cas] = true;
            if(cas == 1)
            {
                if(vis[q][0] == true && vis[q][1] == true) ans = max(ans, cnt[q]);
            }
            q = fail[q];
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
        Init();
        int n, m, t1, t2;
        cin >> n;
        for(int i = 1; i <= n; ++ i)
        {
            cin >> A[i];
            Insert(A[i]);
        }
        for(int i = 0; i < id; ++ i)
            cout << cnt[i] << endl;
        Bfail();
        for(int i = 0; i < id; ++ i)
        {
            cout << cnt[i] << endl;
            cout << fail[i] << endl;
        }
        cin >> m;
        for(int i = 1; i <= m; ++ i)
        {
            memset(vis, false, sizeof(vis));
            cin >> t1 >> t2;
            ACauto(A[t1], 0);
            ACauto(A[t2], 1);
            cout << ans << endl;
        }
    }
    return 0;
}

```