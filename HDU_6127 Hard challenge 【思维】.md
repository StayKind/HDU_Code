HDU_6127 Hard challenge 【思维】
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6127)

### 题目描述 ###
平面上有(n)个点，已知每个点的坐标为((x,y))，以及改点的权值为(val)，任意两点之间可以连接成一条不经过原点的边，该边的权值是两个端点的权值的乘积。现在要求画一条经过原点的直线，定义这条直线的权值为这条直线穿过的所有线段的值的和，问权值的最大值。
### 解题思路 ###
我们将所有的点按照极角排序，分别散落在y轴的左右两侧，y轴左端的点的和为suml，右端左右和为sumr，则全职和为suml*sumr 。我们先以y轴为起始点，不断偏移，更新suml，sumr。以便刷新ans，找到最大的权值。因为两点不经过原点，所以只能偏移一个点。
### 代码部分 ###
```cpp
#include<cmath>
#include<cstdio>
#include<algorithm>
typedef long long ll;
using namespace std;
#define eps 1e-9
#define maxn 200005
struct Node
{
    double x,y,ang;///横纵坐标，极角
    int v;///权值
} p[maxn];

bool cmp(Node a,Node b)///结构体按照极角从小到大排序
{
    return a.ang<b.ang;
}
int n,T;
ll sumL,sumR,ans;
void work()
{
    scanf("%d",&n);
    for(int i=1; i<=n; i++)
    {
        scanf("%lf%lf%d",&p[i].x,&p[i].y,&p[i].v);
        p[i].ang=atan(p[i].y/p[i].x);
    }
    sort(p+1,p+n+1,cmp);
    sumL=sumR=ans=0;
    for(int i=1; i<=n; i++)
        if (p[i].x>eps)sumL+=p[i].v;
        else sumR+=p[i].v;
    ans=sumL*sumR;
    for(int i=1; i<=n; i++)
    {
        if (p[i].x>0)sumL-=p[i].v,sumR+=p[i].v;
        else sumL+=p[i].v,sumR-=p[i].v;
        ans=max(ans,sumL*sumR);
    }
    printf("%lld\n",ans);
}
int main()
{
    scanf("%d",&T);
    while (T--)
        work();
    return 0;
}
```