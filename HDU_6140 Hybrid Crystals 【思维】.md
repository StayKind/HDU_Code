HDU_6140 Hybrid Crystals 【思维】

<!--more-->
> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6140)

### 题目描述 ###
搞得好像阅读理解，就是给定一个序列。序列的每个值都有属性，N代表可加可减，L代表只能加，D代表只能减，当然这些都可以用或者不用。给定一个k，问k是否可能用这个序列构成。
### 解题思路 ###
比赛的时候写了深搜，写了dp各种超。最后下来看别人题解，就是一开始固定一个左右区间L，R，不断的加数扩大区间范围，当最后k在L~R之内就输出"yes"，否则输出"no"。测试一下真的过了，一脸懵逼，出题的的大哥，请收下小弟的膝盖。。。
### 代码部分 ###
```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e3 +10;
struct node
{
    int n;
    char c;
} N[maxn];
int main()
{
    int t;
    scanf("%d", &t);
    while(t --)
    {
        int m, k;
        int Left, Right;
        Left = Right = 0;
        scanf("%d %d",&m,&k);
        for(int i = 1; i<=m; i++)
        {
            scanf("%d",&N[i].n);
        }
        for(int i = 1; i<=m; i++)
        {
            scanf(" %c",&N[i].c);
        }
        for(int i = 1; i <= m; ++ i)
        {
            if(N[i].c == 'N')Left -= N[i].n, Right += N[i].n;
            else if(N[i].c == 'L') Right += N[i].n;
            else Left -= N[i].n;
        }

        if(k>=0)
        {
            if(k <= Right)
                printf("yes\n");
            else
                printf("no\n");
        }
        else
        {
            if(k >=Left)
                printf("yes\n");
            else
                printf("no\n");
        }
    }
    return 0;
}

```