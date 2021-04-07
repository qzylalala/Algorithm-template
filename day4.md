### 算法模板

* 数组实现静态链表，提高效率

  ```c++
  // head存储链表头，e[]存储节点的值，ne[]存储节点的next指针，idx表示当前用到了哪个节点
  int head, e[N], ne[N], idx;
  
  // 初始化
  void init()
  {
      head = -1;
      idx = 0;
  }
  
  // 在链表头插入一个数a
  void insert(int a)
  {
      e[idx] = a, ne[idx] = head, head = idx ++ ;
  }
  
  // 将头结点删除，需要保证头结点存在
  void remove()
  {
      head = ne[head];
  }
  ```

  

* 单调栈

  ```c++
  常见模型：找出每个数左边离它最近的比它大/小的数
  int tt = 0;
  for (int i = 1; i <= n; i ++ )
  {
      while (tt && check(stk[tt], i)) tt -- ;
      stk[ ++ tt] = i;
  }
  ```

  [单调栈习题](https://www.acwing.com/problem/content/832/)



* 单调队列

  ```c++
  常见模型：找出滑动窗口中的最大值/最小值
  int hh = 0, tt = -1;
  for (int i = 0; i < n; i ++ )
  {
      while (hh <= tt && check_out(q[hh])) hh ++ ;  // 判断队头是否滑出窗口
      while (hh <= tt && check(q[tt], i)) tt -- ;
      q[ ++ tt] = i;
  }
  ```

  [滑动窗口](https://www.acwing.com/problem/content/156/)



* KMP算法

  ```c++
  // s[]是长文本，p[]是模式串，n是s的长度，m是p的长度
  cin >> m >> p + 1 >> n >> s + 1;
  
  // 求 next 数组
  for (int i = 2, j = 0; i <= m; i++) {
      while (j && p[i] != p[j + 1]) j = ne[j];
      if (p[i] == p[j + 1]) j++;
      ne[i] = j;
  }
  
  
  // 匹配
  for (int i = 1, j = 0; i <= n; i++) {
      while( j && s[i] != p[j + 1]) j = ne[j];
      if (s[i] == p[j + 1]) j++;
      if (j == m) { // 匹配成功
          j = ne[j];
          // todo
      }
  }
  ```

  [KMP  字符串](https://www.acwing.com/problem/content/833/)