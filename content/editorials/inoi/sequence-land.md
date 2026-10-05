---
draft: false
title: "Sequence Land"
editorial:
  platform: "INOI"
  name: "Sequence Land"
---

*Editorial written by Sri Vidya Sundar.*

{{< problem "inoi-sequence-land" >}}

## Abridged problem statement
$N$ individuals are assigned a sequence called their ID. Two individuals are said to be related if their IDs contain at least $k$ common values. Given the IDs of all individuals and a specific individual called the President, determine the number of people who are directly or indirectly related to the President.

## Solution
To solve this problem, we need to find a way to store who is directly related to whom. A simple way to do this would be to store these relations in a graph.

How?

Think of each node as an individual and each edge as a connection to a direct relative of this individual. This way, the problem reduces to finding the number of nodes in the President's connected component.

<figure>
  <img src="/sequence-land-example.svg"
       alt="Example graph"
       style="display:block; width:100%; height:auto; margin:0 auto;">

  <figcaption>
    Image generated with the help of
    <a href="https://anacc22.github.io/another_graph_editor/">this website</a>.
  </figcaption>
</figure>

In this graph, if node $1$ is the President, their relatives are all nodes from $1$ to $9$. Nodes $10$ and $11$ are not related to the President. Thus, the result is $9$.

## Implementation
To implement this, we first need a way to identify direct relatives. A simple approach is to sort all the IDs and manually iterate through the values in each pair of IDs. This can be done in $O(n^2 \cdot p )$ time where $n$ is the number of individuals and $p$ is the maximum size of the ID. Since $p$ is at most $300$ and $n$ is at most $300$, this is feasible.
Next, we need to store these relationships. This can be done by maintaining an adjacency list.
We then need to traverse all relatives of the President. For this, we use a depth-first search. We start by processing everyone directly connected to the President. Whenever we encounter a new relative, we add them to the list of relatives and recursively process them as well.
Finally, we print the size of the relatives list.

``` cpp
#include <bits/stdc++.h>

using namespace std;

vector<int> relatives;
vector<vector<int>> adj;
vector<bool> visited;

void dfs(int node) {
  if (visited[node])
    return;
  relatives.push_back(node);
  visited[node] = true;
  for (int i : adj[node])
    dfs(i);
}

int main() {
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);

  int n, k;
  cin >> n >> k;
  vector<vector<int>> IDs;
  adj.resize(n);
  visited.resize(n);

  // Input sequences and sorting them.
  for (int i = 0; i < n; i++) {
    int p;
    cin >> p;
    vector<int> sequence(p);
    for (int j = 0; j < p; j++) {
      cin >> sequence[j];
    }
    sort(sequence.begin(), sequence.end());
    IDs.push_back(sequence);
  }

  for (int i = 0; i < n; i++) {
    for (int j = i + 1; j < n; j++) {
      int common = 0, a = 0;
      for (int b = 0; b < IDs[i].size(); b++) {

        // Move through IDs[j] until we reach a value that is at least IDs[i][b]
        while ((a < IDs[j].size()) && (IDs[j][a] < IDs[i][b])) {
          a++;
        }
        if (IDs[j][a] == IDs[i][b])
          common++;
      }
      if (common >= k) {
        adj[i].push_back(j);
        adj[j].push_back(i);
      }
    }
  }
  int president = 0;
  dfs(president);
  cout << relatives.size();
}
```
Time complexity: $O(n^2 \cdot p)$