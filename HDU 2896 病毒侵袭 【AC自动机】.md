
[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=2896)

### 题目意思 ###

给出n个病毒和n个网站，找出每一个网站中含有的病毒种类，并按病毒编号升序输出，最后统计含有病毒的网站个数。

### 解题思路 ###

这是一个AC自动机的模板题目。
这个题目给的字符串中的字符是除回车的任意字符，所以每个节点儿子是有128个的。

我们每个字典树的节点要维护的值有：
从root到该节点是不是一个完整的单词。
如果是完整的单词，它所代表单词的编号。
fail指针。
由于我们的AC自动机要多个文本串去跑，所以我们再加一个vis标记以下那些文本串
已经上去匹配过了。

### 代码部分 ###

```
#include <iostream>  
#include <stdio.h>  
#include <string.h>  
#include <queue>  
#include <algorithm>  
  
using namespace std;  
  
const int allSon = 130;    ///包含所有的字符  
const int maxn = 100005;  
int node[maxn][allSon];    ///字典树节点  
int fail[maxn];            ///fail指针  
int num[maxn];             ///num[i]代表以第i个节点为结尾的单词个数  
int vis[maxn];             ///标记  
int index[maxn];           ///用来存放模式串的编号  
char patten[maxn];         ///模式串  
char text[maxn];           ///文本串  
int ans[maxn];  
int id;                    ///用来给节点编号  
int total;                 ///统计总共有多少个网站感染病毒  
///将模式串插入到字典树,传入的模式串的编号  
void insertPatten(int identifier)  
{  
    int p = 0;      ///指向根节点  
    int pos = 0;    ///下标  
    int len = strlen(patten);  
    while(pos < len)  
    {  
        int ch = patten[pos];  
        if(node[p][ch]==-1)     ///节点不存在  
        {  
            memset(node[id],-1,sizeof(node[id]));  ///孩子节点都置空  
            num[id] = 0;  
            fail[id] = -1;  
            vis[id] = -1;  
            node[p][ch] = id++;  
        }  
        p = node[p][ch];  
        pos++;  
    }  
    num[p]++;  
    index[p] = identifier;      ///存储编号  
}  
///找fail指针，构造AC自动机  
void build_AC_automaton()  
{  
    queue<int>qu;  
    int p = 0;          ///最开始指向根节点  
    qu.push(p);  
    while(!qu.empty())  
    {  
        p = qu.front();  
        qu.pop();  
        for(int i = 0; i < allSon; i++)  
        {  
            if(node[p][i]!=-1)  ///第i个孩子存在  
            {  
                if(p == 0)  ///如果p是根，根的孩子fail指向根  
                {  
                    fail[node[p][i]] = 0;  
                }  
                else  
                {  
                    int temp = fail[p];  
                    while(temp != -1)  
                    {  
                        if(node[temp][i] != -1)  
                        {  
                            fail[node[p][i]] = node[temp][i];  
                            break;  
                        }  
                        temp = fail[temp];  
                    }  
                    if(temp == -1)  
                    {  
                        fail[node[p][i]] = 0;  
                    }  
                }  
                qu.push(node[p][i]);  
            }  
        }  
    }  
}  
///在AC自动机中进行查询，传入参数是第i个网站的编号。  
void find_in_AC_automaton(int identify)  
{  
    int p = 0;  
    int pos = 0;  
    int t = 0;  
    while(text[pos]!='\0')  
    {  
        int ch = text[pos];  
        while(node[p][ch]==-1 && p!=0)  
            p = fail[p];  
        p = node[p][ch];  
        if(p == -1) p = 0;  
        int temp = p;  
        /**由于别的字符串还要过来匹配，所以节点的num  
        值不能进行改变，我们可以用vis数组来实现标记，避免  
        重复统计**/  
        while(temp!=0 && vis[temp]!=identify)  
        {  
            vis[temp] = identify;  
            if(num[temp])   ///代表网站有一个病毒  
            {  
                ans[t++] = index[temp];  
            }  
            temp = fail[temp];  
        }  
        ///由于一个网站病毒最多三个，所以找够三个就出来，不要浪费时间做无用功。  
        if(t >= 3)  
            break;  
        pos++;  
    }  
    if(t != 0)  
    {  
        total++;  
        printf("web %d:",identify);  
        sort(ans,ans+t);  
        for(int i = 0; i < t; i++)  
            printf(" %d",ans[i]);  
        printf("\n");  
    }  
}  
void init()  
{  
     memset(node[0],-1,sizeof(node[0]));  
     num[0] = 0;  
     fail[0] = -1;  
     index[0] = -1;  
     vis[0] = -1;  
     id = 1;  
     total = 0;  
}  
int main()  
{  
    int N,M;  
    while(~scanf("%d",&N))  
    {  
        init();   ///初始化根节点  
        for(int i = 1; i <= N; i++)  
        {  
            scanf("%s",patten);  
            insertPatten(i);  
        }  
        build_AC_automaton();  
        scanf("%d",&M);  
        for(int i = 1; i <= M; i++)  
        {  
            scanf("%s",text);  
            find_in_AC_automaton(i);  
        }  
        printf("total: %d\n",total);  
    }  
    return 0;  
}  

```