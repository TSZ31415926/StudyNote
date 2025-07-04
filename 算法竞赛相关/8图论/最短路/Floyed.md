# 介绍
- 多源源最短路
- 可有负权(无负权环)
- 时间复杂度：O(V³) 空间复杂度：O(V²)

# 代码
```C++
vector<vector<int>> dist(n, vector<int>(n, INF));
cin >> n >> m;
for (int i = 1; i <= m; i++){
    int u, v, w;
    cin >> u >> v >> w;
    dis[u][v] = min(dis[u][v], w);
    dis[v][u] = min(dis[v][u], w);
}
for (int k = 0; k < n; k++)
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
```