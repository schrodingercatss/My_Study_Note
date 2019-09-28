### Problem A：Kalevitch and Chess

**题目传送门**：http://codeforces.com/contest/7/problem/A

**题意**：现在有一个$8*8$的棋盘，所有格子都是白色的，你可以每次选择一行或者一列涂上黑色，一个格子可以被涂多次，但第二次以上均保持黑色，给你最终的效果，让你用最少的笔数绘制出要求的棋盘效果。

**解题思路**： 为了最小，如果整行都是黑色，那么我们肯定选择一行涂色，反之如果一行中有白色，那么只能选择列涂色。

**易错点**：如果全部都涂满，那么应该输出8，否则输出涂行的次数和涂列的次数。

**代码**：

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N  = 10;
bool vis[N][N];

int main()
{
    string s;
    int cnt1 = 0, cnt2 = 10;
    for(int i = 1; i <= 8; ++i) {
        cin >> s;
        int tot = 0;
        for(int j = 0;j < 8; ++j)
            if(s[j] == 'B') tot++;
        if(tot == 8) cnt1++;
        cnt2 = min(cnt2, tot);
    }
    if(cnt1 == 8) cout << 8 << endl;
    else cout << cnt1 + cnt2 << endl;
    return 0;
}
```



***

### Problem B：

**题目传送门**：

**题意**：

**解题思路**： 

**易错点**：

**代码**：

***

### Problem C：

**题目传送门**：

**题意**：

**解题思路**： 

**易错点**：

**代码**：

***

### Problem D：

**题目传送门**：

**题意**：

**解题思路**： 

**易错点**：

**代码**：

***

### Problem E：

**题目传送门**：

**题意**：

**解题思路**： 

**易错点**：

**代码**：

***