### Problem A：
**题目传送门**：http://codeforces.com/contest/3/problem/A

**题意**：给定一个棋盘，给定起始坐标和终点坐标，每次能从8个方向移动，让你求一条最短的路径。

**解题思路**： 我们可以根据终点与起点的x和y坐标得知，x坐标决定是向左还是向右走，y坐标决定向上还是向下走。

**易错点**：输出的时候注意x和y的差值要取绝对值，别忘记换行，别搞错方向。

**代码：**
```cpp
#include<bits/stdc++.h>
using namespace std;
char a[3], b[3];
int x, y, c, d;
int main()
{
    gets(a), gets(b);
    x = (a[0] - b[0]), y = (a[1] - b[1]);
    c = x < 0 ? x = -x, 'R' : 'L';
    d = y < 0 ? y = -y, 'U' : 'D';
    printf("%d\n", max(x, y));
    while(x || y) {
        if(x) x--, putchar(c);
        if(y) y--, putchar(d);
        puts("");
    }
    return 0;
}
```
***
### Problem B：lorry
**题目传送门**：http://codeforces.com/contest/3/problem/B

**题意**：现在有一辆卡车容积为v，有两种物品，物品1占用1个单位空间，物品2占用2个单位空间，不同的物品价值不一样，让你最大限度的利用v，使得价值最大。

**解题思路**： 这题因为数据量很大,如果当成背包问题时间复杂度为$O(n * v)$，如果我们按性价比来排序，则可能造成最后剩余空间无法装下任何物品的情况，造成空间浪费。我们可以用两个容器来存物品1和物品2，然后分别处理，先装物品1，然后再用物品2替换物品1，知道找到一个价值最大的情况。

**易错点**：不能直接用性价比去排序，注意别遗漏排序的比较方法。

**代码：**
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
const int N = 1e5 + 10;

vector<pii> vec1, vec2;
int sum1[N], sum2[N];

int main()
{
    int n, v, ans;
    cin >> n >> v;
    for(int i = 1; i <= n; ++i) {
        int a, b;
        cin >> a >> b;
        if(a == 1) vec1.push_back({b, i});
        else vec2.push_back({b, i});
    }
    sort(vec1.begin(), vec1.end(), greater<pii>());
    sort(vec2.begin(), vec2.end(), greater<pii>());

    for(int i = 1; i <= vec1.size(); ++i) sum1[i] = sum1[i - 1] + vec1[i - 1].first;
    for(int i = 1; i <= vec2.size(); ++i) sum2[i] = sum2[i - 1] + vec2[i - 1].first;

    int c1 = -1, c2 = -1, maxv = -1;
    for(int i = 0; i <= vec1.size(); ++i) {
        if(i > v) break;
        int d = sum1[i] + sum2[min((v - i) / 2, (int)vec2.size())];
        if(d > maxv) {
            maxv = d;
            c1 = i;
            c2 = min((v - i) / 2, (int)vec2.size());
        }
    }
    if(maxv == -1) {
        puts("0");
        return 0;
    }
    printf("%d\n", maxv);
    for(int i = 0; i < c1; ++i) printf("%d ", vec1[i].second);
    for(int i = 0; i < c2; ++i) printf("%d ", vec2[i].second);
    puts("");
    return 0;
} 
```
**卿学姐的代码：** 使用二分来处理物品2,效率不如代码1,但思路很好。
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
const int N = 1e5 + 10;

vector<pii> vec1, vec2;
int sum1[N], sum2[N];

int main()
{
    int n, v, ans;
    cin >> n >> v;
    for(int i = 1; i <= n; ++i) {
        int a, b;
        cin >> a >> b;
        if(a == 1) vec1.push_back({b, i});
        else vec2.push_back({b, i});
    }
    sort(vec1.begin(), vec1.end(), greater<pii>());
    sort(vec2.begin(), vec2.end(), greater<pii>());

    for(int i = 1; i <= vec1.size(); ++i) sum1[i] = sum1[i - 1] + vec1[i - 1].first;
    for(int i = 1; i <= vec2.size(); ++i) sum2[i] = sum2[i - 1] + vec2[i - 1].first;

    int maxv = 0, x = 0, y = 0;
    for(int i = 0; i <= vec1.size(); ++i) {
        if(i > v) break;
        int l = 0, r = vec2.size(), ans = 0;
        while(l <= r) {
            int mid = l + r >> 1;
            if(2 * mid <= v - i) ans = mid, l = mid + 1;
            else r = mid - 1;
        }
        int d = sum1[i] + sum2[ans];
        if(d > maxv) {
            maxv = d;
            x = i;
            y =  ans;
        }
    }

    printf("%d\n", maxv);
    for(int i = 0; i < x; ++i) printf("%d ", vec1[i].second);
    for(int i = 0; i < y; ++i) printf("%d ", vec2[i].second);
    puts("");

    return 0;
}
```
***
### Problem C：Tic-tac-toe
**题目传送门**：http://codeforces.com/contest/3/problem/C

