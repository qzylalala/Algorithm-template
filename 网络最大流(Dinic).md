## 网络最大流



### 基本概念

```markdown
对于任意一张有向图（也就是网络），其中有N个点、M条边以及源点S和汇点T
	c(x,y)称为边的容量 
	f(x,y)称为边的流量

流函数f(x,y)三大性质:
	容量限制：每条边的流量总不可能大于该边的容量的（不然水管就爆了）
	斜对称：正向边的流量=反向边的流量（反向边后面会具体讲）
	流量守恒：正向的所有流量和=反向的所有流量和（就是总量始终不变）
最大流
	使得整个网络流量之和最大的流函数称为网络的最大流，此时的流量和被称为网络的最大流量
增广路	
	若一条从S到T的路径上所有边的剩余容量都大于0，则称这样的路径为一条增广路
反向边
	因为可能一条边可以被包含于多条增广路径，所以为了寻找所有的增广路经我们就要让这一条边有多次被选择的机会
	构建反向边则是这样一个机会，相当于给程序一个反悔的机会。
残量网络	
	在任意时刻，网络中所有节点以及剩余容量大于0的边构成的子图被称为残量网络

邻接表“成对存储”
	将正向边和反向边存在“2和3”、“4和5”、“6和7”。因为在更新边权的时候，我们就可以直接使用xor的方
	式，找到对应的正向边和反向边
```




### Dinic算法

#### 思路

- *D**i**n**i**c*算法框架

1. 在残量网络上 BFS 求出节点的层次，构造分层图
2. 在分层图上 DFS 寻找增广路，在回溯时同时更新边权




### 代码实现(固定风格)

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;

const LL inf = INT_MAX;
const int N = 520010;
int n, m, s, t, now[N];
LL idx = 1, h[N], ne[N], e[N], val[N], dep[N];

void add(int u, int v, LL w) {
	e[++idx] = v;
	val[idx] = w;
	ne[idx] = h[u];
	h[u] = idx;
}


int bfs() {  //在残量网络中构造分层图 
	for(int i = 1; i <= n; i++) dep[i] = inf;
    
	queue<int> q;
	q.push(s);
	dep[s] = 0;
	now[s] = h[s];
    // bfs 分层
	while(!q.empty()) {
		int x = q.front();
		q.pop();
		for(int i = h[x]; i != -1; i = ne[i]) {   // 循环逻辑看上面 head 注释
			int v = e[i];
			if(val[i] > 0 && dep[v] == inf) {   // 正向边 且 节点 v 未访问过
				q.push(v);
				now[v] = h[v];
				dep[v] = dep[x] + 1;              // 深度计算
				if(v == t) return 1; // 到达汇点, 结束
			}
		}
	}
	return 0;
}


int dfs(int x, long long sum) {  //sum是整条增广路对最大流的贡献
	if(x == t) return sum;  // 到达汇点
	long long k, res = 0;  //k是当前最小的剩余容量 
	for(int i = now[x]; i != -1 && sum; i = ne[i]) {
		now[x] = i;  //当前弧优化 
		int v = e[i];
		if(val[i] > 0 && (dep[v] == dep[x] + 1)) {
			k = dfs(v, min(sum, val[i]));
			if(k == 0) dep[v] = inf;  //剪枝，去掉增广完毕的点 
			val[i] -= k;
			val[i^1] += k;
			res += k;  //res表示经过该点的所有流量和（相当于流出的总量） 
			sum -= k;  //sum表示经过该点的剩余流量 
		}
	}
	return res;
}

int main() {
    // 1. 初始化
    memset(h, -1, sizeof(h));
    scanf("%d%d%d%d",&n,&m,&s,&t);
    for(int i = 1; i <= m;i++) {
	int u, v;
	LL w;
	scanf("%d%d%lld", &u, &v, &w);
	add(u, v, w);
	add(v, u, 0);
    }
    
    // 2. bfs 分层   +   dfs 寻找增广路(回溯时 更新边权)
    LL ans = 0;
    while(bfs()) {
	ans += dfs(s, inf);  //流量守恒（流入=流出） 
    }
    printf("%lld",ans);
    return 0;
}
```





[最大流模板题](https://www.luogu.com.cn/problem/P3376)

[参考题解](https://www.luogu.com.cn/problem/solution/P3376)
