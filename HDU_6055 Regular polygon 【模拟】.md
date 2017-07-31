<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6055)

### 题目意思 ###
二维平面给出n点，求可以构成多少正多边形。
### 解题思路 ###
由于要求正多边形，可以总结出，正多边形只能是正方形，这道题目就变成了给n个点求正方形有多少个。
这里就需要考虑怎么判断正方形是否存在。
我们将所有的点进行排序，然后所有的点两两进行组合，然后通过公式求出第三个和第四个点，判断是否在点的集合里面。判断是否在点的集合里面我这里是使用了二分查找的操作，当然也可以用一个标记数组，效率会更高一点。
### 代码部分 ###

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn=510;
struct Node
{
    int x,y;
    bool operator<(const Node &rhs)const
    {
        if(x == rhs.x)
            return y < rhs.y;
        else
            return x < rhs.x;
    }
} nodes[maxn];

int main()
{
    int n;
    while(~scanf("%d",&n))
    {
        int ans=0;//正方形个数
        for(int i=0; i<n; ++i)
            scanf("%d%d",&nodes[i].x,&nodes[i].y);
        sort(nodes,nodes+n);
        for(int i=0; i<n; ++i)
            for(int j=i+1; j<n; ++j)
            {
                Node tmp;//tmp作为正方形的第3或4个点
                ///特殊方法来判断第三个和第四个点的位置
                tmp.x=nodes[i].x+nodes[i].y-nodes[j].y;
                tmp.y=nodes[i].y+nodes[j].x-nodes[i].x;
                if(!binary_search(nodes,nodes+n,tmp)) continue;
                tmp.x=nodes[j].x+nodes[i].y-nodes[j].y;
                tmp.y=nodes[j].y+nodes[j].x-nodes[i].x;
                if(!binary_search(nodes,nodes+n,tmp)) continue;

                ++ans;
            }
        ///在两个点为一个边的时候判断了两次，所以结果应该除二
        printf("%d\n",ans/2);
    }
    return 0;
}

```
