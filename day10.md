## 算法模板

* spfa

  ```c++
  int n, m;      // 总点数 总边数
  int h[N], w[M], e[M], ne[M], idx;       // 邻接表存储所有边
  int dist[N];        // 存储每个点到1号点的最短距离
  bool st[N];     // 存储每个点是否在队列中
  
  // 图的构建, 邻接表存储
  // 加边函数一定记得 memset(h, -1, sizeof(h));
  void add(int a, int b, int c) {
      e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
  }
  
  // 求1号点到n号点的最短路距离，如果从1号点无法走到n号点则返回-1
  // spfa 可以用栈代替队列(TLE时可以换一下试一试)
  int spfa() {
      memset(dist, 0x3f, sizeof dist);
      dist[1] = 0;
  
      queue<int> q;
      q.push(1);
      st[1] = true;
  
      while (q.size()) {
          auto t = q.front();
          q.pop();
  
          st[t] = false;
  
          for (int i = h[t]; i != -1; i = ne[i]) {
              int j = e[i];
              if (dist[j] > dist[t] + w[i]) {
                  dist[j] = dist[t] + w[i];
                  if (!st[j]) {    // 如果队列中已存在j，则不需要将j重复插入
                      q.push(j);
                      st[j] = true;
                  }
              }
          }
      }
  
      // 如果dist[n] == 0x3f3f3f3f 则表示不存在路径到 n
      return dist[n];
  }
  ```
  
  [spfa求最短路](https://www.acwing.com/problem/content/853/)
  
  [spfa判断图中是否存在负环](https://www.acwing.com/problem/content/854/)



* Floyd(多源最短路)

  ```c++
  const int INF = 0x3f3f3f3f;
  int n, m, Q;
  int d[N][N];
  
  // 初始化：
      for (int i = 1; i <= n; i ++ )
          for (int j = 1; j <= n; j ++ )
              if (i == j) d[i][j] = 0;
              else d[i][j] = INF;
  
  // 算法结束后，d[a][b]表示a到b的最短距离
  void floyd() {
      for (int k = 1; k <= n; k ++ )
          for (int i = 1; i <= n; i ++ )
              for (int j = 1; j <= n; j ++ )
                  d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
  }
  
  // 处理结果时(查询 a b)
  if (d[a][b] > INF / 2) puts("impossible");  // 可能存在 INF ->(-2) INF 的情况
  else printf("%d", d[a][b]);
  ```
  

[Floyd求最短路](https://www.acwing.com/problem/content/856/)



Floyd 求传递闭包

```markdown
给定 n 个变量和 m 个不等式。其中 n 小于等于 26，变量分别用前 n 的大写英文字母表示。
不等式之间具有传递性，即若 A>B 且 B>C，则 A>C。请从前往后遍历每对关系，每次遍历时判断：
1. 如果能够确定全部关系且无矛盾，则结束循环，输出确定的次序；
2. 如果发生矛盾，则结束循环，输出有矛盾；
3. 如果循环结束时没有发生上述两种情况，则输出无定解。

输入格式
	输入包含多组测试数据。每组测试数据，第一行包含两个整数 n 和 m。接下来 m 行，每行包含一个不等式，不等式全部为小于关系。当输入一行 0 0 时，表示输入终止。

输出格式
	每组数据输出一个占一行的结果。结果可能为下列三种之一：
	1. 如果可以确定两两之间的关系，则输出 "Sorted sequence determined after t relations: yyy...y.",其中't'指迭代次数，'yyy...y'是指升序排列的所有变量。
	2. 如果有矛盾，则输出： "Inconsistency found after t relations."，其中't'指迭代次数。
	3. 如果没有矛盾，且不能确定两两之间的关系，则输出 "Sorted sequence cannot be determined."。
```

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 26;

int n, m;
bool g[N][N], d[N][N];
bool st[N];

void floyd() {
    memcpy(d, g, sizeof d);

    for (int k = 0; k < n; k ++ )
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ )
                d[i][j] |= d[i][k] && d[k][j];
}

int check() {
    for (int i = 0; i < n; i ++ )
        if (d[i][i])
            return 2;

    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < i; j ++ )
            if (!d[i][j] && !d[j][i])
                return 0;

    return 1;
}

char get_min() {
    for (int i = 0; i < n; i ++ )
        if (!st[i]) {
            bool flag = true;
            for (int j = 0; j < n; j ++ )
                if (!st[j] && d[j][i]) {
                    flag = false;
                    break;
                }
            if (flag) {
                st[i] = true;
                return 'A' + i;
            }
        }
}

int main() {
    while (cin >> n >> m, n || m) {
        memset(g, 0, sizeof g);
        int type = 0, t;
        for (int i = 1; i <= m; i ++ ) {
            char str[5];
            cin >> str;
            int a = str[0] - 'A', b = str[2] - 'A';

            if (!type) {
                g[a][b] = 1;
                floyd();
                type = check();
                if (type) t = i;
            }
        }

        if (!type) puts("Sorted sequence cannot be determined.");
        else if (type == 2) printf("Inconsistency found after %d relations.\n", t);
        else {
            memset(st, 0, sizeof st);
            printf("Sorted sequence determined after %d relations: ", t);
            for (int i = 0; i < n; i ++ ) printf("%c", get_min());
            printf(".\n");
        }
    }

    return 0;
}
```

