## 牛客网：Math Problem

### 题目传送门

[牛客网：Math Problem](<https://ac.nowcoder.com/acm/contest/893/C>)

***

### 解题思路

由同余定理可知，与192同余1的数肯定会构成一个等差数列,${1, 193, 385,....}$,这样，我们只需要对题目给出的区间进行处理，然后使用等差数列求和公式即可。

***

### Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    int l, r, T;
    cin >> T;
    while(T--) {
        cin >> l >> r;
        //对区间进行处理，保证左值和右值均余数均为1。
        while(l % 192 != 1) l++;
        while(r % 192 != 1) r--;

        ll a = (r - l) / 192 + 1;  //等差数列的项数
        ll b = (l + r) / 2ll * a;   //等差数列求和
        cout << b << endl; 
    }
    return 0;
}

```

