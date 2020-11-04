### 算法模板

* 位运算

  ```c++
  // 求n的第k位数字 
  n >> k & 1
      
// 返回n的最后一位1
  lowbit(n) = n & -n     // ex: x = 101010110000  返回值为10000
      
  
  // 快速加	求 a * b % p (a 和 b很大)
  res = 0    
  while(b) {
      if(b & 1) res=(res + a) % p;
      b >>= 1;
      a = 2 * a % p;
  }
  ```
  
  [lowbit使用例题](https://www.acwing.com/problem/content/803/)
  
  [快速加应用](https://www.acwing.com/problem/content/92/)



* 双指针算法

  ```c++
  for (int i = 0, j = 0; i < n; i ++ )
  {
      while (j < i && check(i, j)) j ++ ;
  
      // 具体问题的逻辑
  }
  常见问题分类：
      (1) 对于一个序列，用两个指针维护一段区间
      (2) 对于两个序列，维护某种次序，比如归并排序中合并两个有序序列的操作
  ```

  [最长连续不重复子序列](https://www.acwing.com/problem/content/801/)

  [数组元素的目标和](https://www.acwing.com/problem/content/802/)



* 离散化  (值域很大，但数据稀疏)

  ```c++
  vector<int> alls; // 存储所有待离散化的值
  sort(alls.begin(), alls.end()); // 将所有值排序
  alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素
  
  // 二分求出x对应的离散化的值
  int find(int x) // 找到第一个大于等于x的位置
  {
      int l = 0, r = alls.size() - 1;
      while (l < r)
      {
          int mid = l + r >> 1;
          if (alls[mid] >= x) r = mid;
          else l = mid + 1;
      }
      return r + 1; // 映射到1, 2, ...n
  }
  ```

  [区间和--(离散化 + 前缀和)](https://www.acwing.com/problem/content/804/)



* 区间和并

  ```c++
  // 将所有存在交集的区间合并
  void merge(vector<PII> &segs)
  {
      vector<PII> res;
  
      sort(segs.begin(), segs.end());
  
      int st = -2e9, ed = -2e9;
      for (auto seg : segs)
          if (ed < seg.first)
          {
              if (st != -2e9) res.push_back({st, ed});
              st = seg.first, ed = seg.second;
          }
          else ed = max(ed, seg.second);
  
      if (st != -2e9) res.push_back({st, ed});
  
      segs = res;
  }
  ```

  [区间和并](https://www.acwing.com/problem/content/805/)

