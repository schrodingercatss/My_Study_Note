## HDU1800: Flying to the Mars

### 题目传送门

[HDU1800: Flying to the Mars](<http://acm.hdu.edu.cn/showproblem.php?pid=1800>)

***

### 解题思路

这题我们其实就是一个组合问题，假设现在只有3种等级，集合分别为：$\left\{A_1,A_2,A_3····A_k\right\}$,$\left\{B_1,B_2,B_3····B_n\right\}$,$\left\{C_1,C_2,C_3····C_m\right\}$。

因为$\left\{A,B,C\right\}$,可以放在一个扫帚上，所以我们只需要判断这三个集合里面哪个集合的元素最多，那么就能确定扫把的数量。

所以我们自然而然的想到了用stl中的map来进行统计，也可以手写一个hash_table来实现计数。

***

### Code

```cpp
//法一：map计数法
#include <iostream>
#include<map>
using namespace std;
int main()
{
    int n;
    while(~scanf("%d",&n)) {
        int i;
        map<int,int> mp;
        int max=INT_MIN;
        for(i=0;i<n;i++)
        {
            int level;
            scanf("%d",&level);
            mp[level]++;
            if(mp[level]>max)
            {
                max=mp[level];                     
            }
        }
        printf("%d\n",max);
    }
    return 0;
}
```

```cpp
//法二：手动hash函数法
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <bitset>
#include <cstring>
#include <queue>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 1e4;
const int MOD = 1e4;
int Hash[N + 5], num[N + 5];
int res;
int ELFhash(char *str)
{
    unsigned int h = 0, g;
    while(*str) {
        h = (h << 4) + (*str++);
        if((g = h & 0xF0000000)) {
            h ^= (g >> 24);
            h &= ~g;
        }
    }
    return h;
}

int hash_table(char *str)
{
    int k = ELFhash(str);
    int t = k % MOD;
    while(Hash[t] != k && Hash[t] != -1) t = (t + 1) % MOD; // 开放地址处理法
    if(Hash[t] == -1) num[t] = 1, Hash[t] = k;
    else res = max(res, ++num[t]);
}

int main()
{
    int n;
    char str[105];
    while(~scanf("%d", &n)) {
        getchar();
        res = 1;
        memset(Hash, -1, sizeof Hash);
        for(int i = 1; i <= n; ++i) {
            scanf("%s", str);
            int j = 0;
            while(str[j] == '0') j++; // 去掉前导0,因为01和001可以视为一种
            hash_table(str + j);
        }
        printf("%d\n", res);
    }
    return 0;
}
```



