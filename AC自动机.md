## AC自动机



### 原理讲解文章

[KMP模板](https://github.com/qzylalala/Algorithm-template/blob/master/day4.md)

[Fail指针讲解](https://blog.csdn.net/wyxeainn/article/details/77430218)



### 代码部分

```c++

struct Tree//字典树 
{
     int fail;//失配指针
     int son[26];//子节点的位置
     int end;//标记有几个单词以这个节点结尾 
}AC[1000010];//Trie树
int cnt = 0;//Trie的指针 

inline void Build(string s)
{
        int len = s.length();
        int now = 0;//字典树的当前指针 
        for(int i = 0; i < len; ++i)//构造Trie树
        {
                if(AC[now].son[s[i]-'a'] == 0)//Trie树没有这个子节点
                   AC[now].son[s[i]-'a']= ++cnt;//构造出来
                now = AC[now].son[s[i]-'a'];//向下构造 
        }
        AC[now].end += 1;//标记单词结尾 
}

void Get_fail()//构造fail指针
{
        queue<int> Q;//队列 
        for(int i = 0; i < 26; ++i) {//第二层的fail指针提前处理一下
            if(AC[0].son[i] != 0) {
                AC[AC[0].son[i]].fail = 0;//指向根节点
                Q.push(AC[0].son[i]);//压入队列 
            }
        }
        while(!Q.empty()) {//BFS求fail指针 
            int u = Q.front();
            Q.pop();
            for(int i = 0;i < 26; ++i) {//枚举所有子节点(仅有小写字母 26个)
                if(AC[u].son[i] != 0) { //存在这个子节点
                    AC[AC[u].son[i]].fail=AC[AC[u].fail].son[i];
                    //子节点的fail指针指向当前节点的
                    //fail指针所指向的节点的相同子节点 
                    Q.push(AC[u].son[i]);//压入队列 
                }
                else AC[u].son[i] = AC[AC[u].fail].son[i];
                      //当前节点的这个子节点指向当
                      //前节点fail指针的这个子节点 
            }
        }
}

int query(string s)//AC自动机匹配
{
        int len = s.length();
        int now = 0,ans = 0;
        for(int i = 0; i < len; ++i)
        {
                now = AC[now].son[s[i]-'a'];//向下一层
                for(int t = now; t && AC[t].end != -1; t = AC[t].fail)//循环求解
                {
                         ans += AC[t].end;
                         AC[t].end = -1; // 这里是由于同一字符串只出现一次(一般不需要这一行)
                } 
        }
        return ans;
}
int main()
{
     int n;
     string s;
     cin >> n;
     for(int i = 1;i <= n; ++i)
     {
             cin >> s;
             Build(s);
     }
     AC[0].fail=0;//结束标志 
     Get_fail();//求出失配指针
     cin >> s;//文本串 
     cout<< query(s) << endl;
     return 0;
}
```

[AC自动机模板题](https://www.luogu.com.cn/problem/P3808)
