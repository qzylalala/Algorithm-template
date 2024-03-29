## 算法模板

* Prim

  ```c++
  // 时间复杂度是 O(n^2 + m), n表示点数，m表示边数
  const int INF = 0x3f3f3f3f;
  int n;      // n表示点数
  int g[N][N];        // 邻接矩阵，存储所有边
  int dist[N];        // 存储其他点到当前最小生成树的距离
  bool st[N];     // 存储每个点是否已经在生成树中
  
  
  // 如果图不连通，则返回INF(值是0x3f3f3f3f), 否则返回最小生成树的树边权重之和
  int prim() {
      memset(dist, 0x3f, sizeof dist);
  
      int res = 0;
      for (int i = 0; i < n; i ++ ) {
          int t = -1;
          for (int j = 1; j <= n; j ++ )
              if (!st[j] && (t == -1 || dist[t] > dist[j]))
                  t = j;
  
          if (i && dist[t] == INF) return INF;
  
          if (i) res += dist[t];
          st[t] = true;
  
          for (int j = 1; j <= n; j ++ ) dist[j] = min(dist[j], g[t][j]);
      }
  
      return res;
  }
  
  // 需要初始化 g[][] : 
  // 			g[i][i] = 0,  g[i][j] = INF
  ```

* Kruskal

  ```c++
  // 时间复杂度是 O(mlogm), n表示点数，m表示边数
  const int INF = 0x3f3f3f3f;
  int n, m;       // n是点数，m是边数
  int p[N];       // 并查集的父节点数组
  
  struct Edge {    // 存储边
      int a, b, w;
  
      bool operator< (const Edge &W)const {
          return w < W.w;
      }
  }edges[M];
  
  int find(int x) {
      if (p[x] != x) p[x] = find(p[x]);
      return p[x];
  }
  
  int kruskal() {
      sort(edges, edges + m);
  
      for (int i = 1; i <= n; i ++ ) p[i] = i;    // 初始化并查集
  
      int res = 0, cnt = 0;
      for (int i = 0; i < m; i ++ ) {
          int a = edges[i].a, b = edges[i].b, w = edges[i].w;
  
          a = find(a), b = find(b);
          if (a != b) {    // 如果两个连通块不连通，则将这两个连通块合并
              p[a] = b;
              res += w;
              cnt ++ ;
          }
      }
  
      if (cnt < n - 1) return INF;
      return res;
  }
  ```

    [Kruskal最小生成树](https://www.acwing.com/problem/content/861/)
