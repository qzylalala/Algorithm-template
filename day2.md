### 算法模板

* 高精度加法

  ```c++
  // C = A + B, A >= 0, B >= 0
  // A, B 为倒序
  // 计算A + B (1234 + 5678) 输入应该为 A [4, 3, 2, 1],  B[8， 7， 6， 5]
  // 得到的 C 也为倒序
  vector<int> add(vector<int> &A, vector<int> &B)
  {
      if (A.size() < B.size()) return add(B, A);
  
      vector<int> C;
      int t = 0;
      for (int i = 0; i < A.size(); i ++ )
      {
          t += A[i];
          if (i < B.size()) t += B[i];
          C.push_back(t % 10);
          t /= 10;
      }
  
      if (t) C.push_back(t);   // 最高位进位
      return C;
  }
  ```



* 高精度减法

  ```c++
  // C = A - B, 满足A >= B, A >= 0, B >= 0
  // 同加法, 输入为逆序
  vector<int> sub(vector<int> &A, vector<int> &B)
  {
      vector<int> C;
      for (int i = 0, t = 0; i < A.size(); i ++ )
      {
          t = A[i] - t;
          if (i < B.size()) t -= B[i];
          C.push_back((t + 10) % 10);
          if (t < 0) t = 1;
          else t = 0;
      }
  
      while (C.size() > 1 && C.back() == 0) C.pop_back();   // 去掉先导 0
      return C;
  }
  ```



* 高精度乘低精度

  ```c++
  // C = A * b, A >= 0, b > 0
  vector<int> mul(vector<int> &A, int b)
  {
      vector<int> C;
  
      int t = 0;
      for (int i = 0; i < A.size() || t; i ++ )
      {
          if (i < A.size()) t += A[i] * b;
          C.push_back(t % 10);
          t /= 10;
      }
  
      while (C.size() > 1 && C.back() == 0) C.pop_back(); // 去掉先导 0
  
      return C;
  }
  ```



* 高精度除以低精度

  ```c++
  // A / b = C ... r, A >= 0, b > 0
  vector<int> div(vector<int> &A, int b, int &r)
  {
      vector<int> C;
      r = 0;
      for (int i = A.size() - 1; i >= 0; i -- )
      {
          r = r * 10 + A[i];
          C.push_back(r / b);
          r %= b;
      }
      reverse(C.begin(), C.end());
      while (C.size() > 1 && C.back() == 0) C.pop_back();
      return C;
  }
  ```



* 前缀和

  ```markdown
  一维前缀和
  S[i] = a[1] + a[2] + ... a[i]
  a[l] + ... + a[r] = S[r] - S[l - 1]
  
  二维前缀和
  S[i, j] = 第i行j列格子左上部分所有元素的和
  以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：
  S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]
  ```

  

* 差分

  ```markdown
  一维差分
  给区间[l, r]中的每个数加上c：B[l] += c, B[r + 1] -= c
  
  二维差分
  给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：
  S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c
  ```

  [一维差分-练习题](https://www.acwing.com/problem/content/799/)

  [二维差分-练习题](https://www.acwing.com/problem/content/800/)

  