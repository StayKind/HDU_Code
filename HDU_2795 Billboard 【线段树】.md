HDU_2795 Billboard 【线段树】
<!--more-->

>[原题链接](http://acm.hdu.edu.cn/showproblem.php?pid=2795)

### 题目意思： ###
给定一个公告板，让你贴公告，公告是尽量往左上角贴， 如果可以贴，输出行号，如果不能贴，输出-1.
### 解题思路: ###
以公告板的高度作为构建线段树的标准，另外用一个数组来存储线段树每个节点的宽度。贴公告的时候从左子树往右子树来贴，这样可以保证是从上到下来贴。具体线段树的代码看看下面的代码部分吧。
### 代码部分： ###

```cpp
#include <iostream>
#include <stdio.h>
#define lchild left,mid,root<<1
#define rchild mid+1,right,root<<1|1
using namespace std;

const int maxn = 1e8;
int Max[maxn<<2];
int height,width,n;
///构建线段树
void build(int left,int right,int root)//让所有子树的根的最大宽度都为width
{
    Max[root] = width;
    if(left == right)
        return ;
    int mid = (left+right)>>1;
    build(lchild);
    build(rchild);
}
///更新线段树
void push_up(int root)
{
    Max[root] = max(Max[root<<1],Max[root<<1|1]);//找出左右子树最大的宽度付给父节点
}
///查询并插入
int query(int w,int left,int right,int root)
{
    if(left == right)//将该广告插入当前的节点并保存当前节点的最大宽度
    {
        Max[root] -= w;
        return left;//返回行号
    }
    int mid = (left+right)>>1;
    int ans = 0;
    if(w <= Max[root<<1])//如果左子树中有大于w的边，就将其插入左子树
        ans = query(w,lchild);
    else
        ans = query(w,rchild);
    push_up(root);//进行更新操作
    return ans;
}

int main()
{
    while(~scanf("%d%d%d",&height,&width,&n)) //换做cin会超时
    {
        if(n<height)
            height = n;
        build(1,height,1);//构建线段树
        int w;
        while(n--)
        {
            scanf("%d",&w);
            if(Max[1]<w)//如果根的最大的宽度都没有w那么长，就直接输出-1
                printf("-1\n");
            else//进行查找并输出ans
            {
                int ans = query(w,1,height,1);
                printf("%d\n",ans);
            }
        }
    }
    return 0;
}

```