<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6070)

### 题目描述 ###
已知一个区间的不同题目个数/区间题目个数为这个区间的正确率。给定一个序列，求序列的正确率最小为多少。
### 解题思路 ###
我们求AC率，AC无非就是0~1间的一个数，因此采用二分答案的方法求解。
现在假设答案为mid.则`distinct【left,right】/（right-left+1） <= mid` ,则说明答案是小于mid的。上面的式子可以变成下面的式子。
 `distinct【left,right】 <= mid*(right-left+1)`
 `distinct【left,right】+ mid*left <= mid*(right+1)`
因此确定一个mid值，我们构建一颗空的线段树，每个节点的值先赋值为`mid*left`,其中left为当前节点的左边界，对于线段树每个节点我们存放的是`distinct【left,right】+mid*left`,对于这些节点，我们维护的是节点所代表区间的最小值，因为固定右边界的时候，右侧`mid*(right+1)`是固定的，我们需要让左侧尽量小。

那么需要更新的就是`distinct【left,right】`区间中的值。我们可以通过遍历右边界，数组中的每个数都可以作为右边界，对于新加入线段树的a[i]。pre[a[i]]记录前一次a[i]这个数字出现的位置，因此我们知道在这次遇到a[i]之前，pre[a[i]]+1 ~ i，这些位置都没有出现过a[i]这个数字，所以在线段树中区间pre[a[i]]~i，应该加上1，就是a[i]这个数字。

更新完后，我们就可以查询【1，i】这个区间，原来我比较不理解这里，后来想明白了，1~i，这个区间求解的时候，这个区间包含许多子区间，例如【2,i】,【3,i】,【4,i】~【i,i】。这样的话每次插入第i个数，相当于从这些区间中求了一个`distinct【left,right】+mid*left` 的最小值。在循环过程中，只要有一个值小于`mid*(right+1)`,就说明我们假设的值偏大了，循环完了也没出现这种情况说明假设的值偏小了。

#### 强调两点线段树中的操作： ####

- push_up操作，即更新当前节点的值。我们要维护线段树区间最小值，此函数主要用于建树过程和更新过程，由于建树过程是个递归过程，当建完当前节点的左右子树的时候，当前节点的最小值就是左右子树节点中的最小值。当进行区间更新操作的时候，由于整个区间都加上的一个相同的值，则区间的值就改变了，则其父母的值都需要进行更新，因此更新完左右子树后，就要跟新当前节点的值。

- push_down操作，下推懒惰标记，主要用于更新操作和查询操作，当整个区间加上同一个值的时候，我们知道其子区间也同样需要加上相应的值，懒惰标记就是lazy【】数组，里面存放着某个节点所代表的区间要加上的值。

**注意：**精度问题，二分次数越多，答案越接近正确答案，但也不能过高，过高可能超时，具体二分次数，差不多试一下就可以试出来。

### 代码部分 ###

```cpp
#include <iostream>  
#include <stdio.h>  
#include <string.h>  
#include <algorithm>  
#define eps 1e-6  
#define inf 0x3f3f3f3f  
#define lchild left,mid,root<<1  
#define rchild mid+1,right,root<<1|1  
using namespace std;  
  
const int maxn = 60010;  
int a[maxn];  
double Min[maxn<<2];   ///线段树节点值  
double lazy[maxn<<2];  ///懒惰标记数组  
int pre[maxn];         ///pre[i]用来存放i这个数上一次出现的时候在数组中的位置  
///更新当前节点  
void push_up(int root)  
{  
    Min[root] = min(Min[root<<1],Min[root<<1|1]);  
}  
///懒惰标记下推  
void push_down(int root)  
{  
    if(lazy[root]>eps)   
    {  
        Min[root<<1] += lazy[root];  
        lazy[root<<1] += lazy[root];  
        Min[root<<1|1] += lazy[root];  
        lazy[root<<1|1] += lazy[root];  
        lazy[root] = 0;  
    }  
}  
///构建线段树  
void build(int left,int right,int root,double temp)  
{  
    Min[root] = left*temp;  
    lazy[root] = 0;  
    if(left == right) return;  
    int mid = (left+right)>>1;  
    build(lchild,temp);  
    build(rchild,temp);  
    push_up(root);  
}  
///区间更新，同时加上add  
void update(int L,int R,int add,int left,int right,int root)  
{  
    if(L<=left && right<=R)  
    {  
        Min[root] += add;  
        lazy[root] += add;  
        return;  
    }  
    ///如果父区间都加上了add,则其子区间也要加上add,所以要下推懒惰标记。  
    push_down(root);   
    int mid = (left+right)>>1;  
    ///更新左右子树。  
    if(L<=mid) update(L,R,add,lchild);  
    if(R>mid) update(L,R,add,rchild);  
    push_up(root);  
}  
///区间查询操作  
double query(int L,int R,int left,int right,int root)  
{  
    if(L<=left && right<=R)  
        return Min[root];  
    push_down(root); ///避免要查询的区间有懒惰标记没有下推。  
    double ans = inf*1.0;  
    int mid = (left+right)>>1;  
    if(L<=mid) ans = min(ans,query(L,R,lchild));  
    if(R>mid) ans = min(ans,query(L,R,rchild));  
    return ans;  
}  
///判读二分答案偏大，偏小。  
bool check(int n,double temp)  
{  
    build(1,n,1,temp);  
    memset(pre,0,sizeof(pre));  
    for(int i = 1; i <= n; i++)  
    {  
        update(pre[a[i]]+1,i,1,1,n,1);  
        double minimum = query(1,i,1,n,1);  
        pre[a[i]] = i;  
        if(temp*(i+1)-minimum >= eps)  
            return true;  
    }  
    return false;  
}  
int main()  
{  
    int t,n;  
    scanf("%d",&t);  
    while(t--)  
    {  
        scanf("%d",&n);  
        for(int i = 1; i <= n; i++)  
            scanf("%d",&a[i]);  
        double L,R,mid,ans;  
        L = 0;  
        R = 1;  
        for(int i = 0; i < 30; i++)  
        {  
            mid = (L+R)/2.0;  
            if(check(n,mid)) R=ans=mid;  
            else L=ans=mid;  
        }  
        printf("%.10lf\n",ans);  
    }  
    return 0;  
  
}  
```

> 转自 [**温姑娘的博客**](http://blog.csdn.net/wyxeainn/article/details/76687575)