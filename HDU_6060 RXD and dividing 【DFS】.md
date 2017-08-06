HDU_6060 RXD and dividing 【DFS】

<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6060)

题目描述

给一棵树T，有n个结点。
给一个k，表示有k个集合，我们需要把2,3,4,…n号节点放入集合，要保证k个集合的并集等于{2,3,4,5n}，并且集合互不相交。(集合可以为空)
然后每次取一个集合Si与{1}求并，得到比如{1,2,3}，那么tempi = f({1,2,3})；f({1}并Si)的意思是把合内的所有点连接起来的边的权值和。最后把所有权值和相加的到答案。
最后问你能够得到最大的答案。

解题思路

我们要想得到最大的答案，那么就要尽可能的去利用这些边，也就是尽可能重复计算这些边。
那么我们想，假设先从叶子节点开始，把这些叶子节点放入一个集合，那么这个集合的temp值就会把所有的边都算一遍。那么下次我们取所有叶子节点的父亲，放入一个集合，那么这个集合的temp值会把除了叶子节点到父亲的那条那边的其他所有边都算一遍。因为集合可以为空，以此类推，我们就可以得到最大的答案。但是如果遇到集合不够的情况，就把剩下的所有点加入最后一个集合。
那么有以上分析，其实就是算每条边会算多少次，比如叶子节点到父亲的那条边会算一次。其实一条边会算多少次跟某个点的所有子孙节点个数有关，就比如样例中，2号点有3个子孙节点，那么2号点连接父节点的那条边会算3+1次。3号点有0个子孙节点，那么3号点连接父节点的那条边会算0+1次。
那么其实问题就是转化为求每个点的子孙节点个数，然后算出每条边要重复计算的次数即可。

代码部分

```cpp
#include<bits/stdc++.h>
using namespace std;
 
struct Edge{  
	intv,w;  
};  

Edge temp;  
vector<Edge>vec[1000005];  
int Size[1000005];  ///保存每个节点的子节点的个数
int w[1000005];  ///存权重

void dfs(intu,int pre){  ///递归找到每一个节点的子节点的个数
    Size[u]=1;  
	intlen=vec[u].size();  
	for(inti=0;i<len;i++){  
		int v=vec[u][i].v;  
		if(v!=pre){  
			w[v]=vec[u][i].w;  
			dfs(v,u);  
            Size[u]+=Size[v];  
        }  
    }  
}  

int main(){  
	intn,k;  
	while(~scanf("%d%d",&n,&k)){  
		for(inti=1;i<=n;i++){  
			vec[i].clear();  
            Size[i]=0;  
			w[i]=0;  
        }  
		for(inti=1;i<=n-1;i++){  
			intu,v,w;  
			scanf("%d%d%d",&u,&v,&w);  
			temp.v=v;  
			temp.w=w;  
			vec[u].push_back(temp);  
			temp.v=u;  
		vec[v].push_back(temp);  
       }  
	dfs(1,-1);  
	longlong sum=0;  
	for(inti=2;i<=n;i++){  
       sum+=(long long)w[i]*min(Size[i],k);  ///k个集合，字节个数要和k比较取小的
    }  
	printf("%lld\n",sum);  
  }  
return 0;  
}  	

```