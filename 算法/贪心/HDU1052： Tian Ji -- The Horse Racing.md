# HDU1052： Tian Ji -- The Horse Racing

***

### 题目传送门

[HDU1052： Tian Ji -- The Horse Racing](<http://acm.hdu.edu.cn/showproblem.php?pid=1052>)

***

### 解题思路

这题有很多解法，最经典的就是二分图匹配解法了，我们也可以使用最基本的贪心思想来解决这道题目。

1、如果田忌最快的马比齐王最快的马快，则比之

2、如果田忌最快的马比齐王最快的马慢，则用田最慢的马跟齐最快的马比  //这是贪心的第一步

3、如果田忌最快的马的速度与齐威王最快的马速度相等

3.1、如果田忌最慢的比齐威王最慢的快，则比之                         //这是贪心的第二步

3.2、如果田忌最慢的比齐威王最慢的慢，田忌慢VS齐王快

3.3、田忌最慢的与齐威王最慢的相等，田忌慢VS齐王快

***

### Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e3;
int a[N + 5], b[N + 5];
int main()
{
    int n;
    while(cin >> n, n) {
        for(int i = 0; i < n; ++i) scanf("%d", a + i);
        for(int i = 0; i < n; ++i) scanf("%d", b + i);
        sort(a, a + n, greater<int>());
        sort(b, b + n, greater<int>());
        int l1, l2, r1, r2, tot;
        int cnt = 0;
        l1 = l2 = tot = 0;
        r1 = r2 = n - 1;
        while(true) {
            if(cnt == n) break;
            //如果田忌最快的比齐王最快的慢
            if(a[l1] < b[l2]) {
                tot -= 200;
                l2++, r1--;
            }
            //如果田忌最快和齐王最快相等
            else if(a[l1] == b[l2]) {
                if(a[r1] > b[r2]) {
                    tot += 200;
                    r1--, r2--;
                }
                else {
                    if(a[r1] < b[l2]) tot -= 200;
                    r1--, l2++;
                }
            }
            //如果田忌最快的比齐王最快的快
            else {
                tot += 200;
                l1++, l2++;
            }
            cnt++;
        }
        cout << tot << endl;
    }
    return 0;
}
```

