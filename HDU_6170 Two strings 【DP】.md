HDU_6170 Two strings 【DP】
<!--more-->
> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6170)

### 题目描述 ###
给定一个文本串和一个匹配串，问匹配串是否可以构成文本串，如果可以构成就输出"yes"，否则就输出"no"。
文本串和匹配串都由大小写构成，但是匹配串当中包含了特殊的字符：'.'可以代替任意一种的字符，'*'可以将上一个字符前面与他相同的删除，也可以无限将前面的字符向后拓展，也可以不做任何操作。
### 解题思路 ###
DP思想，开一个二维的dp数组，存放匹配串和文本串分别为i,j时 是否满足匹配。如果满足，值为真，否则值为假，最后判断`dp[len2][len1]`是否为真即可。
**DP的思想**

- 当文本串和匹配串相同，或者匹配串为.则满足为真的条件但要注意这里的dp应该是`dp[i][j] = dp[i - 1][j - 1];`只有前面都满足这才能为真。
- 当匹配串的字符为 * ,当匹配串为*的时候，可以进行向前删除也可以向后填充字符，满足其中一个那么`dp[i][j] = true;`
- **特殊情况**第二个为*，*可以起到删除前面的字符的作用，所以`dp[i][0] = true;`

### 代码部分 ###
```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e4;
bool dp[maxn >> 1][maxn >> 1]; //存放的是匹配串和文本串分别为i,j时 是否满足匹配
char s1[maxn >> 1];//文本串
char s2[maxn >> 1];//匹配串

int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin >> t;
    while(t --)
    {
        cin >> s1 + 1;
        cin >> s2 + 1;
        memset(dp, false, sizeof(dp));
        dp[0][0] = true;
        int len1 = strlen(s1 + 1);
        int len2 = strlen(s2 + 1);
        for(int i = 1; i <= len2; ++ i)
        {
            ///当匹配串第二个为*的时候就说明dp[i][0]都可以匹配，因为它可以把前面的字符给删掉
            if(i == 2 && s2[i] == '*') dp[i][0] = true;
            for(int j = 1; j <= len1; ++ j)
            {
                ///当匹配字符为.的时候或者文本串和匹配串相等的时候，dp[i][j]都等于前一个状态
                if(s2[i] == '.' || s1[j] == s2[i]) dp[i][j] = dp[i - 1][j - 1];
                else
                {
                    if(s2[i] == '*')
                    {
                        ///当匹配串为*的时候，可以进行向前删除也可以向后填充字符，满足其中一个那么dp[i][j] 都为真
                        ///满足删除的条件dp[i][j]为真
                        dp[i][j] = dp[i - 1][j] | dp[i - 2][j];
                        ///这个就是代表*可以像后面填充相同字符，然后dp[i][j]为真
                        if((dp[i - 1][j - 1] || dp[i][j - 1]) && s1[j - 1] == s1[j]) dp[i][j] = true;
                    }
                }
            }
        }
        puts(dp[len2][len1]?"yes":"no");
    }
    return 0;
}

```