## poj1064：Cable master

### 题目传送门

[poj1064：Cable master](http://poj.org/problem?id=1064)

***

### 解题思路

这道题直接求k值很复杂，但是我们可以使用二分k值，很容易得到答案。

这道题的坑点就是精度,输出的答案我们不能对其四舍五入，因为如果`k = 2.55`,四舍五入变成`k=2.60`就会得到错误的答案。

***

### Code

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
using namespace std;
const int N = 1e4;
const int INF = 0x3f3f3f3f;
double arr[N + 5];
int n, k;

bool judge(double d)
{
    int num = 0;
    for(int i = 0; i < n; ++i) {
        num += (int)(arr[i] / d);
    }
    return num >= k;
}

void slove()
{
    double l = 0, r = INF;
    for(int i = 0; i < 100; ++i) {
        double mid = (l + r) / 2;
        if(judge(mid)) l = mid;
        else r = mid;
    }
    printf("%.2f\n", floor(r * 100) / 100);
}

int main()
{
    scanf("%d %d", &n, &k);
    for(int i = 0; i < n; ++i) scanf("%lf", &arr[i]);
    slove();
    return 0;
}

```

