### Problem A：Theatre Square

**题目传送门**：http://codeforces.com/contest/1/problem/A

**题意**：现在有一个矩形的广场，面积为$n*m$, 要用面积为$a*a$的方形大理石砖铺满，问至少需要多少块。

**解题思路**： 我们只需要计算广场的长度需要多少块覆盖，宽度需要多少块覆盖，然后乘起来即可。

**易错点**：数据最大为$10^{9}$，答案可能会超int范围。
另外，不能直接用`cout << (long long) ceil(n * 1.0 / a) * ceil(m * 1.0 / a) <<endl;`输出，这样计算后得到的答案是double类型的，cout会以e记法输出导致WA。

**代码：**

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    int n, m, a;
    scanf("%d %d %d", &n, &m, &a);
    ll ans = ceil(n * 1.0 / a) * ceil(m * 1.0 / a);
    printf("%lld\n", ans);
    return 0;
}
```
***
### Problem B：Spreadsheet
**题目传送门**：http://codeforces.com/contest/1/problem/B

**题意**：现在有两种编号的表示法，一种是Excel的表示法，一种是RXCY表示法，我们要对它进行互相转换。

**解题思路**： 可以用XC来判断是哪一种表示法，如果字母C前面是数字，则可以断定是RXCY表示法，再根据对应的表示法，换成另外的表示法。

**易错点**：不能仅仅根据第一个字母是R就断定是RXCY表示法，这里的Excel表示法相当于是26进制的，但是这个进制是从1开始的，所以进制转换的时候要对其特殊处理。

**我的代码：**
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
string change(int n)
{
    string res;
    while(n) {
        res += (n - 1) % 26 + 'A';
        n = (n - 1) / 26;
    }
    reverse(res.begin(), res.end());
    return res;
}

int main()
{
    string str;
    int n;
    cin >> n;
    while(n--) {
        int row = 0, col = 0;
        cin >> str;
        bool flag = false;
        for(int i = 0; i < str.size() - 1; ++i) {
            if(isdigit(str[i]) && str[i + 1] == 'C') {
                flag = true;
                break;
            }
        }
        if(flag) {
            int i = 1;
            while(str[i] != 'C') row = row * 10 + str[i] - '0', ++i;
            while(str[++i]) col = col * 10 + str[i] - '0';
            cout << change(col) << row << endl;
        } else {
            int i = 0;
            while(!isdigit(str[i])) col = col * 26 + str[i] - 'A' + 1, ++i;
            while(str[i]) row = row * 10 + str[i] - '0', ++i;
            cout << "R" << row << "C" << col << endl;
        }
    }
    return 0;
}
```

**大神的代码：**
使用scanf正则表达式配合sscanf来达到快速字符串处理的效果。
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

void change(int x)
{
    if(x) {
        change((x - 1) / 26);
        putchar('A' + (x - 1) % 26);
    }
}

int main()
{
    int T;
    char s[105];
    for(scanf("%d", &T); T; T--) {
        int row, col;
        scanf("%s", s);
        if(sscanf(s, "%*[A-Z]%d%*[A-Z]%d", &row, &col) == 2) {
            change(col);
            printf("%d\n", row);
        } else {
            col = 0;
            for(int i = 0; isupper(s[i]); ++i)
                col = col * 26 + s[i] - 'A' + 1;
            printf("R%dC%d\n", row, col);
        }
    
    }
    return 0;
}
```
***
### Problem C：Ancient Berland Circus
**题目传送门**：http://codeforces.com/contest/1/problem/C

**题意**：给定一个正多边形的3个顶点，让你求符合要求的最小正多边形的面积。

**解题思路**： 因为给定的三个点一定能组成三角形，那么它们的外接圆半径r是确定下来了的，根据图像观察可知，当外接圆半径一定，边数越少，正多边形的面积一定是越少的，于是问题转换成如何求最大的圆心角，因为正多边形，每一条边对应的圆心角都是恒定的n，所以我们只需要求给定三个点组成的三角形的边对应的圆心角的最大公因数，即可求出最大圆心角，比如(n, 2n, 3n的最大公因数是n，则圆心角为n)。
然后再根据最大圆心角求出一共能分出多少个小三角形，最后求这三个小三角形即可。
![121718089347118.png](https://i.loli.net/2019/08/15/bWjFgyAR95smTEx.png)
![TIM截图20190815195045.png](https://i.loli.net/2019/08/15/CaLYV4XWeHfwKxt.png)

**易错点**：暂无。

**代码：**
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const double PI = acos(-1.0);
const int INF = 0x3f3f3f3f;
const double eps = 1e-4;

struct Point {
    double x, y;
};

double dist(Point p1, Point p2)
{
    return sqrt((p1.x - p2.x) *(p1.x - p2.x) + (p1.y - p2.y) * (p1.y - p2.y));
}

double Angle(double a, double b, double c)  // 反余弦求角度
{
    return acos((a * a + b * b - c * c) / (2 * b * a));
}


double fgcd(double a, double b)
{
    return a < eps ? b : gcd(fmod(b, a), a);
}

int main()
{
    Point p1, p2, p3;
    double a, b, c, p, s, r;
    double A, B, C, Com; // 角度
    cin >> p1.x >> p1.y >> p2.x >> p2.y >> p3.x >> p3.y;
    a = dist(p1, p2);
    b = dist(p1, p3);
    c = dist(p2, p3);
    p = (a + b + c) / 2; //半周长
    s = sqrt(p * (p -a) * (p - b) * (p - c));
    r = a * b * c / (4 * s); // 外接圆半径
    A = Angle(r, r, a);
    B = Angle(r, r, b);
    //C = Angle(r, r, c); 不能再根据这个来求，因为之前已经产生了误差，要用相减的方式来求C
    C = 2 * PI - A - B;
    Com = fgcd(A, fgcd(B, C));
    double ans = 0.5 * r * r * sin(Com) * (2 * PI / Com); // 乘法比除法精度高，所以乘0.5
    printf("%.6f\n", ans);
    return 0;
}
```