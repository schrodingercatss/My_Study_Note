### Problem A：Triangle
**题目传送门**：http://codeforces.com/contest/6/problem/A

**题意**：水题，给你4根木棍，让你判断是否能组成三角形或者退化三角形。

注：退化三角形指两边之和等于第三边的极端情况。

**解题思路**： 因为数据范围小，直接暴力即可，附上dalao的神器做法。


**易错点**：无。

**代码**：
```cpp
#include <bits/stdc++.h>
using namespace std;
int a[4];
int check(int x, int y, int z)
{
    if (x == y)
        return 0;
    if (y == z)
        return 0;
    if (x == z)
        return 0;
    x = a[x], y = a[y], z = a[z];
    if (x + y > z && x + z > y && y + z > x)
        return 2;
    if (x + y == z || y + z == x || x + z == y)
        return 1;
    return 0;
}
int main()
{
    for (int i = 0; i < 4; i++)
        cin >> a[i];
    for (int i = 0; i < 4; i++)
        for (int j = 0; j < 4; j++)
            for (int k = 0; k < 4; k++)
                if (check(i, j, k) == 2)
                    return puts("TRIANGLE"), 0;
    for (int i = 0; i < 4; i++)
        for (int j = 0; j < 4; j++)
            for (int k = 0; k < 4; k++)
                if (check(i, j, k) == 1)
                    return puts("SEGMENT"), 0;
    return puts("IMPOSSIBLE"), 0;
}
```
**大神的思路：**
先把四根木棍的长度按从小到大排序。

从四根木棍中挑出三根显然只有 4 种方法（如下）

1 2 3
1 2 4
1 3 4
2 3 4
排列组合就是：$C_{4}^{3}= \frac{4!}{\left ( 4-3 \right )!3!}= \frac{4\times 3\times 2\times 1}{3\times 2\times 1}= 4$

其中有 2 种不需要考虑，原因如下：

1. 如果第一根木棍和第二根木棍的总长大于第四根，那么就一定大于第三根，所以只要判断第一根木棍和第二根木棍的总长与第三根的关系。
2. 同理，如果第一根木棍和第三根木棍的总长大于第四根，那么第二根木棍和第三根木棍的总长也一定大于第四根，所以只要判断第二根木棍和第三根木棍的总长与第四根的关系。

也就是说，2,3 两种挑法可以忽略，退化的三角形也一样判断。
```cpp
#include <bits/stdc++.h>
using namespace std;
int f[5];//数组存比较方便排序
int main(){
    for ( int i = 1; i <= 4; i++ )
    scanf ( "%d", &f[i] );
    sort ( f + 1, f + 5 );//快速排序
    if ( f[1] + f[2] > f[3] || f[2] + f[3] > f[4] ) printf ( "TRIANGLE\n" );
    else if ( f[1] + f[2] == f[3] || f[2] + f[3] == f[4] ) printf ( "SEGMENT\n" );
    else printf ( "IMPOSSIBLE\n" );
    return 0;
} 
```
***
### Problem B：President's Office
**题目传送门**：http://codeforces.com/contest/6/problem/B

**题意**：给你n,m,c表示n行m列的矩阵，c是主人公的桌子颜色，现在与主人公桌子接触的人的桌子是主人公的小弟，问你这个主人公有多少个小弟。

**解题思路**： 直接暴力即可，如果碰到boss的桌子，就向上下左右判断有没有未出现过的颜色。

**易错点**：无。

**代码**：
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 105;
char maze[N][N];
bool vis[N][N];
int col[N];  // 记录已经出现过的颜色
int dist[4][2] = {1, 0, -1, 0, 0, 1, 0, -1};

int ans, n, m;
char ch;

void dfs(int x, int y){
    for(int i = 0; i < 4; ++i) {
        int dx = x + dist[i][0];
        int dy = y + dist[i][1];
        if(dx >= 0 && dx < n && dy >= 0 && dy < m) {
            if(col[maze[dx][dy]] == 0 && maze[dx][dy] != '.') {
                ans++;
                col[maze[dx][dy]] = 1;
            }
        }
    }
}

