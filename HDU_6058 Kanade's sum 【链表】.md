HDU_6058 Kanade's sum 【链表】

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6058)

题目意思
给定n，k。n表示一个序列的个数，并且序列的值为1~n。求这个序列所有的子序列第k大值之和，序列顺序不可改变。
解题思路
一开始以为是主席树，也是类似求在不改变序列的条件下，求区间的第k大值，然后就是把所有情况都加一遍。
我们只要求出对于一个数x左边最近的k个比他大的和右边最近k个比他大的，扫一下就可以知道有几个区间的k大值是x。

我们考虑从小到大枚举x，每次维护一个链表，链表里只有>=x的数，那么往左往右找只要暴力跳k次，删除也是O(1)的。

时间复杂度：O(nk)
代码部分
```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;

const int N=5e5+7;
int t,n,k,a[N],idx[N];///a表示的是这个数组，idx表示的是某个数在的位置
struct Node
{
    int pre,nxt,idx;
} node[N];

int main()
{
    scanf("%d",&t);
    while(t--)
    {
        scanf("%d%d",&n,&k);
        for(int i=1;i<=n;i++)
        {
            scanf("%d",&a[i]),idx[a[i]]=i;
            node[i]=Node {i-1,i+1,i};
        }
        node[n+1].idx=n+1;
        ll ans=0;
       for(int i=1;i<=n;i++)///找到当前这个数
        {
            int l=idx[i],r=idx[i];///左端点，右端点
            int cntl=1,cntr=0;///往前、往后找的数的个数
            while(cntl<k)///往前找，看有这个数的前面有多少个数
            {
                if(node[l].pre==0)break;///左端点已经是第一个元素了
                cntl++,l=node[l].pre;
            }
            while(cntl)///在前面有这么多数的基础上
            {
                while(cntr+cntl>k)///左右区间的个数大于k了
                {
                    cntr--,r=node[r].pre;///右区间往前移动
                }
                while(cntl+cntr<k)///左右区间的个数小于k了
                {
                    if(node[r].nxt==n+1)break;///移动到最后也就结束了，不能再往后移动
                    cntr++,r=node[r].nxt;///右区间往后移动
                }
                if(cntl+cntr==k)///正好左右区间中有这么多数
                {
                    int L=node[l].idx-node[node[l].pre].idx;///左边涉及的区间
                    int R=node[node[r].nxt].idx-node[r].idx;///右边涉及的区间
                    ans+=1ll*L*R*i;
                    //printf("L====%d    R====%d  i===%d\n",L,R,i);
                }
                l=node[l].nxt,cntl--;///左区间往后移动
            }
            node[node[idx[i]].pre].nxt=node[idx[i]].nxt;
            node[node[idx[i]].nxt].pre=node[idx[i]].pre;
        }
        printf("%lld\n",ans);
    }
    return 0;
}

```