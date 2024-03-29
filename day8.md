## 算法模板

* 朴素dijkstra算法

  ```c++
  #include <iostream>
  #include <cstring>
  #include <algorithm>

  using namespace std;

  const int N = 1010;
  int n, m;
  int g[N][N];  // 存储每条边
  int dist[N];  // 存储1号点到每个点的最短距离
  bool st[N];   // 存储每个点的最短路是否已经确定

  // 求1号点到n号点的最短路，如果不存在则返回-1
  int dijkstra() {
      memset(dist, 0x3f, sizeof dist);
      dist[1] = 0;

      for (int i = 0; i < n - 1; i ++ ) {
          int t = -1;     // 在还未确定最短路的点中，寻找距离最小的点
          for (int j = 1; j <= n; j ++ )
              if (!st[j] && (t == -1 || dist[t] > dist[j]))
                  t = j;

          // 用t更新其他点的距离
          for (int j = 1; j <= n; j ++ )
              dist[j] = min(dist[j], dist[t] + g[t][j]);

          st[t] = true;
      }

      if (dist[n] == 0x3f3f3f3f) return -1;
      return dist[n];
  }

  int main() {
    cin >> n >> m;
    // 求最短路，初始化距离为极大
    memset(g, 0x3f, sizeof(g));
    for (int i = 1; i <= m; i ++) {
      int x, y, z;
      cin >> x >> y >> z;
      g[x][y] = min(g[x][y], z);
    }

    cout << dijkstra() << endl;

    return 0;
  }
  ```

  [dijkstra 求最短路](https://www.acwing.com/problem/content/851/)



* 堆优化版dijkstra 

  ```c++
  // 时间复杂度 O(nm), n表示点数，m表示边数
  // 注意在模板题中需要对下面的模板稍作修改，加上备份数组，详情见模板题。
  typedef pair<int, int> PII;
  
  int n;      // 点的数量
  int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
  int dist[N];        // 存储所有点到1号点的距离
  bool st[N];     // 存储每个点的最短距离是否已确定
  
  // 求1号点到n号点的最短距离，如果不存在，则返回-1
  int dijkstra()
  {
      memset(dist, 0x3f, sizeof dist);
      dist[1] = 0;
      priority_queue<PII, vector<PII>, greater<PII>> heap;
      heap.push({0, 1});      // first存储距离，second存储节点编号
  
      while (heap.size())
      {
          auto t = heap.top();
          heap.pop();
  
          int ver = t.second, distance = t.first;
  
          if (st[ver]) continue;
          st[ver] = true;
  
          for (int i = h[ver]; i != -1; i = ne[i])
          {
              int j = e[i];
              if (dist[j] > distance + w[i])
              {
                  dist[j] = distance + w[i];
                  heap.push({dist[j], j});
              }
          }
      }
  
      if (dist[n] == 0x3f3f3f3f) return -1;
      return dist[n];
  }
  ```

  [dijkstra 求最短路II](https://www.acwing.com/problem/content/852/)



* Bellman-Ford

  ```c++
  #include <cstring>
  #include <iostream>
  #include <algorithm>
  
  using namespace std;
  
  const int N = 510, M = 10010;
  
  struct Edge {
      int a, b, c;
  }edges[M];
  
  int n, m, k;
  int dist[N];
  int last[N];
  
  void bellman_ford() {
      memset(dist, 0x3f, sizeof dist);
  
      dist[1] = 0;
      for (int i = 0; i < k; i ++ ) {   // 循环 K 次, 从1号点到n号点的最多经过k条边的最短距离
          memcpy(last, dist, sizeof dist);
          for (int j = 0; j < m; j ++ ) {
              auto e = edges[j];
              dist[e.b] = min(dist[e.b], last[e.a] + e.c);
          }
      }
  }
  
  int main() {
      scanf("%d%d%d", &n, &m, &k);
  
      for (int i = 0; i < m; i ++ ) {
          int a, b, c;
          scanf("%d%d%d", &a, &b, &c);
          edges[i] = {a, b, c};
      }
  
      bellman_ford();
  
      if (dist[n] > 0x3f3f3f3f / 2) puts("impossible");
      else printf("%d\n", dist[n]);
  
      return 0;
  }
  ```

  

