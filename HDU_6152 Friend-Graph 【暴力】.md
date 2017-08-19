HDU_6152 Friend-Graph 【暴力】

<!--more-->
>[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6152)

### 题目描述 ###
就是一个团队如果存在三个或者三个以上的人互为朋友的关系，或者都不是朋友的关系，那么就说明这个团队是一个坏团队，否则输出好团队。
### 解题思路 ###
暴力大法好，将每个人之间的关系都存于一个w数组之中，注意这里的数组为bool类型，暴力任意三个节点，如果他们之间的关系都为true或者false，则输出 "Bad Team!"，否则输出"Great Team"。与此同时知道了能用bool类型的数组尽量用bool类型，因为bool类型所占内存较小。
### 代码部分 ###
```cpp
#include<iostream>
#include<cstdio>
#include<string.h>
using namespace std;
const int maxn = 3e3;
bool w[maxn][maxn];

bool flag(int n)
{
    for(int i = 1; i <= n; ++ i)
    {
        for(int j = i + 1; j <= n; ++ j)
        {
            for(int k = j + 1; k <= n; ++ k)
            {
                if(w[i][j] == w[i][k] && w[i][k] == w[j][k] && w[i][j] == w[j][k])
                    return true;
            }
        }
    }
    return false;
}
int main()
{
    int t, n, m;
    scanf("%d", &t);
    while(t --)
    {
        scanf("%d", &n);
        for(int i = 1; i < n; ++ i)
        {
            for(int j = 1 + i; j <= n; ++ j)
            {
                scanf("%d", &m);
                if(m == 1)
                    w[j][i] = w[i][j] = false;
                else
                    w[j][i] = w[i][j] = true;
            }
        }

        if(flag(n))
            printf("Bad Team!\n");
        else
            printf("Great Team!\n");
    }
    return 0;
}

```
