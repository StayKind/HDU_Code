<!--more-->

>[原题链接](http://acm.hdu.edu.cn/showproblem.php?pid=2577)

### 题目要求： ###
给出一个字符串，判断最少需要按多少下键盘才能打出这行带大写和小写的英文字符串。（起始的时候大写锁定键是关着的，要求结束的时候大写锁定键也是关的）。
### 解题思路： ###
- **第一种方法模拟**
模拟的思路：对每个字符进行处理，分四种情况来考虑。
//大写键未上锁遇到小写
//大写键上锁遇到大写
//大写键未上锁遇到大写来判断是否按大写锁定，还是按shift。
//大写键上锁遇到小写来判断是否按大写锁定，还是按shift。
前两种情况直接让ans++即可。
后面的情况利用了贪心的思想，如果一个字母后面跟着两个不同形式的字母时，按大小写锁定键和shift是相同的按键数量，所以统一采用按大小写锁定键，如果只有一个不一样的，那就按shift即可。

**代码如下：**

```cpp
#include<iostream>
#include<stdio.h>
#include<string>
using namespace std;
int main()
{
    int n;
    cin >> n;
    while(n--)
    {
       string ch;
       cin>>ch;
       int len=ch.size();
       int i,ans,flag;
       i = ans = flag = 0;
       while(i<len)
       {
          if(flag==0&&ch[i]>='a'&&ch[i]<='z') //大写键未上锁遇到小写
              ans++;
          if(flag==1&&ch[i]>='A'&&ch[i]<='Z') //大写键上锁遇到大写
              ans++;
          if(flag==0&&ch[i]>='A'&&ch[i]<='Z') //大写键未上锁遇到大写来判断是否按大写锁定，还是按shift。
          {
              if(i+1<len)
              {
                    if(ch[i+1]>='A'&&ch[i+1]<='Z')
                    {
                        flag=1;
                        ans+=2;
                    }
                    else
                    {
                        ans+=2;
                    }
              }
              else
              ans+=2;

          }
          if(flag==1&&ch[i]>='a'&&ch[i]<='z')//大写键上锁遇到小写来判断是否按大写锁定，还是按shift。
          {
              if(i+1<len)
              {
                    if(ch[i+1]>='a'&&ch[i+1]<='z')
                    {
                        flag=0;
                        ans+=2;
                    }
                    else
                    {
                        ans+=2;
                    }
              }
              else
              {
                  ans+=2;
                  flag=0;
              }
          }
          i++;
       }
       if(flag==1)
        ans+=1;
       printf("%d\n",ans);
    }
    return 0;
}

```
- **第二种方法动态规划**
动态规划思路：定义`dp[0][i]`为小写状态下敲击完第i个字母时最小敲击次数，`dp[1][i]`为大写状态下敲击完第i个字母时最小敲击次数。很容易可以找出转移方程。

**代码如下：**

```cpp
#include <bits/stdc++.h>
#define maxn 110
using namespace std;

int dp[2][maxn];
int main()
{
    int t;
    char s[maxn];
    scanf("%d", &t);
    while(t--)
    {
        scanf(" %s", s + 1);
        int len = strlen(s + 1);
        dp[0][0] = 0, dp[1][0] = 1;
        for(int i = 1;  s[i]; i++)
        {
            if(s[i] >= 'a' && s[i] <= 'z')//当前是小写字母
            {
                dp[0][i] = min(dp[0][i-1] + 1, dp[1][i-1] + 2);//小写状态直接按字母，大写状态按CapsLock+字母，取较小值
                dp[1][i] = min(dp[0][i-1] + 2, dp[1][i-1] + 2);//小写状态按字母+CapsLock，大写状态按shift+字母，取较小值
            }
            else//大写字母
            {
                dp[0][i] = min(dp[0][i-1] + 2, dp[1][i-1] + 2);//小写状态按shift+字母，大写状态按字母+CapsLock，取较小值
                dp[1][i] = min(dp[0][i-1] + 2, dp[1][i-1] + 1);//小写状态按CapsLock+字母，大写状态直接按字母，取较小值
            }
        }
        dp[1][len] += 1;//大写需要变成小写锁定
        printf("%d\n", min(dp[0][len], dp[1][len]));
    }
    return 0;
}

```
