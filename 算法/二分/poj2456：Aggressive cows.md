## poj2456：Aggressive cows

### 题目传送门

[poj2456：Aggressive cows](http://poj.org/problem?id=2456)

***

### 解题思路

一般类似最大化最小值或者最小化最大值的问题，通常使用二分搜索法来解决。我们使用二分来确定d的值即可。

***

### Code

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 1e5;
const int INF = 0x3f3f3f3f;
int arr[N + 5];
int n, m;


bool judge(int d)
{
    int last = 0;
    for(int i = 1; i < m; ++i) {
        int now = last + 1;
        while(now < n && arr[now] - arr[last] < d) now++;
        if(now == n) return false;
        last = now;
    }
    return true;
}


void slove()
{
    sort(arr, arr + n);
    int l = 0, r = INF;
    while(l < r) {
        int mid = l + r + 1 >> 1;
        if(judge(mid)) l = mid;
        else r = mid - 1;
    }
    cout << l << endl;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);cout.tie(0);
    cin >> n >> m;
    for(int i = 0;i < n; ++i) cin >> arr[i];
    slove();
    return 0;
}
```

