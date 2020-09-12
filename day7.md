## 算法模板

* 树与图的存储(邻接表)

  ```c++
  // 对于每个点k，开一个单链表，存储k所有可以走到的点。h[k]存储这个单链表的头结点
  int h[N], e[N], ne[N], idx;
  
  // 添加一条边a->b
  void add(int a, int b)
  {
      e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
  }
  
  // 初始化
  idx = 0;
  memset(h, -1, sizeof h);
  ```



* 树与图的遍历

  ```c++
  (1) 深度优先
  int dfs(int u)
  {
      st[u] = true; // st[u] 表示点u已经被遍历过
  
      for (int i = h[u]; i != -1; i = ne[i])
      {
          int j = e[i];
          if (!st[j]) dfs(j);
      }
  }
  
  (2) 宽度优先
  queue<int> q;
  st[1] = true; // 表示1号点已经被遍历过
  q.push(1);
  
  while (q.size())
  {
      int t = q.front();
      q.pop();
  
      for (int i = h[t]; i != -1; i = ne[i])
      {
          int j = e[i];
          if (!st[j])
          {
              st[j] = true; // 表示点j已经被遍历过
              q.push(j);
          }
      }
  }
  ```

  [树的重心](https://www.acwing.com/problem/content/848/)

  [图中点的层次](https://www.acwing.com/problem/content/849/)



* 拓扑排序 O(n + m)

  ```c++
  bool topsort()
  {
      int hh = 0, tt = -1;
  
      // d[i] 存储点i的入度
      for (int i = 1; i <= n; i ++ )
          if (!d[i])
              q[ ++ tt] = i;
  
      while (hh <= tt)
      {
          int t = q[hh ++ ];
  
          for (int i = h[t]; i != -1; i = ne[i])
          {
              int j = e[i];
              if (-- d[j] == 0)
                  q[ ++ tt] = j;
          }
      }
  
      // 如果所有点都入队了，说明存在拓扑序列；否则不存在拓扑序列。
      return tt == n - 1;
  }
  ```

  [有向图的拓扑排序](https://www.acwing.com/problem/content/850/)