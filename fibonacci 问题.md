## fibonacci 问题

### 问题描述

```markdown
斐波那契数，通常用 F(n) 表示，形成的序列称为斐波那契数列。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
给定 N，计算 F(N)。
```



题目很简单，但是最近学了 dp 状态转移方程 --> 矩阵乘法， 试试水

[题目链接](https://leetcode-cn.com/problems/fibonacci-number/)



### 代码

```c++
class Solution {
public:
    int fib(int N) {
        if (N == 0) return 0;
        if (N <= 2) return 1;
        vector<vector<int>> dp = {{1, 1}}; // dp[2], dp[1]
        vector<vector<int>> ma = {{1,1},{1,0}};
        N -= 2;
        while (N > 1) {        // 快速幂
            if ((N & 1) == 1) dp = mul(dp, ma);
            ma = mul(ma, ma);
            N >>= 1;
        }
        dp = mul(dp, ma);
        return dp[0][0]; // dp[n]
    }

    vector<vector<int>> mul(const vector<vector<int>> & a, const vector<vector<int>> & b) {
        int m = a.size(), n = a[0].size(), k = b[0].size();
        vector<vector<int>> c(m, vector<int> (k));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < k; j++) {
                for (int p = 0; p < n; p++) {
                    c[i][j] += a[i][p] * b[p][j];
                }
            }
        }
        return c;
    }
};
```

