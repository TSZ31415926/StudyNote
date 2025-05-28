# 介绍
- 求单源最短路
- 无负权 
- 时间复杂度：O((V+E)logV)
  
# 代码
1. 准备
```C++
vector<vector<pair<int,int>>>mp;
vector<int>dis;
priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>>p_que;
mp.resize(n+1);
dis.assign(n+1,uinf);
```

2.核心代码
```C++
while(!p_que.empty()){
    dis[st]=0;
    p_que.push({0,1});//注意前面是距离，后面才是点
    int now_dis=p_que.top().first;
    int now_u=p_que.top().second;
    p_que.pop();
    if(now_dis>dis[now_u]){
        continue;
    }
    for(auto &edge:mp[now_u]){
        int dist=edge.first;
        int v=edge.second;
        if(dis[now_u]+dist<dis[v]){
            dis[v]=dis[now_u]+dis;
            p_que.push({dis[v],v});
        }
    }
}
```
# 题型类别
## 最短路计数
```C++
void solve()
{
    int n, m;
    cin >> n >> m;
    vector<vector<int>> tree(n + 1);
    vector<int> ans(n + 1, 0), dis(n + 1, uinf);
    // 构建图
    for (int i = 1; i <= m; i++)
    {
        int u, v;
        cin >> u >> v;
        tree[u].push_back(v);
        tree[v].push_back(u);
    }
    // 使用小顶堆的优先队列（距离，节点）
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> p_que;
    // 初始化起点
    dis[1] = 0;
    ans[1] = 1; // 起点到自身有1种路径
    p_que.push({0, 1});
    while (!p_que.empty())
    {
        auto [d, u] = p_que.top();
        p_que.pop();
        // 跳过过时的队列条目
        if (d != dis[u])
            continue;
        // 遍历所有邻居
        for (int v : tree[u])
        {
            // 找到更短路径：重置计数
            if (dis[v] > dis[u] + 1)
            {
                dis[v] = dis[u] + 1;
                ans[v] = ans[u]; // 重置为当前路径的计数
                p_que.push({dis[v], v});
            }
            // 找到等长路径：累加计数
            else if (dis[v] == dis[u] + 1)
            {
                ans[v] = (ans[v] + ans[u]) % mod;
            }
        }
    }
    // 输出每个节点的最短路径数量
    for (int i = 1; i <= n; i++)
    {
        cout << ans[i] << '\n';
    }
}
```