 ### Problem A：Chat Servers Outgoing Traffic
**题目传送门**：http://codeforces.com/contest/5/problem/A

**题意**：现在有一个聊天室，给你一堆传输的指令，`+`表示ADD命令，`-`表示Remove命令，`<name>:<message>`表示Send命令，Send命令会向所有在线用户(包含发送者自己)发送$l$个字节的数据，其他命令不传输数据，让你求总传输的数据。

**解题思路**： 直接模拟即可。

**易错点**：无。

**代码**：
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    string str;
    unordered_set<string> st;
    int tot = 0;
    while(getline(cin, str)) {
        if(str[0] == '+') st.insert(str.substr(1));
        else if(str[0] == '-') st.erase(str.substr(1));
        else {
            int i = 0, cnt = 0;
            while(str[i] != ':') ++i;
            while(str[++i]) ++cnt;
            tot += cnt * st.size();
        }
    }
    cout << tot << endl;
    return 0;
}
```
更一步简化的思路，因为题目保证命令合法，也就是说不需要使用set或者map来存储用户是否在线，只需要记录在线人数即可。

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    char buf[1005];
    int tot = 0, now = 0;
    while(gets(buf)) {
        if(buf[0] == '+') now++;
        else if(buf[0] == '-') now--;
        else {
            int pos = -1;
            for(int i = 0; buf[i]; ++i) {
                if(buf[i] == ':') {
                    pos = i;
                    break;
                }
            }
            tot += now * (strlen(buf) - pos - 1);
        }
    }
    cout << tot << endl;
    return 0;
}
```

***
### Problem B：

**题目传送门**：http://codeforces.com/contest/5/problem/B

**题意**：给出一些字符串，让你用 * 号组成的最小矩形把这些字符串围起来，不足的补充空格，所有字符串中间对齐，如果出现长度的奇偶性不同，那就优先空着左边，下次再优先空着右边，输出最终处理的结果



**解题思路**： 直接模拟即可。

**易错点**：如果长度奇偶性不同，要一左以右的输出。

**代码**：
```cpp
#include <bits/stdc++.h>
#pragma GCC optimize(2)
using namespace std;
typedef long long ll;
int main()
{
    vector<string> vec;
    string str;
    int max_len = 0;
    while(getline(cin, str, '\n')) {
        vec.push_back(str);
        max_len = max(max_len, (int)str.size());
    }
    for(int i = 0; i < max_len + 2; ++i) putchar('*');
    puts("");
    bool flag = true;
    for(int i = 0; i < vec.size(); ++i) {
        putchar('*');
        int t = max_len - vec[i].size();
        int left = 0, right = 0;    
        if(t & 1 && flag) left = (t - 1) / 2, right = (t + 1) / 2, flag = false;
        else if(t & 1) left = (t + 1) / 2, right = (t - 1) / 2, flag = true;
        else left = right = t / 2;
        for(int j = 0; j < left; ++j) putchar(' ');
        cout << vec[i];
        for(int j = 0; j < right; ++j) putchar(' ');
        puts("*");
    }
    for(int i = 0; i < max_len + 2; ++i) putchar('*');
    puts("");

    cout << endl;
}
```

**dalao的代码:**
```cpp
#include <algorithm>
#include <iostream>
#include <cstdio>
#include <string>
#include <vector>
 
using namespace std;
 
vector<string> data;
 
int main() {
    for (;;) {
        char buf[1000];
        if (!gets(buf)) break;
        data.push_back(buf);        
    }
    int max_len = 0;
    for (int i = 0; i < data.size(); i++) {
        max_len = max(max_len, (int)data[i].size());
    }
    cout << string(max_len + 2, '*') << "\n";
    int flag = 0;
    for (int i = 0; i < data.size(); i++) {
        int l1 = (max_len - data[i].size()) / 2;
        int l2 = (max_len - data[i].size()) / 2;
        if (l1 + l2 + data[i].size() != max_len) {
            if (!flag) l2++;
            else l1++;
            flag = 1 - flag;
        }
        cout << "*" << string(l1, ' ') << data[i] << string(l2, ' ') << "*\n";
    }
    cout << string(max_len + 2, '*') << "\n";
    return 0;
}
```
***
### Problem C：
**题目传送门**：http://codeforces.com/contest/5/problem/C

**题意**：给你一个括号序列，让你找到最长的连续的合法括号序列,然后让你输出这个括号序列的长度是多少,这么长的括号序列一共有多少个。

**解题思路**： 这是一个典型的区间$dp$问题，设$dp[i]$表示位置为i的右括号结尾的最长合法括号子序列的长度，则易得：
$dp[i]=dp[tmp-1]+i-tmp+1$，其中tmp表示与位置为i的右括号匹配的左括号的位置（可以用栈记录）。这样我们用栈和dp就解决了这个问题。

**易错点**：无

**代码**：
```cpp
#include <bits/stdc++.h>
#pragma GCC optimize(2)
using namespace std;
const int N = 1e6 + 10;
int dp[N];
int main()
{
    string s;
    stack<int> stk;
    cin >> s;
    for(int i = 0; i < s.size(); ++i) {
        if(s[i] == '(') stk.push(i);
        else {
            if(stk.size()) {
                dp[i] = i - stk.top() + 1;
                if(stk.top() > 0) dp[i] += dp[stk.top() - 1];
                stk.pop();
            }
        }
    }
    int ans1 = 0, ans2 = 1;
    for(int i = 0; i < s.size(); ++i) {
        if(dp[i] > ans1) {
            ans1 = dp[i];
            ans2 = 1;
        } else if(dp[i] == ans1) ans2++;
    }
    if(ans1 == 0) puts("0 1");
    else cout << ans1 << " " << ans2 << endl;

    return 0;
}
```
***
### Problem D：
**题目传送门**：http://codeforces.com/contest/5/problem/D

