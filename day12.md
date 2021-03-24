## 算法模板

* 染色法判别二分图

  ```c++
  // 时间复杂度 O(m + n)
  int n;      // n表示点数
  int h[N], e[M], ne[M], idx;     // 邻接表存储图
  int color[N];       // 表示每个点的颜色，-1表示未染色，0表示白色，1表示黑色
  
  void add(int a, int b) {
      e[idx] = b, ne[idx] = h[a], h[a] = idx++;
  } 
  
  // 参数：u表示当前节点，c表示当前点的颜色
  bool dfs(int u, int c) {
      color[u] = c;
      for (int i = h[u]; i != -1; i = ne[i])
      {
          int j = e[i];
          if (color[j] == -1)
          {
              if (!dfs(j, !c)) return false;
          }
          else if (color[j] == c) return false;
      }
  
      return true;
  }
  
  bool check() {
      memset(color, -1, sizeof color);
      bool flag = true;
      for (int i = 1; i <= n; i ++ )
          if (color[i] == -1)
              if (!dfs(i, 0)) {
                  flag = false;
                  break;
              }
      return flag;
  }
  ```
  
  [染色法判定二分图](https://www.acwing.com/problem/content/862/)



* 匈牙利算法(二分图最大匹配算法)

  ```c++
  int n1, n2;     // n1表示第一个集合中的点数，n2表示第二个集合中的点数
  int h[N], e[M], ne[M], idx;     // 邻接表存储所有边，匈牙利算法中只会用到从第一个集合指向第二个集合的边，所以这里只用存一个方向的边
  int match[N];       // 存储第二个集合中的每个点当前匹配的第一个集合中的点是哪个
  bool st[N];     // 表示第二个集合中的每个点是否已经被遍历过
  
  void add(int a, int b) {
      e[idx] = b, ne[idx] = h[a], h[a] = idx++; 
  }
  
  bool find(int x)
  {
      for (int i = h[x]; i != -1; i = ne[i])
      {
          int j = e[i];
          if (!st[j])
          {
              st[j] = true;
              if (match[j] == 0 || find(match[j]))
              {
                  match[j] = x;
                  return true;
              }
          }
      }
  
      return false;
  }
  
  // 求最大匹配数，依次枚举第一个集合中的每个点能否匹配第二个集合中的点
  int res = 0;
  for (int i = 1; i <= n1; i ++ )
  {
      memset(st, false, sizeof st);
      if (find(i)) res ++ ;
  }
  ```

  [二分图的最大匹配](https://www.acwing.com/problem/content/863/)