HDU_6050 Funny Function 【规律+逆元】
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6050)

### 题目意思： ###
根据题中所给的规律，求出给定的m，n的时候，F(m,1)的值为多少。因为在求解过程中，得到的结果会比较大，所以在求解的过程中需要不断得对`1e9+7`取余。
### 解题思路： ###
为了讲解过程方便，现将其打表：
```
n=2							
1	1	3	5	11	21	43	 85
2	4	8	16	32	64	128	 256
6	12	24	48	96	192	384	 768
18	36	72	144	288	576	1152 2304
n=3							
1	1	3	5	 11	  21	43	  85
5	9	19	37	 75	  149	299	  597
33	65	131	261	 523  1045	2091  4181
229	457	915	1829 3659 7317	14635 29269
n=4							
1	 1	  3	   5	 11	   21	 43	    85
10	 20	  40   80	 160   320	 640	1280
150	 300  600  1200	 2400  4800	 9600	19200
2250 4500 9000 18000 36000 72000 144000	288000
n=5							
1	   1	   3	   5	   11	   21	    43	     85
21	   41	   83	   165	   331	   661	    1323	 2645
641	   1281	   2563	   5125	   10251   20501    41003	 82005
19861  39721   79443   158885  317771  635541   1271083	 2542165
615681 1231361 2462723 4925445 9850891 19701781 39403563 78807125
```


要求的是F(m,1)，则肯定与上面的结果有关系的，数据量过大，肯定是没有办法循环得到结果，由此看出，这道题必是规律题。

---
当n为奇数和偶数的时候，求F(m,1)的规律是不一样的。

- **偶数：** 打表可以看出当n为偶数的时候，从F(2,1)开始，第一列的下一个是上一个的k倍，多打机组数据可看出，k为`3,15,63,255……`k的规律就是`2^n-1`。由此可得出，当n为偶数的时候，`F(m,1) = F(2,1)*(2^n-1)^(m-2)`。
- **奇数：** 奇数求解的过程就相对来说比较复杂一点了。当n为奇数的时候，可以发现，F(m-1,1)*(2^n-1)与F(m,1)之间为一个常数k。我们就可以进一步的通过这样的关系求出F(m,1)的值

**奇数求解过程：**通过打表可以看出，当n等于`3,5,7,9……`的时候，k的值为`2,10,42,170……`
可以看出之间的规律：
```
2 = 2^1
10 = 2^1+2^3
42 = 2^1+2^3+2^5
170 = 2^1+2^3+2^5+2^7
……
//可以看出是等比为2^2的前k项和 
```

---

结合上面可以得出：`F(m-1,1)*(2^n-1)-F(m,1) = Sk`
`F(m,1) = F(m-1,1)*(2^n-1)-Sk;`
又可以根据这个公式推出

![](http://115.159.67.147/wp-content/uploads/2017/07/FF.png)

---

算到这里就仅仅剩下F(2,1)的求解了
先打表看看n:(1~i)第一行与F（2,1）的关系
```
S[i]: 1 1 3 5 11 21 43 85 171 341 683 1365 2731 5461 10923 21845 43691 87381 174763 349525
F[i]: 1 2 5 10 21 42 85 170 341 682 1365 2730 5461 10922 21845 43690 87381 174762 349525
```
F[i]表示当n为i时F(2,1)的值。
当i为奇数的时候`F[i]=s[i+1];`当i为偶数的时候`F[i]=s[i+1]-1;`
 `s[i+1]=((-1)^i+2^(i+1))/3;`
接下来的求解过程就只需要注意边计算边取模就可以了。

注意：在对除数取余的时候为了防止精度的丢失，需要用到乘法的逆元，将除法变成乘法。这里使用的乘法逆元算法为费马小定理。

---

#### **费马小定理** ####
在p是素数的情况下，对任意整数x都有`x^p≡x(mod)p`。
可以在p为素数的情况下求出一个数的逆元，`x∗x^(p−2)≡1(mod p)`，`x^(p−2)`即为逆元。
```cpp
ll mult(ll a,ll n)  //求a^n%mod
{
    ll s=1;
    while(n)
    {
        if(n&1)s=s*a%mod;
        a=a*a%mod;
        n>>=1;
    }
    return s;
} //mult(a,n-2);

```

---

### 代码部分： ###

```cpp
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <cmath>
#include <vector>
#include<iostream>
using namespace std;

typedef __int64 LL;
const int mod = 1e9+7;
LL n,m;
LL mult(LL a,LL n)
{
    LL ans=1;
    while(n)
    {
        if(n&1)ans=(ans*a)%mod;
        a=(a*a)%mod;
        n>>=1;
    }
    return ans;
}

void solve()
{
    if(m==1)
    {
        cout<<"1"<<endl;
        return;
    }
    LL three=mult(3,mod-2);         // 3 的逆元 费马小定理 求乘法逆元
    LL one=((mult(2,n+1)+(n&1?-1:1)+mod)%mod*three)%mod;    // F[1][n+1]
    if(n%2==0)
        one=(one-1+mod)%mod;  // 计算 F[2][1]
    LL ans=0;
    LL mu=(mult(2,n)-1+mod)%mod;    // 2^n-1
    LL mun=mult(mu,m-2);            // (2^n-1)^(m-2)
    if(n%2==0)                      // 偶数时
        ans=(one*mun)%mod;
    else                            // 奇数时
    {
        LL cha=n/2;
        cha=((2*(mult(4,cha)-1+mod)%mod)%mod)*three%mod;
        LL add=((cha*(mun-1+mod)%mod)*mult((mu-1+mod)%mod,mod-2))%mod;
        ans=((one*mun)%mod-add+mod)%mod;
    }
    cout<<ans<<endl;
}

int main()
{
    ios::sync_with_stdio(false);
    int T;
    cin>>T;
    while(T--)
    {
        cin>>n>>m;
        solve();
    }
    return 0;
}
```

>源代码：[**千千**](https://www.dreamwings.cn/hdu6050/4839.html)

---
#### 最后讲一个`cin`的小技巧 ####
`ios::sync_with_stdio(false);`
可以将cin和scanf的效率相匹敌。
详情说明：
scanf在Linux平台上测试结果为2.01秒
```cpp
const int MAXN = 10000000;  
int numbers[MAXN];  
void scanf_read()  
{  
    freopen("data.txt","r",stdin);  
    for (int i=0;i<MAXN;i++)  
        scanf("%d",&numbers[i]);  
```
cinLinux平台上测试结果为6.38秒
```cpp
const int MAXN = 10000000;  
   
int numbers[MAXN];  
   
void cin_read()  
{  
    freopen("data.txt","r",stdin);  
    for (int i=0;i<MAXN;i++)  
        std::cin >> numbers[i];  
}  
```
cin慢是有原因的，其实默认的时候，cin与stdin总是保持同步的，也就是说这两种方法可以混用，而不必担心文件指针混乱，同时cout和stdout也一样，两者混用不会输出顺序错乱。正因为这个兼容性的特性，导致cin有许多额外的开销，如何禁用这个特性呢？只需一个语句`std::ios::sync_with_stdio(false);`，这样就可以取消cin于stdin的同步了。

```cpp
const int MAXN = 10000000;  
   
int numbers[MAXN];  
   
void cin_read_nosync()  
{  
    freopen("data.txt","r",stdin);  
    std::ios::sync_with_stdio(false);  
    for (int i=0;i<MAXN;i++)  
        std::cin >> numbers[i];  
}  
```
**取消同步后效率究竟如何？经测试运行时间锐减到了2.05秒，与scanf效率相差无几了！有了这个以后可以放心使用cin和cout了。**

[toc]