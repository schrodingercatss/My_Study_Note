#POJ1733：**Parity game**
***
### 题目传送门

[poj1733 ：Parity game](<http://poj.org/problem?id=1733>)

***

### 解题思路

这道题是一道典型的带权并查集和扩展域并查集的题，带权并查集顾名思义就是连接两个点之间的边有一个权值，而扩展域并查集就如同食物链那道题的解法一般。

因为这题的$N <=10^9$所以，不能直接开数组去存，这让我们自然而然的联想到了离散化处理。

法一：

首先讲带权并查集的思路。因为题目要求的是奇偶性，可知，一个01数列中从第l个元素到第r个元素之间有奇数个1，如果令sum[]为前缀和，则等价于sum[l-1]\^sum[r]=1​;偶数个1的情况就是sum[l-1]^sum[r]=0;这个应该不难理解。当然这个题目中我们不需要求出sum[]具体是多少，只需要知道它们之间的关系就行了。

　　那么我们就可以设置并查集了，设置一个d[]数组，表示x与fa[x]之间的关系（也就是sum[x]\^sum[fa[x]]）。每次询问时，先令x=l-1,y=r,找到fa[x]与fa[y]，再判断一下，如果x,y在同一并查集里，那么就直接看d[x]^d[y]是否等于所询问的关系，如果不满足，直接输出答案然后退出;否则，说明目前还不知道x,y之间的关系，那就将x,y合并到同一个并查集。在操作过程中，维护d[]数组即可。

法二：

再来讲讲扩展域并查集的思路。这种方法我个人觉得会比较好理解。基本思路也是和上面一样用前缀和之间的关系来判断。把每个变量x变成两个x_odd和x_even，也就是奇偶两种情况。对于每一个询问，用0代表偶数，1代表奇数，那么对于每个询问只有以下情况：

　　1,询问为0：判断x_odd与y_even是否在同一集合中（或者判断x_even和y_odd也一样，都是等价的，原因后面会说），如果在同一集合，则矛盾，直接输出答案;否则，合并x_odd与y_odd，x_even与y_even，表示sum[x]与sum[y]同奇偶。

　　2,询问为1：判断x_odd与y_odd是否在统一集合中（当然判断x_even与y_even也一样），如果在统一集合，则矛盾，直接输出答案;否则，合并x_odd与y_even，x_even与y_odd，表示sum[x]与sum[y]异奇偶。

　　可知，因为合并的时候是将两种情况一起合并的，所以两种询问是等价的。

***

```cpp
//法一
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <queue>
using namespace std;
const int N = 2e4 + 5;
int n, m, ans, cnt;
int a[N], d[N], bcj[N];
struct node {
    int l, r, w;
} p[N];

int Find(int x)
{
    if (bcj[x] < 0)
        return x;
    int root = Find(bcj[x]);  
    d[x] ^= d[bcj[x]]; //路径压缩：x的父结点到根的情况异或上x到父结点的情况就是x到根的情况
    return bcj[x] = root;
}

int main()
{
    memset(bcj, -1, sizeof bcj);
    char str[5];
    scanf("%d %d", &n, &m);
    for(int i = 1; i <= m; ++i) {
        scanf("%d %d %s", &p[i].l, &p[i].r, str);
        p[i].w = (str[0] == 'e' ? 0 : 1);
        a[++cnt] = p[i].l - 1;
        a[++cnt] = p[i].r;
    }
    sort(a + 1, a + cnt + 1);
    n = unique(a + 1, a + cnt + 1) - a - 1;
    
    for(int i = 1; i <= m; ++i) {
        int x = lower_bound(a + 1, a + n + 1, p[i].l - 1) - a;
        int y = lower_bound(a + 1, a + n + 1, p[i].r) - a;
        int fx = Find(x), fy = Find(y);
        if(fx == fy) {
            if(d[x] ^ d[y] != p[i].w) {
                printf("%d\n", i - 1);
                return 0;
            }
        }
        else {
            bcj[fy] = fx;
            d[fy] = d[x] ^ d[y] ^ p[i].w;
        }
    }
    printf("%d\n", m);
    return 0;
}
```

```cpp
//法二
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <queue>
using namespace std;
const int N = 4e4 + 5;
int n, m, ans, cnt;
int a[N], d[N], bcj[N];
struct node {
    int l, r, w;
} p[N];

int Find(int x)
{
    return bcj[x] < 0 ? x : Find(bcj[x]);
}

void Unite(int x, int y)
{
    x = Find(x), y = Find(y);
    if(x == y) return ;
    bcj[y] = x;
}
int main()
{
    memset(bcj, -1, sizeof bcj);
    char str[5];
    scanf("%d %d", &n, &m);
    for(int i = 1; i <= m; ++i) {
        scanf("%d %d %s", &p[i].l, &p[i].r, str);
        p[i].w = (str[0] == 'e' ? 0 : 1);
        a[++cnt] = p[i].l - 1;
        a[++cnt] = p[i].r;
    }
    sort(a + 1, a + cnt + 1);
    n = unique(a + 1, a + cnt + 1) - a - 1;
    
    for(int i = 1; i <= m; ++i) {
        int x = lower_bound(a + 1, a + n + 1, p[i].l - 1) - a;
        int y = lower_bound(a + 1, a + n + 1, p[i].r) - a;
        int x1 = x, x2 = x + n;
        int y1 = y, y2 = y + n;
        if(p[i].w == 0) {
            if(Find(x1) == Find(y2)) {  //x_odd，y_even为偶数在一个集合里面则非法
                printf("%d\n", i - 1);
                return 0;
            }
            Unite(x1, y1);  //合并x_odd,y_odd
            Unite(x2, y2);  //合并x_even, y_even
        }
        else {
            if(Find(x1) == Find(y1)) {
                printf("%d\n", i - 1);
                return 0;
            }
            Unite(x2, y1);  //合并x_even, y_odd
            Unite(x1, y2);  //合并x_odd,y_even
        }
    }
    printf("%d\n", m);
    return 0;
}
```

