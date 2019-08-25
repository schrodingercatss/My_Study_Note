## 牛客网：Stone

### 题目传送门

[牛客网：Stone](<https://ac.nowcoder.com/acm/contest/893/D>)

***

### 解题思路

因为每次只能选择相邻的两堆合并，并且费用为较小的那一堆，所以我们可以从石头数目最大的那一堆一次合并其他堆，这样就相当于总费用为其他$n-1$堆的和。

***

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    int T, n;
    ll t, maxv, sum;
    scanf("%d", &T);
    while(T--) {
        maxv = sum = 0;
        scanf("%d", &n);
        for(int i = 0; i < n; ++i) {
            scanf("%lld", &t);
            maxv = max(maxv, t);
            sum += t;
        }
        cout << sum - maxv << endl;
    }
    return 0;
}
```

