HDU_3460 Ancient Printer 【字典树】
<!--more-->
> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=3460)

### 题目描述 ###
现有一个打印机，有3个功能：

- 输入'a'~'z'
- 删除一个字母
- 输出打印机中的字符串

问给定n个字符串，要将他们都输出，最少需要多少次操作。
### 解题思路 ###
首先，这道题的输出可以先不用考虑，因为有几个单词需要输出几次。再有就是输入和删除，你将其所有单词插入到字典树当中，树中有多少节点就进行多少次输入和删除。最后因为最长的没有删除，再减去最长的长度即可。
### 代码部分 ###
因为时间限制较少，所以优先考虑静态建字典树。
```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 5e5 + 10;
int nex[maxn][26];
int cnt[maxn];
int top, root, ans, Max;///ans 节点的个数
inline int idx(char c) {return c - 'a';}
inline int Creat()
{
    for(int i = 0; i< 26; ++ i) nex[top][i] = 0;
    cnt[top] = 0;
    return top ++;
}
inline void Init()
{
    top = 0, ans = 0, Max = -1;
    memset(nex, 0, sizeof(nex));
    memset(cnt, 0, sizeof(cnt));
    root = Creat();
}
void Insert(string s)
{
    int len = s.size();
    int now = root;
    for(int i = 0; i < len; ++ i)
    {
        int temp = idx(s[i]);
        if(!nex[now][temp]) nex[now][temp] = Creat();
        now = nex[now][temp];
        cnt[now] ++;
    }
}
void Search(int x)
{
    for(int i = 0; i < 26; ++ i) if(nex[x][i]) {ans ++, Search(nex[x][i]);}
}
int main()
{
    ios::sync_with_stdio(false);
    int n;
    string s;
    while(cin >> n)
    {
        Init();
        for(int i = 1; i <= n; ++ i)
        {
            cin >> s;
            int len = s.size();
            if(len > Max) Max = s.size();
            Insert(s);
        }
        Search(0);
        cout << ans * 2 - Max + n << endl;
    }
    return 0;
}

```