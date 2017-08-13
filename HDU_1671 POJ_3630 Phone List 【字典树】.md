HDU_1671 POJ_3630 Phone List 【字典树】
<!--more-->

> [题目链接](http://poj.org/problem?id=3630)

### 题目描述 ###
给定一个电话号码列表，确定它是否一致，意思是没有数字是另一个的前缀。如果有输出NO，否则输出YES
### 解题思路 ###
字典树求前缀问题，但这里要注意，**可能一个电话号码是这个字符之前的前缀也可能是这个字符之后的前缀。**在插入字符的同时来判断该字符是否可以构成前缀即可。
HDU这道题如果动态分配字典树不清除会内存超限。在POJ中动态分配内存时间还会超时。所以需要静态分配内存。从中知道了**动态分配内存会省内存但是时间限制会增加。静态分配内存会更耗内存。**
### 代码部分 ###
#### 静态字典树
```cpp
#include <iostream>
#include <sstream>
#include <stdlib.h>
#include <stdio.h>
using namespace std;
const int en = 10;
const int maxn = 1e6;
bool flag;
struct Trie
{
    int cnt;
    Trie *nex[en];
}T[maxn];
int top;
inline int idx(char c)
{
    return c - '0';
}
inline Trie * CreatTrie()
{
    Trie *p = &T[top ++];
    p -> cnt = 0;
    for(int i = 0; i< en; ++ i) p -> nex[i] = NULL;
    return p;
}
inline Trie * Init()
{
    top = 0;
    return CreatTrie();
}
void Insert(Trie *root, string s)
{
    Trie *p = root;
    for(int i = 0; i < s.size(); ++ i)
    {
        int temp = idx(s[i]);
        if(p -> nex[temp] == NULL) p -> nex[temp] = CreatTrie();
        else if(p -> nex[temp] -> cnt || i == s.size() - 1)flag = false;
        p = p -> nex[temp];
    }
    p -> cnt ++;
}

int main()
{
    ios::sync_with_stdio(false);
    string s;
    int t, m;
    cin >> t;
    while(t --)
    {
        flag = true;
        Trie *root = Init();
        cin >> m;
        while(m --)
        {
            cin >> s;
            Insert(root, s);
        }
        puts(flag ? "YES" : "NO");
    }

    return 0;
}

```

---

### 动态字典树
```cpp
#include <iostream>
#include <sstream>
#include <stdlib.h>
#include <stdio.h>
using namespace std;
const int en = 10;
bool flag;
struct Trie
{
    int cnt;
    Trie *nex[en];
};
int top;
inline int idx(char c)
{
    return c - '0';
}
inline Trie * CreatTrie()
{
    Trie *p = (Trie *)(malloc)(sizeof(Trie));
    p -> cnt = 0;
    for(int i = 0; i< en; ++ i) p -> nex[i] = NULL;
    return p;
}
inline Trie * Init()
{
    top = 0;
    return CreatTrie();
}
void Insert(Trie *root, string s)
{
    Trie *p = root;
    for(int i = 0; i < s.size(); ++ i)
    {
        int temp = idx(s[i]);
        if(p -> nex[temp] == NULL) p -> nex[temp] = CreatTrie();
        else if(p -> nex[temp] -> cnt || i == s.size() - 1)flag = false;
        p = p -> nex[temp];
    }
    p -> cnt ++;
}
void Free(Trie *root)
{
    for(int i = 0; i < en; ++ i)
    {
        if(root -> nex[i])
            Free(root -> nex[i]);
    }
    free(root);
}
int main()
{
    ios::sync_with_stdio(false);
    string s;
    int t, m;
    cin >> t;
    while(t --)
    {
        flag = true;
        Trie *root = Init();
        cin >> m;
        while(m --)
        {
            cin >> s;
            Insert(root, s);
        }
        puts(flag ? "YES" : "NO");
        Free(root);
    }

    return 0;
}

```