HDU_【2017 Multi-University Training Contest 1】

<!--more-->
### 1001. Add More Zero ###

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6033)

#### 题目意思： ####
没啥意思，单纯的数学公式。
求log10（2^m-1）向下取整

![](http://115.159.67.147/wp-content/uploads/2017/07/1.png)

#### 代码部分： ####

```cpp
#include <iostream>
#include <stdio.h>
#include <math.h>

using namespace std;

int main()
{
    int m,k,t=0;
    while(~scanf("%d",&m))
    {
        k = (int)m*log10(2.0);
        printf("Case #%d: %d\n",++t,k);
    }
    return 0;
}
```
### 1011. KazaQ's Socks ###

>[原题链接](http://acm.hdu.edu.cn/showproblem.php?pid=6043)

#### 题目意思： ####
一个人有n个袜子，编号1-n,每天穿一双，穿得只剩下一只的时候，会洗好按顺序放过去，继续穿，问第k次穿得袜子是编号多少的袜子。
#### 解题思路： ####
这是一道规律题。规律如下：
当n=3，袜子的编号：12312131213……由此可见`123`，`12`，`13`，`12`，`13`……12和13会一直循环下去
当n=4,袜子的编号：1234123124123124……由此可见`1234`，`123`，`124`，`123`，`124`……123和124会一直循环下去
等等
具体实现看代码部分吧
#### 代码部分: ####

```cpp
#include <iostream>
#include <stdio.h>
#define LL long long int
using namespace std;

int main()
{
    LL n,m;
    int flag=1;
    while(~scanf("%lld%lld",&m,&n))
    {
        printf("Case #%d: ",flag++);
        if(n <= m)
        {
            printf("%lld\n",n);
        }
        else
        {
            n -= m;
            LL a,b;
            a = n/(m-1);
            b = n%(m-1);
            if(b == 0)
            {
                if(a%2==0)
                {
                    printf("%lld\n",m);
                }
                else
                {
                    printf("%lld\n",m-1);
                }
            }
            else
            {
                printf("%lld\n",b);
            }
        }
    }
    return 0;
}
```

**总结下这次比赛，干坐五个小时，就做出来两道签到题。看大佬AC，叹息12与2的差距。不说了，补题……**