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





### 多重背包

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 110;

int n, m;
int v[N], w[N], s[N];
int f[N][N];

int main() {
    cin >> n >> m;

    for (int i = 1; i <= n; i ++ ) cin >> v[i] >> w[i] >> s[i];

    for (int i = 1; i <= n; i ++ )
        for (int j = 0; j <= m; j ++ )
            for (int k = 0; k <= s[i] && k * v[i] <= j; k ++ )
                f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);

    cout << f[n][m] << endl;
    return 0;
}
```

[多重背包模板题](https://www.acwing.com/problem/content/4/)





### 多重背包优化

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 12010, M = 2010;

int n, m;
int v[N], w[N];
int f[M];

int main() {
    cin >> n >> m;

    int cnt = 0;
    for (int i = 1; i <= n; i ++ ) {
        int a, b, s;
        cin >> a >> b >> s;
        int k = 1;
        while (k <= s) {
            cnt ++ ;
            v[cnt] = a * k;
            w[cnt] = b * k;
            s -= k;
            k *= 2;
        }
        
        if (s > 0) {
            cnt ++ ;
            v[cnt] = a * s;
            w[cnt] = b * s;
        }
    }

    n = cnt;

    // 转化为 0 1 背包问题
    for (int i = 1; i <= n; i ++ )
        for (int j = m; j >= v[i]; j -- )
            f[j] = max(f[j], f[j - v[i]] + w[i]);

    cout << f[m] << endl;

    return 0;
}
```

[多重背包优化模板题](https://www.acwing.com/problem/content/5/)





### 分组背包

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;

int n, m;
int v[N][N], w[N][N], s[N];
int f[N];

int main() {
    cin >> n >> m;

    for (int i = 1; i <= n; i ++ ) {
        cin >> s[i];
        for (int j = 0; j < s[i]; j ++ )
            cin >> v[i][j] >> w[i][j];
    }

    for (int i = 1; i <= n; i ++ )
        for (int j = m; j >= 0; j -- )
            for (int k = 0; k < s[i]; k ++ )
                if (v[i][k] <= j)
                    f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);

    cout << f[m] << endl;

    return 0;
}
```

[分组背包模板题](https://www.acwing.com/problem/content/9/)

