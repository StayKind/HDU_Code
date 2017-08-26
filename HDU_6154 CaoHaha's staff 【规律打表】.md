
<!--more-->
>[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6154)

### 题目描述 ###
在一个二维坐标系中，想要得到一块为n的面积，问最少在坐标轴上面画多少笔画可以得到这样的面积，笔画可以是上、下、左、右、两种对角线共六种。
### 解题思路 ###
规律题，打表求出笔画为n时可以得到最大的面积，然后在找的过程周找到大于等于n的最小的下标即可。
核心是一个找边长为根号`√2`倍数的菱形（正方形）。然后从第一个菱形边长为`√2`开始一点一点扩展，即可找出规律。规律可参考代码部分。
补充一下找规律的步骤：

![](http://www.adreame.com/wp-content/uploads/2017/08/DB.png)

看上图黑色的正方形为第一个长为 `√2` 的菱形，也就是起始位置。

 - 所以我们先将DB[4] = 2，也就是边长为4的最大为面积为2
 - 紧接着画5条边构成的最大面积，也就是红色的那条线，面积比第4条边多了0.5，这时候我们赋值x1为0.5。由此可以推断DB[5] = DB[5 - 1] + x1。
 - 6条边：为蓝色的边，面积比4条边多了2.0，这时候我们赋值x2为2.0。由此可以推断 DB[6] = DB[5 - 2] + x2。
 - 7条边：为绿色的边，面积比6条边多了1.5，这时候我们知道之前的x1 = x1 + 1.0。由此推断DB[7] = DB[7 - 1] + x1;
 - 8条边：为橙色的边，面积比6条边多了4.0，这时候我们知道之前的x2 = x2 + 2.0。由此推断DB[8] = DB [8 - 2] + x2；
 - 再接着往下就是和上面的一样，每次加四条边就是一个循环。自己拿笔画一下就可以得出答案了。

### 代码部分 ###
```cpp
#include <iostream>
#include <string>
#include <string.h>
#include <cstring>
#include <algorithm>
using namespace std;
const int maxn = 1e5;
double DB[maxn];
int main()
{
    ios::sync_with_stdio(false);
    double x1 = 0.5, x2 = 2;
    DB[4] = 2;
    for(int i = 5; i <= maxn ; ++ i)
    {
        if(i % 4 == 1)DB[i] = DB[i - 1] + x1, x1 += 1.0;
        if(i % 4 == 2)DB[i] = DB[i - 2] + x2, x2 += 2.0;
        if(i % 4 == 3)DB[i] = DB[i - 1] + x1;
        if(i % 4 == 0) DB[i] = DB[i - 2] + x2;
    }
    int t;
    cin >> t;
    while(t --)
    {
        double n;
        cin >> n;
        for(int i = 4; i <= maxn; ++ i)
        {
            if(DB[i] >= n)
            {
                cout << i << endl;
                break;
            }
        }
    }
    return 0;
}
```
