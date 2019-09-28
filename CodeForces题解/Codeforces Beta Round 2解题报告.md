### Problem A：Winner
**题目传送门**：http://codeforces.com/contest/2/problem/A

**题意**：现在有一个得分板，按时间顺序记录了每一轮玩家的得分，最终得分最大的为赢家，如果有多个人得分都为最大值，则最先到达最大值的人赢。

**解题思路**： 我们可以先按顺序加和得到游戏结果，找到最大值，然后再模拟一遍游戏过程，找到最终分数最大且先达到最大值的人。

**易错点**：注意符号，第二次最终分数最大且先达到最大值的人可能分数的中间值超过最大值，因为分数可以为负数，另外，不能直接模拟输出最先到最大值的人，这样少了一个充分条件：最终分数最大的人。

**代码：**
```cpp
#include <bits/stdc++.h>
using namespace std;
map<string, int> m1, m2;
vector<string> v1;
vector<int> v2;

int main()
{
    int n, m;
    string str;
    cin >> n;
    for(int i = 0; i < n; ++i) {
        cin >> str >> m;
        v1.push_back(str), v2.push_back(m);
        m1[str] += m;
    }    

    int maxv = 0;
    for(auto &x: m1) maxv = max(maxv, x.second);

    for(int i = 0; i < n; ++i) {
        m2[v1[i]] += v2[i];
        if(m1[v1[i]] >= maxv && m2[v1[i]] >= maxv) {// 最终分数最大的人，且先达到最大值
            cout << v1[i] << endl;
            break;
        }
    }
    return 0;
}
```
***
### Problem B：http://codeforces.com/contest/2/problem/B
**题目传送门**：

**题意**：给你一个$n*n$的矩阵，每次只能向右或向下走，让你找到一条从左上角出发走到右下角的路，满足路径上数的乘积末尾0的个数最少，因为格子中的数字可能有0，所以我们要分成2种情况。
1. 如果找到1条路上$min(2,5)$的值为0，我们选择这一条路；
2. 如果没有找到，并且$min(2, 5) > 1$, 我们选择含0的那条路。

**解题思路**： 要求出乘积末尾0的个数最少，要得到0，则需要因子2和5相乘，那么问题便转化成找到一条路径，路径上$min(2, 5)$的值最小，因为数据量过大，我们不可能用搜索来解决，所以想达到了动态规划。

由上述分析易得状态转移方程：
$dp[i][j][0/1]=min(dp[i−1][j][0/1],dp[i][j−1][0/1])+a[i][j][0/1]$

路径可由递归打印来完成。

**易错点**：注意判断0的情况，打印方向别弄反。

**代码：**
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1005;
int n, x, a, b;
bool flag;
int dp[2][N][N];  // 0表示2的个数，1表示5的个数
int p[2][N][N];

void print_track(int x, int y, int k, int f) // f为0表示向右，为1表示向下
{
    if(x == 1 && y == 1);
    else if(x == 1) print_track(x, y - 1, k, 0); // 只能往左回溯
    else if(y == 1) print_track(x - 1, y, k, 1); // 只能往上回溯
    else {
        if(dp[k][x][y] == dp[k][x - 1][y] + p[k][x][y])   //(x, y)从(x - 1, y)右移1个单位转移而来
            print_track(x - 1, y, k, 1);
        else 
            print_track(x, y - 1, k, 0);
    }
    if(f == 233) return ;
    putchar(f ? 'D' : 'R');
}

int main()
{
    cin >> n;
    for(int i = 1; i <= n; ++i) 
        for(int j = 1; j <= n; ++j) {
            cin >> x;
            if(!x) {
                dp[0][i][j]++, dp[1][i][j]++;
                a = i, b = j;
                flag = true;
                continue;
            }
            while(x % 2 == 0) {
                p[0][i][j]++;
                x /= 2;
            }
            while(x % 5 == 0) {
                p[1][i][j]++;
                x /= 5;
            }
        }
    memset(dp, 0x3f, sizeof dp);
    for(int k = 0; k < 2; ++k) 
        for(int i = 1; i <= n; ++i)
            for(int j = 1; j <= n; ++j) {
                if(i - 1) dp[k][i][j] = min(dp[k][i][j], dp[k][i - 1][j]);
                if(j - 1) dp[k][i][j] = min(dp[k][i][j], dp[k][i][j - 1]);
                if(i == j && i == 1) dp[k][i][j] = 0;
                dp[k][i][j] += p[k][i][j];
            }
    
    int ans = min(dp[0][n][n], dp[1][n][n]);
    if(ans > 1 && flag) {
        puts("1");
        for(int i = 1; i < a; ++i) putchar('D');
        for(int i = 1; i < b; ++i) putchar('R');
        for(int i = a; i < n; ++i) putchar('D');
        for(int i = b; i < n; ++i) putchar('R');
    } else {
        printf("%d\n", ans);
        if(dp[0][n][n] < dp[1][n][n]) {
            print_track(n, n, 0, 233);// 因为最后一个点不用打印，所以我们随便赋值，只要不为1和0就行
        } else {
            print_track(n, n, 1, 233);
        }
    }

    return 0;

}
```

**大神的代码：**
```cpp
#include <bits/stdc++.h>
#define inf 0x3f3f3f3f
using namespace std;
int f[1001][1001][2], n, x, k;
char g[1001][1001][2];
void gao(int x, int y)
{
    if (x == 1 && y == 1) return;
    if (g[x][y][k]) gao(x - 1, y), putchar('D');
    else gao(x, y - 1), putchar('R');
}

