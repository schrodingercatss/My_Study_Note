## POJ1984：**Navigation Nightmare**

### 题目传送门

[poj:2924 Navigation Nightmare](<http://poj.org/problem?id=1984>)

***

### 题目大意

输入N表示村庄，M表示命令条数。接下来有M条命令，输入x，y，z，e

分别表示村庄x到村庄y的距离为z，e表示东西南北的方向。

接下来输入k，表示有k个询问。输入k行命令，分别有x，y，z。

求村庄x与村庄y在第z条命令后的曼哈顿距离。

这题是一个典型的带权并查集问题，不同的地方是这题既需要维护根节点到x方向上的偏移量，还需要维护根节点y方向上的偏移量。

Find操作时时，每次找完father就要加上father的x、y坐标偏移量，这样Find操作完以后就得到了与根的偏移量。然后合并时, （注意，这里是 bcj[x].pre = y,如果写成bcj[y].pre = x需要翻转一下写法）

dr[x].x = r.x - dr[r.u].x + dr[r.v].x;  

dr[x].y = r.y - dr[r.u].y + dr[r.v].y;      即 x->y <==> u->v - u->x + v->y 如下图：

![EUVv0f.jpg](https://s2.ax1x.com/2019/05/03/EUVv0f.jpg)

***

### Code

```cpp
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <queue>
using namespace std;
const int MAXN = 4e4 + 5;
const int MAXV = 1e4 + 5;
int n, m, k, ans[MAXV];
struct Edge {  //存储边数据
    int u, v, len;
    char dir;
} edge[MAXN];

struct Query { //存储询问
    int u, v, index, pos;
} query[MAXV];

struct node {  //并查集
    int x, y, pre;
} bcj[MAXN];

inline void init()
{
    query[0].index = 0;
    for(int i = 0; i <= n; ++i) {
        bcj[i].pre = -1;
        bcj[i].x = bcj[i].y = 0;
    }
}

int Find(int x)
{
    if(bcj[x].pre < 0) return x;
    int t = bcj[x].pre;
    bcj[x].pre = Find(bcj[x].pre);
    bcj[x].x += bcj[t].x;
    bcj[x].y += bcj[t].y;
    return bcj[x].pre;
}

void Union(int a, int b, int d, char dir)
{
    int ra = Find(a), rb = Find(b);
    if(ra == rb) return;
    bcj[ra].pre = rb;
    if(dir == 'E' || dir == 'W') {
        if(dir == 'W') d = -d; //以东为正方向
        bcj[ra].x = bcj[b].x + d - bcj[a].x; //更新东西权值
        bcj[ra].y = bcj[b].y - bcj[a].y;  //更新南北权值
    }
    else if(dir == 'N' || dir == 'S') {
        if(dir == 'S') d = -d;  //以北为正方向
        bcj[ra].x = bcj[b].x - bcj[a].x;
        bcj[ra].y = bcj[b].y + d - bcj[a].y;
    }
}

int main()
{
    scanf("%d %d", &n, &m);
    init();
    for(int i = 1; i <= m; ++i) {
        scanf("%d %d %d %c", &edge[i].u, &edge[i].v, &edge[i].len, &edge[i].dir);
    }
    scanf("%d", &k);
    for(int i = 1; i <= k; ++i) {
        scanf("%d %d %d", &query[i].u, &query[i].v, &query[i].index);
    }

    for(int i = 1; i <= k; ++i) {
        for(int j = query[i - 1].index + 1; j <= query[i].index; ++j) {
            Union(edge[j].u, edge[j].v, edge[j].len, edge[j].dir);
        }
        int a = query[i].u, b = query[i].v;
        int ra = Find(a), rb = Find(b);
        if(ra == rb) {
            ans[i] = abs(bcj[a].x - bcj[b].x) + abs(bcj[a].y - bcj[b].y);
        }
        else ans[i] = -1;
    }
    for(int i = 1; i <= k; ++i) 
        printf("%d\n", ans[i]);
    return 0;
}
```

