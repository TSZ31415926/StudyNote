# 介绍
- 求单源最短路
- 可有负权 
- 时间复杂度：O(V*E)

# 代码
1.准备
```C++
deque<int>dq;
vector<bool>is_inq(n+1,false);//标记是否在队列中
vector<int>cnt(n+1),dis(n+1,uinf);//可能要配合resize/assign
dis[st]=0;
dq.push(st);
is_inq[st]=true;
cnt[st]++;
int sum=0;//LLL优化：队列中元素的路径和
int now_dqsize=0;
```

2. 核心代码
```C++
while(!dq.empty()){
    while(dis[dq.front()]*now_dqsize>sum){// LLL优化：当当前节点距离远大于平均值时，重新放入队尾
        dq.push_back(dq.front());
        dq.pop_front();
    }
    int now_u=dq.front();
    dq.pop_front();
    is_indq[now_u]=false;
    sum-=dis[now_u];    
    now_dqsize--;
    for(auto &[v, w] : adj[u]){
        if(dis[now_u]+w<dis[v]){
            dis[v]=dis[now_u]+w;
        }
        if(!is_indq[v]){
            if(!dq.empty()&&dis[v]<dis[dq.front()]){// SLF优化：更小的距离放队首
                dq.push_front(v);
            }
            else{
                dq.push_back(v);
            }
            is_indq[v]=true;
            sum+=dis[v];
            now_dqsize++;
            if (++cnt[v] > n) { // 负权环检测：入队次数超过n次
                cerr << "Negative cycle detected!" << endl;
                return;
            }
        }
        
    }
}
```

# 一些说明
优化点说明：
1. 双端队列 (Deque):
   - SLF (Small Label First): 新节点的距离小于队首时前插
   - LLL (Large Label Last): 当队首节点距离大于队列平均值时后置

2. 负权环检测:
   - 统计每个节点入队次数，超过n次说明存在负权环

3. 内存优化:
   - 使用引用传递邻接表和距离数组
   - 预分配内存避免动态扩容

4. 数值稳定性:
   - 使用long long防止溢出
   - 处理INF情况避免无效计算
5. inq[u] = false; 是 SPFA 算法的核心机制之一，它：
   - 允许节点多次入队：适应负权边场景。
   - 避免冗余计算：精确控制队列内容。
   - 保证正确性：确保所有可能的更短路径都被处理。

6. 复杂度分析:
   - 平均时间复杂度: O(m) (m为边数)
   - 最坏时间复杂度: O(nm) (与Bellman-Ford相同)