**题意**：给定一个井字棋游戏某一个时刻棋盘的状态，让你判断局势情况。

**解题思路**： 暴力模拟即可。

不合法的情况如下：

1、X的数量少于0的数量。

2、X的数量-0的数量>1。

3、X数量等于0的数量，而此时存在三个X相连（即先手获胜）。这是不可能的，因为先手下子之后，X的数量必然多于0的数量，不可能相等。

4、X的数量大于0的数量，而此时存在三个0相连（即后手获胜）。这是不可能的，因为后手下子之后X的数量和0的数量应当相等。



**易错点**：没有仔细搞清楚不合法情况有哪些。

**代码：**
```cpp
#include <bits/stdc++.h>
#define debug(x) printf("debug: %d\n", x)
using namespace std;

char maze[4][4];
bool search(int x, int y, char ch)
{
    int row = 0, col = 0, l_line = 0, r_line = 0;
    // 列
    for(int i = x; i >= 0; --i) if(maze[i][y] == ch) col++;
    for(int i = x + 1; i < 3; ++i) if(maze[i][y] == ch) col++;
    if(col == 3) return true;
    //debug(1);
    //行
    for(int i = y; i >= 0; --i) if(maze[x][i] == ch) row++;
    for(int i = y + 1; i < 3; ++i) if(maze[x][i] == ch) row++;
    if(row == 3) return true;
    //debug(2);
    // 左对角线
    for(int i = x, j = y; i >= 0 && j >= 0; --i, -- j) if(maze[i][j] == ch) l_line++;
    for(int i = x + 1, j = y + 1; i * j < 9; ++i, ++j) if(maze[i][j] == ch) l_line++;
    if(l_line == 3) return true;
    //debug(3);
    // 右对角线
    for(int i = x, j = y; i >= 0 && y < 3; --i, ++j) if(maze[i][j] == ch) r_line++;
    for(int i = x + 1, j = y - 1; i < 3 && j >= 0; ++i, --j) if(maze[i][j] == ch) r_line++;
    if(r_line == 3) return true;

    return false;
}
int main()
{
    int x0 = 0, x1 = 0;
    for(int i = 0; i < 3; ++i)
        for(int j = 0; j < 3; ++j) {
            cin >> maze[i][j];
            if(maze[i][j] == 'X') x1++;
            if(maze[i][j] == '0') x0++;
        }
    if(abs(x0 - x1) > 1 || (x0 > x1)) {
        puts("illegal");
        return 0;
    }
    bool won1 = false, won2 = false;
    for(int i = 0; i < 3; ++i) {
        for(int j = 0; j < 3; ++j) {
            int ch = maze[i][j];
            if(ch == 'X' || ch == '0') {
                if(search(i, j, ch)) {
                    if(ch == 'X') won1 = true;
                    else won2 = true;
                }
            }
        }
    }

    if(won1 && won2) {
        puts("illegal");
        return 0;
    }
    else if(won1) {
        if(x1 - x0 == 1) puts("the first player won");
        else puts("illegal");
        return 0;
    } else if(won2) {
        if(x1 == x0) puts("the second player won");
        else puts("illegal");
        return 0;
    }
    if(x0 + x1 == 9) puts("draw");
    else if(x1 - x0 == 1) puts("second");
    else if(x1 - x0 == 0) puts("first");
    return 0;
}
```

