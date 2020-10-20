## 背包问题



### 01背包

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;
int dp[N];
int v[N], w[N];

int main() {
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= n; ++i) cin >> v[i] >> w[i];    
    
    // 状态转移方程 dp[i][j] = max(dp[i-1][j-w] + v, dp[i-1][j-w])
    // 优化版本, 注意第二层循环, 思考优化的根据
    for (int i = 1; i <= n; ++i)
        for (int j = m; j >= v[i]; --j)
                dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
    
    cout << dp[m] << endl;
    return 0;
}
```

 [01背包模板题](https://www.acwing.com/problem/content/2/)





### 完全背包

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;
int dp[N];
int v[N], w[N];

int main() {
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= n; ++i) cin >> v[i] >> w[i];    
/*
f[i,j] = max(f[i-1,j],f[i-1,j-v]+w,f[i-1,j-2*v]+2*w,f[i-1,j-3*v]+3*w, .....)
f[i,j-v]= max(f[i-1,j-v],f[i-1,j-2*v]+w,f[i-1,j-2*v]+2*w,.....)
由上两式，可得出如下递推关系： 
f[i][j]=max(f[i-1][j], f[i,j-v]+w) 
*/
    for (int i = 1; i <= n; ++i) {
        for (int j = v[i]; j <= m; ++j) {
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
        }
    }
    
    cout << dp[m] << endl;
    
    return 0;
}
```

[完全背包模板题](https://www.acwing.com/problem/content/3/)

