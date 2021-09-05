## 数学相关



* 判定质数

  ```c++
  bool is_prime(int x) {
      if (x < 2) return false;
      for (int i = 2; i <= x / i; i ++ )
          if (x % i == 0)
              return false;
      return true;
  }
  ```

  

* 求区间内 素数个数   (线性筛法)

  ```c++
  // 时间复杂度   O(n)
  int primes[N], cnt;     // primes[]存储所有素数
  bool st[N];         // st[x]存储x是否被筛掉
  
  void get_primes(int n) {
      for (int i = 2; i <= n; i ++) {
          if (!st[i]) primes[cnt ++ ] = i;
          for (int j = 0; primes[j] <= n / i; j ++) {
              st[primes[j] * i] = true;
              if (i % primes[j] == 0) break;
          }
      }
  }
  ```

  

* 最大公约数

  ```c++
  int gcd(int a, int b) {
      return b ? gcd(b, a % b) : a;
  }
  ```

  

* 快速幂

  ```c++
  // 求 m^k mod p，时间复杂度 O(logk)。
  int qmi(int m, int k, int p) {
      int res = 1 % p, t = m;
      while (k) {
          if (k & 1) res = res * t % p;
          t = t * t % p;
          k >>= 1;
      }
      return res;
  }
  ```



* 组合数

  ```c++
  // c[a][b] 表示从a个苹果中选b个的方案数
  // 注意 溢出问题, 通常会在下面代码基础上取模
  for (int i = 0; i < N; i ++ )
      for (int j = 0; j <= i; j ++ )
          if (!j) c[i][j] = 1;
          else c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]);
  ```

* 扩展欧几里得算法
  ```c++
  // 扩展欧几里得算法, 求x, y，使得ax + by = gcd(a, b)
  // 返回值为 gcd(a, b）
  LL exgcd(LL a, LL b, LL &x, LL &y) {
      if (!b) {
          x = 1; y = 0;
          return a;
      }
      LL d = exgcd(b, a % b, y, x);
      y -= (a / b) * x;
      return d;
  }

