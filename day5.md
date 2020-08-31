## 算法模板

* Tire树

  ```c++
  int son[N][26], cnt[N], idx;
  // 这里 26 是表示所有小写字母(若有大写字母，则可修改为 52)
  // 0号点既是根节点，又是空节点
  // son[][]存储树中每个节点的子节点
  // cnt[]存储以每个节点结尾的单词数量
  
  // 插入一个字符串
  void insert(char *str)
  {
      int p = 0;
      for (int i = 0; str[i]; i ++ )
      {
          int u = str[i] - 'a';
          if (!son[p][u]) son[p][u] = ++ idx;
          p = son[p][u];
      }
      cnt[p] ++ ;
  }
  
  // 查询字符串出现的次数
  int query(char *str)
  {
      int p = 0;
      for (int i = 0; str[i]; i ++ )
      {
          int u = str[i] - 'a';
          if (!son[p][u]) return 0;
          p = son[p][u];
      }
      return cnt[p];
  }
  ```

  [Tire 字符串统计](https://www.acwing.com/problem/content/837/)



* 并查集

  ```c++
  (1)朴素并查集：
  
      int p[N]; //存储每个点的祖宗节点,根节点满足 p[x] = x
  
      // 返回x的祖宗节点
      int find(int x)
      {
          if (p[x] != x) p[x] = find(p[x]);
          return p[x];
      }
  
      // 初始化，假定节点编号是1~n
      for (int i = 1; i <= n; i ++ ) p[i] = i;
  
      // 合并a和b所在的两个集合：
      p[find(a)] = find(b);
  
  
  (2)维护size的并查集：
  
      int p[N], size[N];
      //p[]存储每个点的祖宗节点, size[]只有祖宗节点的有意义，表示祖宗节点所在集合中的点的数量
  
      // 返回x的祖宗节点
      int find(int x)
      {
          if (p[x] != x) p[x] = find(p[x]);
          return p[x];
      }
  
      // 初始化，假定节点编号是1~n
      for (int i = 1; i <= n; i ++ )
      {
          p[i] = i;
          size[i] = 1;
      }
  
      // 合并a和b所在的两个集合：
      size[find(b)] += size[find(a)];
      p[find(a)] = find(b);
  
  
  (3)维护到祖宗节点距离的并查集：
  
      int p[N], d[N];
      //p[]存储每个点的祖宗节点, d[x]存储x到p[x]的距离
  
      // 返回x的祖宗节点
      int find(int x)
      {
          if (p[x] != x)
          {
              int u = find(p[x]);
              d[x] += d[p[x]];
              p[x] = u;
          }
          return p[x];
      }
  
      // 初始化，假定节点编号是1~n
      for (int i = 1; i <= n; i ++ )
      {
          p[i] = i;
          d[i] = 0;
      }
  
      // 合并a和b所在的两个集合：
      p[find(a)] = find(b);
      d[find(a)] = distance; // 根据具体问题，初始化find(a)的偏移量
  ```

  [合并集合](https://www.acwing.com/problem/content/838/)

  [连通块中点的数量](https://www.acwing.com/problem/content/839/)



* 手写堆

  ```c++
  // h[N]存储堆中的值, h[1]是堆顶，x的左儿子是2x, 右儿子是2x + 1
  // ph[k]存储第k个插入的点在堆中的位置
  // hp[k]存储堆中下标是k的点是第几个插入的
  int h[N], ph[N], hp[N], size;
  
  // 交换两个点，及其映射关系
  void heap_swap(int a, int b)
  {
      swap(ph[hp[a]],ph[hp[b]]);
      swap(hp[a], hp[b]);
      swap(h[a], h[b]);
  }
  
  void down(int u)
  {
      int t = u;
      if (u * 2 <= size && h[u * 2] < h[t]) t = u * 2;
      if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) t = u * 2 + 1;
      if (u != t)
      {
          heap_swap(u, t);
          down(t);
      }
  }
  
  void up(int u)
  {
      while (u / 2 && h[u] < h[u / 2])
      {
          heap_swap(u, u / 2);
          u >>= 1;
      }
  }
  
  // O(n)建堆
  for (int i = n / 2; i; i -- ) down(i);
  ```

  [堆排序](https://www.acwing.com/problem/content/840/)

  [模拟堆](https://www.acwing.com/problem/content/841/)
