#POJ1456：Supermarket
***
### 题目传送门

[poj1456 ：Supermarket](<http://poj.org/problem?id=1456>)

***

### 解题思路

这是一道典型的贪心问题，题目大意为：给你n个商品，每个商品都有自己的价值和保质期，请你设计一个出售方法，使这n个商品出售的价值最大。

法一：对保质期从小到大排序，然后直接使用优先队列即可解决。

法二：使用并查集进行查找优化。

***

### Code

```cpp
//法一
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <queue>
using namespace std;
const int N = 1e4;
struct node {
    int val, ed;
    inline bool operator < (const node &x) const {
        return ed < x.ed;
    }
} arr[N + 5];

int main()
{
    int n, t;
    while(cin >> n) {
        for(int i = 0; i < n; ++i) {
            cin >> arr[i].val >> arr[i].ed;
        }
        sort(arr, arr + n);
        priority_queue<int, vector<int>, greater<int> > q;
        int cur = 1;
        for(int i = 0; i < n; ++i) {
            if(cur <= arr[i].ed) {
                q.push(arr[i].val);
                cur++;
            }
            else {
                if(arr[i].val > q.top()) {
                    q.pop();
                    q.push(arr[i].val);
                }
            }
        }
        int ans = 0;
        while(q.size()) ans += q.top(), q.pop();
        cout << ans << endl;
    }
    return 0;
}
```

开始做这道题的时候并没有想到可以使用并查集来进行维护。后来看了dalao的题解才发现可以用它来优化查找。

**并查集思路：**

首先按照价值排序，然后遍历找到可以最早进行该任务的时间，用并查集来合并这个时间并且找到可以执行这个时间的最晚时间（也就是在这一步贪心了），因为对于这个任务，肯定越晚完成越好，因为可以留更多的时间给其他的任务。然后如果实在找不到时间那这个任务不能选了。因为有其他更优秀的任务占据了这个时间，整个过程用并查集维护。

先安排价格高的商品，最好是放在保质期d那一天，如果已经被占（商品价格肯定比它高），往前找。类似并查集找祖宗的过程。注:bcj[i]只能初始化为-1；如果初始化为0，当保质期为一天有多个商品时，会出现重复计算。

```cpp
//法二
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <queue>
using namespace std;
const int N = 1e4;
int bcj[N + 5];

struct node {
    int val, ed;
    inline bool operator < (const node &x) const {
        return val > x.val;
    }
} arr[N + 5];

int Find(int x)
{
    if(bcj[x] < 0) return x;
    return bcj[x] = Find(bcj[x]);
}
int main()
{
    int n, t;
    while(cin >> n) {
        memset(bcj, -1, sizeof bcj); //这里只能初始化为-1，因为天数是不连贯的。
        for(int i = 0; i < n; ++i) {
            cin >> arr[i].val >> arr[i].ed;
        }
        sort(arr, arr + n);
        int sum = 0;
        for(int i = 0; i < n; ++i) {
            int  t = Find(arr[i].ed); //找到一个未被占用的时间且为最晚的时间
            if(t > 0) {  //如果t小于等于0，说明找不到可以存放的位置了，只能舍弃了
                sum += arr[i].val;
                bcj[t] = t - 1;  // t天被占用后，它的祖宗应该指向前一个单位时间，也就是t-1
            }
        }
        cout << sum << endl;
    }
    return 0;
}
```

