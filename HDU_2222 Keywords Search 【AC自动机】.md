HDU_2222 Keywords Search 【AC自动机模板题】
<!--more-->
>[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=2222)

### 题目描述 ###
给定n个匹配串，和一个文本串，问有多少个匹配串在字符串中出现过。输出匹配串的个数。
### 解题思路 ###
AC自动机模板题，AC自动机可以通过fail指针轻松的高效的进行字符串匹配，学习完AC自动机以后就可以很容易的解出这道题了。
### 代码部分 ###
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int en = 26;
inline int idx(char c)
{
    return c - 'a';
}
struct Trie
{
    Trie *nex[en];
    Trie *fail;
    int cnt;
}*root;

inline Trie *Creat()
{
    Trie *p = (Trie *)(malloc)(sizeof(Trie));
    p -> cnt = 0;
    p -> fail = NULL;
    for(int i = 0; i < en; ++ i) p -> nex[i] = NULL;
    return p;
}

inline void Init()
{
    root = Creat();
}

void Insert(string s)
{
    Trie *p = root;
    int len = s.size();
    for(int i = 0; i < len; ++ i)
    {
        int t = idx(s[i]);
        if(p -> nex[t] == NULL) p -> nex[t] = Creat();
        p = p -> nex[t];
    }
    p -> cnt ++;
}
void BFail()
{
    Trie *p = root;
    queue <Trie*> Q;
    Q.push(p);
    while(!Q.empty())
    {
        p = Q.front();
        Q.pop();
        for(int i = 0; i < en; ++ i)
        {
            if(p -> nex[i])
            {
                if(p == root)
                {
                    p -> nex[i] -> fail = root;
                }
                else
                {
                    Trie *q = p -> fail;
                    while(q)
                    {
                        if(q -> nex[i])
                        {
                            p -> nex[i] -> fail = q -> nex[i];
                            break;
                        }
                        q = q -> fail;
                    }
                    if(!q) p -> nex[i] -> fail = root;
                }
                Q.push(p -> nex[i]);
            }
        }
    }
}
int ACauto(string s)
{
    int ans = 0;
    Trie *p = root;
    int len = s.size();
    for(int i = 0; i < len; ++ i)
    {
        int t = idx(s[i]);
        while(!p -> nex[t] && p != root) p = p -> fail;
        p = p -> nex[t];
        if(!p) p = root;
        Trie *q = p;
        while(q && q -> cnt != -1)
        {
            ans += q -> cnt;
            q -> cnt = -1;
            q = q -> fail;
        }
    }
    return ans;
}
inline void Free(Trie *p)
{
    if(p) for(int i = 0; i < en; ++ i) Free(p -> nex[i]);
    free(p);
}
int main()
{
    ios::sync_with_stdio(false);
    int t, n;
    string A, B;
    cin >> t;
    while(t --)
    {
        cin >> n;
        Init();
        for(int i = 1; i <= n; ++ i)
        {
            cin >> A;
            Insert(A);
        }
        cin >> B;
        BFail();
        cout << ACauto(B) << endl;
        free(root);
    }
    return 0;
}

```