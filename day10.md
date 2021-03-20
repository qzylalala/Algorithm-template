## 算法模板

* spfa

  ```c++
  int n, m;      // 总点数 总边数
  int h[N], w[M], e[M], ne[M], idx;       // 邻接表存储所有边
  int dist[N];        // 存储每个点到1号点的最短距离
  bool st[N];     // 存储每个点是否在队列中
  
  // 图的构建, 邻接表存储
  void add(int a, int b, int c) {
      e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
  }
  
  // 求1号点到n号点的最短路距离，如果从1号点无法走到n号点则返回-1
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
  
      if (dist[n] == 0x3f3f3f3f) return -1;
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