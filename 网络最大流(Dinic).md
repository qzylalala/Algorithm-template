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
	将正向边和反向边存在“2和3”、“4和5”、“6和7”。因为在更新边权的时候，我们就可以直接使用xor的方			式，找到对应的正向边和反向边
```



### Dinic算法

#### 思路

- *D**i**n**i**c*算法框架

1. 在残量网络上 BFS 求出节点的层次，构造分层图
2. 在分层图上 DFS 寻找增广路，在回溯时同时更新边权



#### 代码实现

```c++
// 分层图 + DFS  可以处理 10^4 ~ 10^5 规模的网络

#include <bits/stdc++.h>
using namespace std;
const long long inf=2005020600;
int n,m,s,t,u,v;   // n点  m边  s源点  t汇点  u->v
long long w,ans,dis[520010]; // w边权   ans最大流  dis->深度
int tot = 1, now[520010], head[520010]; 
// tot 加边计数器     
// now   ?         
// head[x] -> 节点 x 对应的最后一条边 的序号
// e[head[x]].net  ->  节点 x 对应的 倒数第二条边 的序号
// 节点 x 对应的 第一条边的序号 满足 e[最后一条边的序号].net = 0;

struct node {
	int to, net;  // to -> 目的节点序号       net -> 
	long long val; // 流量
} e[520010];  // 存边

void add(int u,int v,long long w) {
    // 加 正向边 (注意，从 2, 3 开始)
	e[++tot].to = v;
	e[tot].val = w;
	e[tot].net = head[u];
	head[u] = tot;
	
    // 加 反向边
	e[++tot].to = u;
	e[tot].val = 0;
	e[tot].net = head[v];
	head[v] = tot;
}

int bfs() {  //在残量网络中构造分层图 
	for(int i = 1; i <= n; i++) dis[i] = inf;
    
	queue<int> q;
	q.push(s);
	dis[s] = 0;
	now[s] = head[s];
    // bfs 分层
	while(!q.empty()) {
		int x = q.front();
		q.pop();
		for(int i = head[x]; i; i = e[i].net) {   // 循环逻辑看上面 head 注释
			int v = e[i].to;
			if(e[i].val > 0 && dis[v] == inf) {   // 正向边 且 节点 v 未访问过
				q.push(v);
				now[v] = head[v];
				dis[v] = dis[x] + 1;              // 深度计算
				if(v == t) return 1; // 到达汇点, 结束
			}
		}
	}
	return 0;
}

int dfs(int x, long long sum) {  //sum是整条增广路对最大流的贡献
	if(x == t) return sum;  // 到达汇点
	long long k, res = 0;  //k是当前最小的剩余容量 
	for(int i = now[x]; i && sum; i = e[i].net) {
		now[x] = i;  //当前弧优化 
		int v = e[i].to;
		if(e[i].val > 0 && (dis[v] == dis[x] + 1)) {
			k = dfs(v, min(sum, e[i].val));
			if(k == 0) dis[v] = inf;  //剪枝，去掉增广完毕的点 
			e[i].val -= k;
			e[i^1].val += k;
			res += k;  //res表示经过该点的所有流量和（相当于流出的总量） 
			sum -= k;  //sum表示经过该点的剩余流量 
		}
	}
	return res;
}

int main() {
    // 1. 初始化
	scanf("%d%d%d%d",&n,&m,&s,&t);
	for(int i = 1; i <= m;i++) {
		scanf("%d%d%lld", &u, &v, &w);
		add(u, v, w);
	}
    
    // 2. bfs 分层   +   dfs 寻找增广路(回溯时 更新边权)
	while(bfs()) {
		ans += dfs(s, inf);  //流量守恒（流入=流出） 
	}
	printf("%lld",ans);
	return 0;
}
```

[最大流模板题](https://www.luogu.com.cn/problem/P3376)

[参考题解](https://www.luogu.com.cn/problem/solution/P3376)