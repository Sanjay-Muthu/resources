---
draft: false
title: 'Free Ticket'
editorial:
  platform: "INOI"
  name: "Free Ticket"
---

*Editorial written by Ekansh Majumder.*

{{< problem "inoi-free-ticket" >}}

This solution requires knowledge of BFS, tree diameters, Floyd–Warshall, or Dijkstra's algorithm. Some material will be attached for reference when it is required.

## Subtask #1

In this subtask, $F = C - 1$ and $p = 1$ for each direct flight.

$F = C - 1$ tells us that the given airline network is a tree. 

This boils down the problem to finding the shortest path in an unweighted tree, which is equivalent to finding the diameter of the tree (refer to page 135 [here](https://cses.fi/book/book.pdf)).

This can be done using BFS or DFS. We will be using DFS for our solution.

```cpp
int maxDist = -1, farthestNode = 1;

void dfs(int u, int p, int d, vector<vector<int>> &adj) {
  if (d > maxDist) {
    maxDist = d;
    farthestNode = u;
  }
  for (int v : adj[u]) {
    if (v != p) {
      dfs(v, u, d + 1, adj);
    }
  }
}
```

In `main()`:

```cpp
dfs(1, 0, 0, adj);
maxDist = -1;
dfs(farthestNode, 0, 0, adj);
```

The time complexity of this solution is $\mathcal{O}(C + F) = \mathcal{O}(C)$.

## Subtask #2

In this subtask, $p = 1$ for each direct flight.

There is no constraint on the shape of the graph anymore. 

As all the edge weights are $1$, the shortest paths among all pairs can be computed with a standard unweighted BFS from every node (refer to page 119 [here](https://cses.fi/book/book.pdf)).

```cpp
int ans = 0;
for (int i = 1; i <= C; i++) {
  vector<int> dist(C + 1, -1);
  queue<int> q;
  dist[i] = 0;
  q.push(i);

  while (!q.empty()) {
    int u = q.front();
    q.pop();

    ans = max(ans, dist[u]);

    for (int v : adj[u]) {
      if (dist[v] == -1) {
        dist[v] = dist[u] + 1;
        q.push(v);
      }
    }
  }
}
```

The time complexity of this solution is $\mathcal{O}(C \cdot (C + F))$.

## Full Solution #1

The first solution uses the Floyd–Warshall algorithm to compute the shortest path among all pairs (refer to page 129 [here](https://cses.fi/book/book.pdf)).

The Floyd–Warshall algorithm runs in $\mathcal{O}(C^3)$ time, which we can afford since $C \le 230$.

```cpp
for (int k = 1; k <= C; k++) {
  for (int i = 1; i <= C; i++) {
    for (int j = 1; j <= C; j++) {
      if (dist[i][k] < INF && dist[k][j] < INF) {
        dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
      }
    }
  }
}

// Find the maximum shortest path cost across all pairs
int maxCheapest = 0;
for (int i = 1; i <= C; i++) {
  for (int j = i + 1; j <= C; j++) {
    if (dist[i][j] < INF) {
      maxCheapest = max(maxCheapest, dist[i][j]);
    }
  }
}
```

The time complexity of this solution is $\mathcal{O}(C^3)$.

## Full Solution #2

The second solution uses multi-source Dijkstra.

We run Dijkstra's algorithm from every vertex to compute the shortest path among all pairs. This approach is more efficient in sparse graphs (refer to page 126 [here](https://cses.fi/book/book.pdf)).

```cpp
int ans = 0;
for (int source = 1; source <= C; source++) {
  vector<int> dist(C + 1, INF);
  // Priority queue stores {distance, vertex}
  priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

  dist[source] = 0;
  pq.push({0, source});

  while (!pq.empty()) {
    auto [d, u] = pq.top();
    pq.pop();
    if (d > dist[u]) continue;
    for (auto& edge : adj[u]) {
      int v = edge.first;
      int w = edge.second;
      if (dist[u] + w < dist[v]) {
        dist[v] = dist[u] + w;
        pq.push({dist[v], v});
      }
    }
  }
  for (int v = source + 1; v <= C; v++) {
    if (dist[v] < INF) {
      ans = max(ans, dist[v]);
    }
  }
}
```

The time complexity of this solution is $\mathcal{O}(C \cdot (F \log C))$.