**题意**：物理题，有一辆车从出发点发车到另一个距离为$l$的城市，车速最大是$v$，中途有一个点是限速点，速度不能超过$w$，让你求到达的最短时间。

**解题思路**： 结合物理公式，分情况讨论就行了。
匀速运动时：$v=\frac s t$

匀变速时：$v=v_0+at$

位移公式：$x=v_0t+\frac {at^2}2$

位移速度关系式：$v^2-v_0^2=2ax$

注： $v_1 = sqrt(2 * a * x + w * w)$的推导过程如下：

这种情况是未加速到v即到达了目的地，设到达时速度为$v_1$

由$v_1^2 - w^2 = 2ax$，得$v_1 = sqrt(2 * a * x + w * w)$

**易错点**：无。

**代码**：
```cpp
#include <bits/stdc++.h>
#define sqr(x) (x) * (x)
#pragma GCC optimize(2)
using namespace std;
 
double f(double w, double x, double a, double v)
{
    double x2 = (v * v - w * w) / (2 * a); // 加速运动到v的位移
    if(x2 <= x) {
        double t1 = (x - x2) / v; // 未到达则进行匀速运动
        return (v - w) / a + t1;  //平均速度公式
    }
    // 如果未加速到v就到达了
    double v1 = sqrt(2 * a * x + w * w); 
    return (v1 - w) / a;
}
 
int main()
{
    double a, v, l, d, w;
    cin >> a >> v >> l >> d >> w;
    double t = sqrt((2 * d) / a);
    double V = min(a * t, v);
    if(V <= w) {
        printf("%.5f\n", f(0, l, a, v));
    }else {
        double x1 = v * v / (2 * a); // 加速阶段位移
        double x2 = (v * v - w * w) / (2 * a); // 减速阶段位移
        if(x1 + x2 <= d) {
            double t3 = (d - x1 -x2) / v; // 匀速阶段时间
            printf("%.10f\n", v / a + (v - w) / a + t3 + f(w, l - d, a, v)); // (v - w) / a为平均速度公式
        } else {
            double v1 = sqrt(a * d + w * w / 2);
            double t1 = v1 / a, t2 = (v1 - w) / a; // 平均速度公式
            printf("%.10f\n", t1 + t2 + f(w, l - d, a, v));
        }
    }
 
    return 0;
}

```

***
### Problem E：
**题目传送门**：http://codeforces.com/contest/5/problem/E

**题意**：给一个环，环上有n个点，如果每两个点之间没有比这两个点大的点，则这两个点能连一条弧，问最多能连几条弧。

**解题思路**： 
假设对于i，我们已经计算出了$left[i]$, $right[i]$, $same[i]$，其中$left[i]$表示i左侧比第一个比i高的位置，$right[i]$表示`i`右侧第一个比i高的位置，$same[i]$表示从`i`到$right[i]$的左开右闭区间内高度等于i的山的数目。

简而言之，left和right是位置，而same是数目。

那么对于一座山i而言，它可以和$left[i]$ 和 $right[i]$都构成能互相看见的pair，并且和`i`到$right[i]$之间所有高度等于i的山构成能互相看见的pair。

所以问题就是计算left数组、right数组和same数组。

我们对山的高度数组进行旋转，选取最高的山开始，因为如果最高的山在中间，任意两点存在连线的话必然是不会经过最高的山的，我们对山拆环成链然后使用retate函数或者取余的方式进行旋转，然后在数组末尾让$H[n] = H[0]$,这样就完成了拆环成链。

然后我们分别计算三个数组即可。对于两个不相等的点，由于都是找比自己大的，就避免了由大到小又由小到大重复两次。对于相等的点，每增加一个相等的点，就相当于增加了之前点的数目的对数（开始是1个点0对）。这样也是不重复的。把这些对加起来。


**易错点**：当$L[i] == 0$, $R[i] = n$的情况下，这个时候显然只有一座最高的山，所以我们需要对ans做-1操作。

**代码**：
```cpp
#include <bits/stdc++.h>
#define sqr(x) (x) * (x)
#pragma GCC optimize(2)
#define speed() ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
using namespace std;
typedef long long ll;
const int N = 1e6 + 10;
int H[N], L[N], R[N], C[N];
int main()
{
    speed();
    int n;
    cin >> n;
    for(int i = 0; i < n; ++i) cin >> H[i];
    rotate(H, max_element(H, H + n), H + n);  // 拆环成链，把高度最高的山旋转到第一位，可以使用取余实现
    H[n] = H[0]; // 断口处多复制一份

    for(int i = n - 1; i >= 0; --i) {
        R[i] = i + 1;
        while(R[i] < n && H[R[i]] < H[i]) R[i] = R[R[i]];

        if(R[i] < n && H[R[i]] == H[i]) {
            C[i] = C[R[i]] + 1;
            R[i] = R[R[i]];
        }
    }

    ll ans = 0;
    for(int i = 0; i < n; ++i) {
        ans += C[i];
        if(H[i] == H[0]) continue;
        L[i] = i - 1;
        // L[i] > 0这个条件可以不用加，因为上面的if条件以及限制了L[i]肯定大于0
        while(L[i] > 0 && H[L[i]] < H[i]) L[i] = L[L[i]];
        ans += 2;
        // 这个条件必须加，因为只有L[i] == 0 R[i] == n才代表是最高山峰，这种情况会重复计算一次
        if(L[i] == 0 && R[i] == n) ans --;
    }
    cout << ans << endl;

    return 0;
}

```