**大神的代码：**
```cpp
#include <stdio.h>
int i;
char a[9];
bool t(char c, int i)
{

    if (a[3 * i] == c && a[3 * i + 1] == c && a[3 * i + 2] == c || a[i] == c && a[i + 3] == c && a[i + 6] == c || a[0] == c && a[4] == c && a[8] == c || a[2] == c && a[4] == c && a[6] == c)
        return true;
    else
        return false;
}

int main()
{
    int wo = 0, wx = 0, co = 0, cx = 0;
    for (i = 0; i < 9; i++) {
        scanf("%c", &a[i]);
        if (a[i] == '\n') {
            i--;
            continue;
        }
        if (a[i] == '0')
            co++;
        if (a[i] == 'X')
            cx++;
    }
    for (i = 0; i < 3; i++) {
        if (t('0', i)) wo = 1;
        if (t('X', i)) wx = 1;
    }
    if ((wx && cx != co + 1) || (wo && cx != co) || (cx != co + 1 && cx != co))
        printf("illegal");
    else if (wx)
        printf("the first player won");
    else if (wo)
        printf("the second player won");
    else if (cx + co == 9)
        printf("draw");
    else if (cx == co)
        printf("first");
    else
        printf("second");

    return 0;
}
```
***
### Problem D： Least Cost Bracket Sequence
**题目传送门**：http://codeforces.com/contest/3/problem/D

**题意**：

**解题思路**： 
我们可以遍历这个序列，维护一个前缀和sum，当遇到左括号时计数器+1，遇到右括号时计数器-1。

如果中途遇到cnt小于0的情况，则说明这个序列不是括号匹配的，但是在我们这里，右括号可能是'?'变来的，所以当遇到cnt小于0时，则去看前面有没有‘？’变过来的右括号，如果没有，那就说明无论如何这个序列无法被替换为括号匹配的序列；如果有的话，则选取一个“最好”的由'?'变过来的右括号，将其改为左括号，这样的话cnt又可以加2了。这里所谓的“最好”，就是贪心的过程。

我们使用一个大根堆来维护问号序列，用$b - a$的值，也就是右边的cost减去左边的cost，差值越大替换成左括号花费越少。

如果这样到最后sum还大于0，则说明即使无法获得一个括号匹配的序列，输出-1即可。

1.将所有的问号替换为右括号

2.遍历序列，维护前缀和

3.当前缀和小于0则考虑从前面找一个问号变成左括号而不是右括号


**易错点**：c++的优先队列默认是大根堆，注意答案可能超过int，要使用long long。

**代码：**
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> pii;
const int N = 5e4 + 5;

int main()
{
    string str;
    priority_queue<pii> Q;
    ll sum = 0, ans = 0, x, y;
    cin >> str;
    for(int i = 0; i < str.size(); ++i) {
        if(str[i] == '(') sum++;
        else if(str[i] == ')') sum--;
        else {
            sum--;
            cin >> x >> y;
            ans += y;
            str[i] = ')';
            Q.push({y - x, i});
        }
        if(sum < 0) {
            if(Q.empty()) break;
            auto t = Q.top(); Q.pop();
            sum += 2;
            str[t.second] = '(';
            ans -= t.first;
        }
    }

    if(sum) puts("-1");
    else cout << ans << endl << str << endl; 

    return 0;
}

```