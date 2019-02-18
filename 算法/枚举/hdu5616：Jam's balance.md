### hdu5616: Jam's balance (二进制枚举子集)

***

#### 题目传送门

[hdu5616：Jam's balance](http://acm.hdu.edu.cn/showproblem.php?pid=5616)

#### 解题思路：

> 因为砝码只有20个,我们可以通过二进制的方式来枚举出所有的子集情况，然后用一个数组来对所以出现的重量进行标记打表，最后利用该表来判断是否能称出该质量即可。

------

#### 二进制枚举子集的核心思路

首先一个集合的子集有$2^n$个（该 $n$ 的值一般  $n<=12$），所以枚举的个数有`（1<<n​）`个。

所以最外层循环应为：`for(int i=0; i<(1<<n); i++)`

二进制枚举的过程如下：

每个位置值为1则保留，不为1则舍弃 ；

设$s=11$（二进制为`1011`）那么我们保留` 0 1 3` 位置上的数值；

那么如何找到每个位置上的数值呢？

我们遍历十进制表示（比如`11`），我们可以转化为二进制再枚举每一位

一个很巧妙的方式就是利用位运算；

`1<<0=1(0);`

`1<<1=2(10);`

`1<<2=4(100);`

`1<<3=8(1000);`

`1<<4=16(10000);`

...

`1<<7=128(10000000);`

我们只需要将`11&(1<<i)`我们便可以得到每一位是不是1 

（`1<<i`除了那一位，剩余的都是0，所以我么就可以得到那一位是不是1）

**最终代码如下：**

```cpp
for(int i=0; i<(1<<n); i++)//二进制枚举//枚举每一个状态
    {
        for(int j=0; j<n; j++)//枚举该状态下二进制的每一位数值
        {
            if(i&(1<<j))//当前状态的第i位是否为1
                printf(" %d ",a[j]);
        }
    }
```

***

### 实现代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int arr[25];
bool vis[2005];
int main()
{
    int T, n, m, weight;
    cin >> T;
    while(T--) {
        memset(vis, false, sizeof(vis));
        cin >> n;
        for(int i = 0; i < n; ++i) cin >> arr[i];
        cin >> m;
        for(int i = 0; i < (1 << n); ++i) {
            int cnt = 0;
            for(int j = 0; j < n; ++j) {
                if(i & (1 << j)) cnt += arr[j];
            }
            vis[cnt] = true;
            for(int j = 0; j < n; ++j) {
                if(cnt - arr[j] > 0) vis[cnt - arr[j]] = true;
            }
        }
        while(m--) {
            cin >> weight;
            if(vis[weight]) puts("YES");
            else puts("NO");
        }
    }
    return 0;
}
```

