## POJ：2924  **A Bug's Life**

### 题目传送门

[poj:2924 **A Bug's Life**](<http://poj.org/problem?id=2492>)

***

### 解题思路

这题是一个典型的种类并查集问题，之前做过了食物链那道并查集的题目，很快就想到了这题的解题思路。

法一：用某点到根节点的距离的奇偶来判断性别，这里可以用0和1来代替性别，防止数据太大超过int类型。

法二：使用跟白书上食物链的解题方法一样，开两倍大小的数组，并查集里面合并同一性别的昆虫, 用n+x表示与x性别相反的昆虫。

***

### Code

在Find中取余是为了进行路径压缩，而Unite取余是为了更新合并后的点离根节点的距离。

```cpp
//法一
#include <iostream>
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 2005;
int bcj[N + 5], dis[N + 5];

inline void init()
{
    memset(bcj, -1, sizeof bcj);
    memset(dis, 0, sizeof dis);
}

int Find(int x)
{
    if(bcj[x] < 0) return x;
    int pre = bcj[x];
    bcj[x] = Find(bcj[x]);
    dis[x] = (dis[x] + dis[pre]) % 2; //路径压缩
    return bcj[x];
}

bool Union(int x, int y)
{
    int rx = Find(x), ry = Find(y);
    if(rx == ry) {
        if((dis[x] + dis[y]) % 2 == 0) return true;
        else return false;
    }
    else {
        bcj[ry] = rx;
        dis[ry] = (dis[x] + dis[y] + 1) % 2;
    }
    return false;
}

int main()
{
    int T, n, m, a, b;
    scanf("%d", &T);
    for(int t = 1; t <= T; ++t) {
        init();
        bool flag = false;
        scanf("%d %d", &n, &m);
        for(int i = 0; i < m; ++i) {
            scanf("%d %d", &a, &b);
            if(Union(a, b)) flag = true;
        }
        printf("Scenario #%d:\n", t);
        if(flag) puts("Suspicious bugs found!");
        else puts("No suspicious bugs found!");
        puts("");
    }
    return 0;    
}


```

***

假设输入数据为a和b，不妨设a为雄性，则a+n为雌性，b为雌性，b+n为雄性，这样我们把同性的进行合并，也就是合并a和b+n，b和a+n。这样我们每次输入只需要判断a和b或者a+n和b+n是否在一个集合里即可，又因为我们是同时处理加了n和不加n的情况，所以只需要判断一种情况即可。

```cpp
//法二
#include <iostream>
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 2005;
int bcj[N * 2];

int Find(int x) 
{
    return bcj[x] < 0 ? x : bcj[x] = Find(bcj[x]);
}

void Unite(int x, int y)
{
    x = Find(x), y = Find(y);
    if(x == y) return;
    bcj[y] = x;
}

int main()
{
    int T, n, m, a, b;
    scanf("%d", &T);
    for(int t = 1; t <= T; ++t) {
        scanf("%d %d", &n, &m);
        for(int i = 0; i < 2 * N; ++i) bcj[i] = -1;
        int flag = false;
        for(int i = 0; i < m; ++i) {
            scanf("%d %d", &a, &b);
            if(Find(a) == Find(b)) flag = true;
            else {
                Unite(a, b + n);
                Unite(a + n, b);
            }
        }
        if(flag)  printf("Scenario #%d:\nSuspicious bugs found!\n\n", t);
        else printf("Scenario #%d:\nNo suspicious bugs found!\n\n", t);
    }
    return 0; 
}
```

