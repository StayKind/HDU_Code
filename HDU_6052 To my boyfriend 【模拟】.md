HDU_6052 To my boyfriend 【模拟】
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6052)

### 题目意思 ###
给定一个m*n的矩阵，矩阵的每一个单元上都有一种颜色，子矩阵的权值为子矩阵中不同颜色的数量，最后让你求出子矩阵权值之和除以所有的子矩阵之和。最后保留九位小数。
### 解题思路 ###
首先，求`m*n`的矩阵总共可以构成多少种子矩阵。这是有公式可以直接求出来的。公式：`n*(n+1)m(m+1)/*4 `
接着难点就是来求不同颜色的矩形。
可以把每种颜色的每个点进行单独考虑，考虑过之后会将其标记，不会算入其他的子矩阵之中。考虑的过程是把考虑的点判断除标记点外可以构成多少个，把这种矩阵加起来就可以表示所有的子矩阵之和。
#### 分析测试数据 ####
测试数据矩阵如下图：
![](http://115.159.67.147/wp-content/uploads/2017/07/1-1.png)

- 首先先考虑颜色为1的点。先考虑坐标为(1,1)的点。可以构成子矩阵的数量为`1*2*3`。为6。测完以后将其标记以防重复。
- 第二个点(3,1)。构成矩阵的数量为`2*2*1`。为4。测完同标记
- 第三个点(2,2)。构成矩阵的数量为`2*2*1+1*1*1`。为5。颜色为1的点找完以后开始找颜色为二的点。
- 然后是颜色为2的点。先考虑坐标为(1,2)的点。可以构成子矩阵的数量为`2*2*2`。为8.测完以后记性标记。
- 第二个点(2,1)。数量`1*3*1+1*1*1=4`，标记。
- 第三个点(2,3)。数量`1*2*1+1*1*1=3`，最后结束。

最后的结果为：`6+4+5+8+4+3=30`，总的子矩阵个数为`2*3*3*4/4`。最后的结果为`30/18`保留9位小数。
具体的计算步骤可以参考代码分析。

### 代码部分 ###

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
LL w[105][105];//保存原理图
int vis[105],m,n;//vis[j] = i 表示将第j列用i颜色标记
vector<int> y[105];//y[i]用来表示第i行有哪些列被标记过
vector<pair<int,int> > C[10005];//用来保存所有颜色出现的坐标
LL calc(int col)
{
    memset(vis,0,sizeof(vis));
    LL sum = 0;
    for(auto now:C[col])
    {
        int ni = now.first,nj = now.second;//保存当前颜色的坐标
        for(int i = 1; i <= m; i++)//对于每次找那些坐标被标记过之间都应该将此数组清零，防止以前数据对这次查找有影响
            y[i].clear();
        for(int i=1; i<=m; i++)
            if(vis[i])
                y[vis[i]].push_back(i);
        int yl = 1,yr = m,flag = 0;
        for(int i = ni; i>=1; i--)
        {
            //枚举所有可能的上边界
            vector<int>::iterator it;
            for(it = y[i].begin(); it!=y[i].end(); it++)
            {
                //如果第i行有被标记的，找出被标记的列数，并更新左右区间
                int yy = *it;
                if(yy<nj)
                    yl = max(yl,yy+1);
                else if(yy > nj)
                    yr = min(yr,yy-1);
                else
                {
                    //如果这个点的正上方有与它颜色相同的点结束循环
                    flag = 1;
                    break;
                }
            }
            if(flag) break;
            sum += (n-ni+1)*(nj-yl+1)*(yr-nj+1);//计算已ii上上边界的方案数，
        }
        vis[nj] = ni;//将第nj列标记为ni方边利用y数组更新边界
    }
    return sum;
}
int main()
{
    int T;
    scanf("%d",&T);
    while(T--)
    {
        scanf("%d %d",&n,&m);
        for(int i=0; i<=n*m; i++)
            C[i].clear();
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= m; j++)
            {
                scanf("%lld",&w[i][j]);
                C[w[i][j]].push_back(make_pair(i,j));//记录每种颜色的坐标
            }
        LL sum = 0;//记录不同颜色子矩阵的个数
        for(int i=0; i<=n*m; i++)
            if(!C[i].empty())
            {
                sort(C[i].begin(),C[i].end());//将每种颜色按坐标先行优先再列优先排序
                sum += calc(i);
            }
        LL num = n*(n+1)*m*(m+1)/4;//子矩阵的个数
        double ans = (sum*1.0)/num;
        printf("%.9lf\n",ans);
    }
    return 0;
}

```

这里使用了`for(auto now:C[col])`，auto是 c++11 里面的自动类型推导。如果不知道auto做什么的，点击：[auto的用法](http://www.cnblogs.com/me115/p/4800777.html)
如果代码直接编译不了就可以将codeblocks进行修改
Setting->Compiler->
![](http://115.159.67.147/wp-content/uploads/2017/07/X5UJHG1M3RXYCPR_1XM.png)