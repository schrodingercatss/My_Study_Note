### Problem A：
**题目传送门**：http://codeforces.com/contest/4/problem/A

**题意**：现在给你一个数n，让你判断它是否能分解成2个偶数。

**解题思路**： 因为奇数 = 奇数 + 偶数，所以奇数不可能实现，而偶数中有一个特殊的数2，只能分解成2个奇数相加，所以要进行特判。

**易错点**：遗漏2这种特殊的情况。

**代码**：
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    int n;
    cin >> n;
    puts(n == 2 || n & 1 ? "NO" : "YES");
    return 0;
}
```
***
### Problem B：
**题目传送门**：http://codeforces.com/contest/4/problem/B

**题意**：给定d和sum，输入d行，每行两个数，判断在每行中两数范围之内取一个数相加最终结果能否为sum。

**解题思路**： 在保证合法的情况下贪心。首先，我们得到最小值和最大值的和s1和s2，然后我们对sum进行判断，如果$sum < s1$，或者$sum > s2$，属于无解情况，如果合法，我们先把sum减去s1,然后每次默认选最大的，如果无法选择最大的，就从区间中挑一个合适的数，直到合乎题意。

**易错点**：没有对sum进行特判，没有搞清楚选择合适数的方法。

**代码**：
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1005;
int a[N], b[N];
int main()
{
    int n, sum;
    cin >> n >> sum;
    int s1 = 0, s2 = 0;
    for(int i = 0; i < n; ++i) {
        cin >> a[i] >> b[i];
        s1 += a[i], s2 += b[i];
    }
 
    if(s1 > sum || s2 < sum) puts("NO");
    else {
        puts("YES");
        sum -= s1;
        for(int i = 0; i < n; ++i) {
            if(b[i] - a[i] < sum) {
                printf("%d ", b[i]); // 优先安排大的
                sum -= b[i] - a[i];
            }
            else {
                printf("%d ", sum + a[i]);
                sum = 0;
            }
        }
    }
    return 0;
}

```
***
### Problem C：
**题目传送门**：http://codeforces.com/contest/4/problem/C

**题意**：给你一堆字符串，让你判断是否出现过，如果没出现过输出OK，出现过输出字符串+i，i为之前出现次数，比如abc, abc,第一次输出OK，第二次输出abc

**解题思路**： 使用c++的map和count函数即可完成。

**易错点**：无

**代码**：
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main()
{
    unordered_map<string, int> mmp;
    int n;
    string str;
    cin >> n;
    while(n--) {
        cin >> str;
        if(mmp.count(str) == 0) {
            puts("OK");
            mmp[str] = 0;
        }
        else {
            cout << str << ++mmp[str] << endl;
        }
    }
    return 0;
}
```
***
### Problem D：
**题目传送门**：http://codeforces.com/contest/4/problem/D

**题意**：你有一张长宽为x,y的卡片，你有n个盒子，长宽分别为$x_i$,$y_i$。然后问你卡片最多塞多少层盒子。并且把这些盒子按照从里到外输出。

**解题思路**： 因为数据量比较小，我们可以直接使用记忆化搜索即可，如果数据量较大，我们要使用线段树。

**易错点**：无。

**代码**：
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 5005;
int x[N], y[N], dp[N];
int from[N], n;

int dfs(int t)
{
    if(dp[t]) return dp[t];
    for(int i = 1; i <= n; ++i) {
        if(x[t] < x[i] && y[t] < y[i])
            if(dfs(i) + 1 > dp[t]) {
                from[t] = i;
                dp[t] = dfs(i) + 1;
            }
    }
    return dp[t];

}



int main()
{
    cin >> n;
    for(int i = 0; i <= n; ++i) cin >> x[i] >> y[i];
    int ans = dfs(0);
    cout << ans << endl;
    for(int i = from[0]; i; i = from[i]) cout << i << " ";
    puts("");

    return 0;
}
```
