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