int main()
{
    cin >> n >> m >> ch;
    col[ch] = 1;

    for(int i = 0; i < n; ++i) cin >> maze[i];

    for(int i = 0; i < n; ++i)
        for(int j = 0; j < m; ++j) {
            if(maze[i][j] == ch) {
                dfs(i, j);
            }
        }
    cout << ans << endl;
    return 0;
}
```

***
### Problem C：
**题目传送门**：http://codeforces.com/contest/6/problem/C

**题意**：两个人吃一排巧克力，吃每个巧克力都要花费不同的时间，第一个人从左往右吃，第二个人从右往左吃，并且如果只剩一个巧克力且两人同时吃时，让给第一个人吃，问两人一共吃了多少个巧克力。

**解题思路**： 贪心双指针算法即可，每次让第一个人优先选，这样便可以实现女士优先的条件。

**易错点**：无

**代码**：
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5 +10;
int arr[N];

int main()
{
    int n;
    cin >> n;
    for(int i = 1; i <= n; ++i) cin >> arr[i];

    int i = 1, j = n;
    int t1 = 0, t2 = 0;
    while(i <= j) {
        if(t1 <= t2) t1 += arr[i++]; //用时相等即两人刚好要吃相同的巧克力时，女士优先，男士靠边。
        else t2 += arr[j--];
    }
    cout << i - 1 << " " << n - j << endl;
    return 0;
}
```
***
### Problem D
**题目传送门**：：http://codeforces.com/contest/6/problem/D

**题意**：

**解题思路**： 

方法一：因为数据量较小，直接暴力dfs即可，每次保证当前位置左边的目标全部被消灭即可

方法二：dp，题解待完善。

**易错点**：不能直接打1和n,只能用溅射攻击来消灭他们。

**代码**：
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 20;
int h[N];
int ans = 1 << 30;
vector<int> T, T2;
int n, a, b;

void dfs(int cur, int times)
{
    if(times >= ans) return ;
    if(cur == n) {
        if(h[cur] < 0) {
            T2 = T;
            ans = times;
        }
        return ;
    }
    for(int i = 0; i <= max(h[cur - 1] / b + 1, max(h[cur] / a + 1, h[cur + 1] / b + 1)); ++i) {
        if(h[cur - 1] < b * i) {
            h[cur - 1] -= b * i, h[cur] -= a * i, h[cur + 1] -= b * i;
            for(int j = 0; j < i; ++j) T.push_back(cur); // 把攻击当前目标的次数逐一压入vector

            dfs(cur + 1, times + i);

            for(int j = 0; j < i; ++j) T.pop_back();
            h[cur -1] += b * i, h[cur] += a * i, h[cur + 1] += b * i;
        }
    }
}

int main()
{
    cin >> n >> a >> b;
    for(int i = 1; i <= n; ++i) cin >> h[i];
    dfs(2, 0);
    cout << ans << endl;
    for(auto &x: T2) cout << x << " ";
    cout << endl;

    return 0;
}
```
***
### Problem E：
**题目传送门**：http://codeforces.com/contest/6/problem/E

**题意**：给你一堆数，让你找到满足最大值和最小值小于等于k的区间，第一行输出区间长度和区间个数，接下来n行输出每个区间的左端点和右端点。

**解题思路**：类似于双端队列的写法，我们用multiset来处理这个问题，如果当前区间不合法，那么我们不断删去起始端的数就好了。还可以使用二分 + RMQ 或者线段树来解决。

**易错点**：无。

**代码**：
***
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> Pii;

const int N = 1e5 + 10;
int arr[N];

vector<Pii> vec;

int main()
{
    int n, k;
    cin >> n >> k;
    for(int i = 0; i < n; ++i) cin >> arr[i];
    multiset<int> mmp;
    int j = 0, len = -1;
    for(int i = 0; i < n; ++i) {
        mmp.insert(arr[i]);
        while(*mmp.rbegin() - *mmp.begin() > k) mmp.erase(mmp.find(arr[j++]));
        if(i - j + 1 > len) {
            len = i - j + 1;
            vec.clear();
        }
        if(i - j + 1 == len) {
            vec.push_back({j + 1, i + 1});
        }
    }
    cout << len << " " << vec.size() << endl;
    for(auto &p: vec) cout << p.first << " " << p.second << endl;

    return 0;
}
```