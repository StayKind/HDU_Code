### 1002. Balala Power! ###

>[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6034)


#### 题目意思： ####
巴拉巴拉变成猪！！！ 讲意思之前先吐槽一番，做题简直就像是做阅读理解。
咳咳，回归正题：
输入几个只有小写英文字母字符串，a-z可以用0-25来任意代替，但要尽可能的取最大值。取值的过程因为数值过大，要不断的对1e9+7取余。
一行题意看原文看小半个小时TT。
#### 解题思路 ####
每个字符对答案的贡献都可以看作一个 26 进制的数字，问题相当于要给这些贡献加一个 0 到 25 的权重使得答案最大。最大的数匹配 25，次大的数匹配 24，依次类推。排序后这样依次贪心即可，唯一注意的是不能出现前导 0。
#### 难点坑点： ####
1. 给字母赋值
2. 前导零

**给字母赋值：**将字母的权值位数用一个二维数组进行存储，然后将每一位都进行从小到大的排序，这样就可以给字母赋值了
**前导零处理：**前面已经将所有的字母的比重按从小到大的顺序排好，所以在排除前导零的过程为了保证的到的值最大，所以要排除比重最小的前导可能为零的情况。具体实现就看代码部分吧。
#### 代码部分： ####

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;

const int N = 100020;  //最大的字符串长度，这里注意需要开大一点，在计算权值的时候防止溢出
const int Q = 1e9 + 7;  //MOD
int n, L;//L为输入的字符串的最大长度
int num[26][N];//权值位个数，对其进行sort排序，判断应赋予的值
int power[N], sum[N];//计算字符串的和所用到，power表权重，sum表字母的权重和
bool ban[26];  //标记前导0

char str[N];
int a[26];  //用于cmp排序

bool cmp(int A, int B)
{
    for (int i = L - 1 ; i >= 0 ; -- i)
    {
        if (num[A][i] != num[B][i])
        {
            return num[A][i] < num[B][i];
        }
    }
    return 0;
}

void work()
{
    memset(num, 0, sizeof(num));
    memset(ban, 0, sizeof(ban));
    memset(sum, 0, sizeof(sum));
    L = 0;
    ///排列字母的比重，过程有累加字母的权值位数，通过权值位数进行排序。
    ///在这个过程中，也对字符串的相加做了铺垫。
    ///注意过程中，需要不断得对mod取余
    for (int i = 0 ; i < n ; ++ i)
    {
        scanf("%s", str);
        int len = strlen(str);
        if (len > 1)//如果字符串的长度大于一，表示其前导不能为零 ban从0变成1
        {
            ban[str[0] - 'a'] = 1;
        }
        reverse(str, str + len); //字符串反转
        
        for (int j = 0 ; j < len ; ++ j)
        {
            ++ num[str[j] - 'a'][j];
            sum[str[j] - 'a'] += power[j];///？  sum存放权值，用于求和所用
            if (sum[str[j] - 'a'] >= Q)  //还要不断对mod取余
            {
                sum[str[j] - 'a'] -= Q;
            }
        }
        L = max(L, len);//刷新其输入的字符串的最大长度
    }
    for (int i = 0 ; i < 26 ; ++ i) //对权值进位
    {
        for (int j = 0 ; j < L ; ++ j)
        {
            num[i][j + 1] += num[i][j] / 26;
            num[i][j] %= 26;
        }
        while (num[i][L])//将最高位向后拓展
        {
            num[i][L + 1] += num[i][L] / 26;
            num[i][L ++] %= 26;
        }
        a[i] = i;
    }
    sort(a, a + 26, cmp);
    ///前面的sort过程已经将所有的字母的比重按从小到大的顺序排好，
    ///所以在排除前导零的过程为了保证的到的值最大，所以要排除比重最小的前导可能为零的情况。
    ///排除前导0的过程
    ///从小到大可以保证求得的值最大
    int zero = -1;
    for (int i = 0 ; i < 26 ; ++ i)
    {
        if (!ban[a[i]])
        {
            zero = a[i];
            break;
        }
    }
    ///计算字符串之和，二十六进制转十进制求和
    int res = 0, x = 25;
    for (int i = 25 ; i >= 0 ; -- i)
    {
        if (a[i] != zero)///前导为零的在计算过程之中将其排除
        {
            res += (LL)(x --) * sum[a[i]] % Q;
            res %= Q;
        }
    }
    static int ca = 0;
    printf("Case #%d: %d\n", ++ ca, res);
}

int main()
{
    //N 为最大位数
    power[0] = 1;
    ///每位上的权值（需要在计算的过程中不断对Q取余）,字符串相加中会用到
    for (int i = 1 ; i < N ; ++ i)  
    {
        power[i] = (LL)power[i - 1] * 26 % Q;
    }
    while (~scanf("%d", &n))
    {
        work();
    }
    return 0;
}

``` 	
