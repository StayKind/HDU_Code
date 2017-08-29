
<!--more-->
> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=1305)

### 题目描述 ###
不断输入字符串直到字符为"9"结束，如果在"9"之前，一个字符的代码不是另一个字符的代码的前缀，则这种情况为立即可解就输出"is immediately decodable"否则输出"is not immediately decodable"
### 解题思路 ###
字典树求前缀是否出现过，每插入一个字符就对字符进行查找。
插入时给字符最后的节点加1表示该节点存在。查找的过程：如果在向下扩展的过程中发现节点的cnt有值，就说明该字符已经存在，即为当前查找的字符的前缀，就让标记为假然后按格式输出就可以了。
### 代码部分 ###
```cpp
#include <bits/stdc++.h>
using namespace std;
const int en = 10;
const int maxn = 1e6;
bool flag;
struct Trie
{
    int cnt;
    Trie *nex[en];
} T[maxn];
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
        p = p -> nex[temp];
    }
    p -> cnt ++;
}
int Search(Trie *root, string s)
{
    Trie *p = root;
    for(int i = 0; i < s.size(); ++ i)
    {
        int temp = idx(s[i]);
        if(p -> nex[temp] -> cnt && i != s.size() - 1) flag = false;
        p = p -> nex[temp];
    }
}
int main()
{
    ios::sync_with_stdio(false);
    int cas = 0;
    Trie *root = Init();
    flag = true;
    string s;
    while(cin >> s)
    {
        if(s != "9")
        {
            Insert(root, s);
            Search(root, s);
        }
        else
        {
            if(flag)
                cout << "Set " << ++ cas <<  " is immediately decodable" << endl;
            else
                cout << "Set " << ++ cas <<  " is not immediately decodable" << endl;
            Trie *root = Init();
            flag = true;
        }
    }
    return 0;
}

```
