HDU_6097 Mindis 【几何】
<!--more-->

> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6097)

### 题目描述 ###
给定一个圆心在坐标原点的圆的半径，然后给出两个到该圆的圆心等长的非圆外点P，Q（点既可 以在圆内，也可以在圆上），让从圆上找一点D使其到两点的距离最短，即DP+DQ最小。
### 解题思路 ###
请况分为以下三种：

- P点和Q点重合，这时直接输出2*（R-OP）； 
- OP=OQ=R； 这时的最多短距离就是P Q的距离了。 
- OP=OQ < R;这种情况比较麻烦，首先我们要明白一个概念反演点。 
反演点：已知圆O的半径为R，从圆心O出发任作一射线，在射线上任取两点M,N。若OM=m，ON=n，且mn=R^2，则称点M,N是关于圆O的反演点。 

![](http://www.adreame.com/wp-content/uploads/2017/08/几何1.png)

>由图可知： 
OQ×OQ1 =r×r=|OD|² 
易知△ODQ∽△ODQ1 △ODP∽△ODP1 
故可以推出OP/OD=DP/DP1 OQ/OD=DQ/DQ1 
两式结合DP+DQ=(OP/OD) * DP1+(OQ/OD) * DQ1 
又∵OP =OQ 
∴DP+DQ=OP/OD（DP1+DQ1） 
所以我们只需要求出（DP1+DQ1）的值即可

![](http://www.adreame.com/wp-content/uploads/2017/08/几何2.png)

上图为od1>r的情况，第一个是od1<=r的情况。

### 代码部分 ###
```cpp
#include <bits/stdc++.h>

using namespace std;
double dis(double x1,double y1,double x2,double y2)
{
    double d = sqrt((x2-x1)*(x2-x1)+(y2-y1)*(y2-y1));
    return d;
}
int main()
{
    int t;scanf("%d",&t);
    while(t--)
    {
        double px,py,qx,qy,r;
        scanf("%lf %lf %lf %lf %lf",&r,&px,&py,&qx,&qy);
        if(px == qx&&py == qy){///第一种情况
            printf("%.7lf\n",2.0*(r - dis(0.0,0.0,px,py)));
        }
        else if(dis(0.0,0.0,px,py) == r){///第二种情况
            printf("%.7lf\n",dis(px,py,qx,qy));
        }
        else{///第三种情况
            double op = dis(px,py,0.0,0.0);
            double a = acos((px*qx+py*qy)/(op*op))/2.0 ;///利用反三角函数求出角POD
            double op1 = r*r/op;
            double od1 = op1*cos(a);///利用角POD求出O点到距离od1
            if(od1<=r)
            {
                printf("%.7lf\n",2.0*op1*sin(a)*(op/r));///若距离小于半径，则最短距离为p1q1*(op/r)
            }
            else{
                double dd1 = od1 - r;
                double dp1 = sqrt(dd1*dd1 + (op1*sin(a))*(op1*sin(a)));///否则计算出dp1的距离dq1 = dp1
                printf("%.7lf\n",2.0*dp1*op/r);
            }
        }
    }
    return 0;
}
```

> [原文链接](http://blog.csdn.net/merry_hj/article/details/77151650)