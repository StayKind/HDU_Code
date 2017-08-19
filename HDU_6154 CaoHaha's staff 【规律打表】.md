HDU_6154 CaoHaha's staff 【规律打表】
<!--more-->
>[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6154)

### 题目描述 ###
在一个二维坐标系中，想要得到一块为n的面积，问最少在坐标轴上面画多少笔画可以得到这样的面积，笔画可以是上、下、左、右、两种对角线共六种。
### 解题思路 ###
规律题，打表求出笔画为n时可以得到最大的面积，然后在找的过程周找到大于等于n的最小的下标即可。
核心是一个找边长为根号`√2`倍数的菱形（正方形）。然后从第一个菱形边长为`√2`开始一点一点扩展，即可找出规律。规律可参考代码部分。
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
    double x1 = 1.5, x2 = 4;
    DB[4] = 2;
    DB[5] = 2.5;
    DB[6] = 4;
    for(int i = 7; i <= maxn ; ++ i)
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