int main()
{
    scanf("%d", &n);
    memset(f, 0, sizeof f);
    for (int i = 2; i <= n; ++i) {
        f[0][i][0] = f[0][i][1] = f[i][0][0] = f[i][0][1] = inf;
    }
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= n; ++j) {
            scanf("%d", &k);
            if (!k) {
                x = i;
            }
            else {
                while (k % 2 == 0) ++f[i][j][0], k /= 2;
                while (k % 5 == 0) ++f[i][j][1], k /= 5;
            }
            for (int k = 0; k < 2; k++)
                if (g[i][j][k] = f[i - 1][j][k] < f[i][j - 1][k])
                    f[i][j][k] += f[i - 1][j][k];
                else
                    f[i][j][k] += f[i][j - 1][k];

        }
    }

    k = f[n][n][1] < f[n][n][0];
    if (x && f[n][n][k] > 1) {
        cout << "1\n";
        for (int i = 2; i <= x; i++) putchar('D');
        for (int i = 2; i <= n; i++) putchar('R');
        for (int i = x + 1; i <= n; i++) putchar('D');
        puts("");
    }
    else {
        printf("%d\n", f[n][n][k]);
        gao(n, n);
        puts("");
    }
    return 0;
}

```
***
### Problem C：Commentator problem

**题目传送门**：http://codeforces.com/contest/2/problem/C

**题意**：给定三个圆的圆心坐标$(xi,yi)$和半径$r_i$，求点使得在该点看三个圆的视角相同，如果有多个点，取视角最大的点。

**解题思路**：

>小科普——视角               
>定圆O和不在O上的定点A，从A向O引两条切线，这两条切线所形成的角可以看做视角。 又因为已知O的半径r和OA的长，显然，视角的大小为$2*asin(r/OA)$，也能够利用$sin(r/OA)$的值来衡量。

可以解方程来写，但是做法太过复杂，我们可以使用模拟退火算法。
首先找出这个三个点构成的三角形的圆心，然后计算出$sin(r/OA)$的值，然后分别在上下左右探测，看看哪个值更小。如此循环，直到dt的值小于eps就能输出了。

**易错点**：这里我们可以用x代替asinx,都是增函数，用D/r代替r/D，这样精度更高，最后一定要用方差来衡量，直接用差衡量误差很大。

**代码：**
```cpp
#include <bits/stdc++.h>
#define sqr(x) (x) * (x)
using namespace std;
const int N = 1005;
const double eps = 1e-6;

int dir[4][2] = {1, 0, 0, 1, -1, 0, 0, -1};
double g[3];

struct node {
    double x, y, r;
} c[3];


double f(double ax, double ay) //f函数为三个视角中两两差的平方和，用来衡量解的优劣
{
    for(int i = 0; i < 3; ++i) g[i] = sqrt(sqr(c[i].x - ax) + sqr(c[i].y - ay)) / c[i].r; //用D/r和r/D来衡量是等价的，asin(x)和x是等价的。
    double tmp = 0;
    for(int i = 0; i < 3; ++i) tmp += sqr(g[i] - g[(i + 1) % 3]);
    return tmp;
}

int main()
{
    double dt = 1, ax = 0, ay = 0; //dt为步长， ax和ay为三点组成的三角形的重心
    for(int i = 0; i < 3; ++i) {
        scanf("%lf %lf %lf", &c[i].x, &c[i].y, &c[i].r);
        ax += c[i].x, ay += c[i].y;
    }
    ax /= 3, ay /= 3; // 选择重心进行探测
    while(dt > eps) {
        int pos = -1;
        double now = f(ax, ay);
        for(int i = 0; i < 4; ++i) { // 从4个方向进行探测
            double next = f(ax + dir[i][0] * dt, ay + dir[i][1] * dt);
            if(next < now) now = next, pos = i;
        }
        if(pos == -1) dt *= 0.5; //如果四个方向都没有找到最优解，则缩小步长
        else ax += dir[pos][0] * dt, ay += dir[pos][1] * dt; // 更新探中心
    }
    if(f(ax, ay) < eps) printf("%.5f %.5f\n",ax, ay);

    return 0;
}
```

**卿学姐的代码：**
```cpp
#include <bits/stdc++.h>
#define sqr(x) (x) * (x)
using namespace std;
const int N = 1005;
const double eps = 1e-6;

int dir[4][2] = {1, 0, 0, 1, -1, 0, 0, -1};
double g[3];

struct node {
    double x, y, r;
} c[3];

double cost(double ax, double ay)
{
    for(int i = 0; i < 3; ++i) g[i] = sqrt(sqr(c[i].x - ax) + sqr(c[i].y - ay));
    double tmp = 0;
    for(int i = 0; i < 3; ++i) tmp += sqr(g[i] - g[(i + 1) % 3]);

    return tmp;
}

int main()
{
    double dt = 1, ax = 0, ay = 0; //dt为步长， ax和ay为三点组成的三角形的重心
    for(int i = 0; i < 3; ++i) {
        scanf("%lf %lf %lf", &c[i].x, &c[i].y, &c[i].r);
        ax += c[i].x, ay += c[i].y;
    }
    ax /= 3, ay /= 3; // 选择重心进行探测
    while(dt > eps) {
        bool flag = false;
        if(cost(ax + dt, ay) < cost(ax, ay)) ax += dt, flag = true;
        else if(cost(ax, ay + dt) < cost(ax, ay)) ay += dt, flag = true;
        else if(cost(ax - dt, ay) < cost(ax, ay)) ax -= dt, flag = true;
        else if(cost(ax, ay - dt) < cost(ax, ay)) ay -= dt, flag = true;
        
        if(!flag) dt *= 0.5;
    }

    if(cost(ax, ay) < eps) printf("%.5f %.5f\n", ax, ay);

    return 0;
}
```