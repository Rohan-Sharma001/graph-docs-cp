# 1. Graph Representation

**Difficulty band:** `800-1000`

## What it solves

Before running any graph algorithm, we need a data structure that stores edges
efficiently.

## The mental model

A graph algorithm only ever asks some variation of these questions:

- from node `u`, which nodes can I go to next?
- what is the cost of going there?
- do I need to iterate over all edges or only neighbors of one node?

Your representation should make the most common operation cheap.

## Most common representations

- **Adjacency list**: Best default choice for competitive programming.
- **Edge list**: Useful when iterating over all edges, especially in
  Bellman-Ford or Kruskal.
- **Adjacency matrix**: Useful when `n` is small and you need constant-time
  edge checks.

## Why adjacency lists are the default

In most contest graphs, the graph is sparse, which means `m` is much smaller
than `n^2`. With an adjacency list:

- you only store edges that actually exist
- iterating neighbors of `u` takes time proportional to `deg(u)`
- BFS, DFS, Dijkstra, tree DFS, bridges, and SCC all naturally use it

An adjacency matrix is only attractive when the graph is tiny or dense, or when
you really need `is there an edge from u to v?` in `O(1)`.

## Default template for an unweighted graph

```cpp
int n, m;
cin >> n >> m;

vector<vector<int>> adj(n + 1);
for (int i = 0; i < m; i++) {
    int u, v;
    cin >> u >> v;
    adj[u].push_back(v);
    adj[v].push_back(u); // remove this line for directed graphs
}
```

## Template for a weighted graph

```cpp
int n, m;
cin >> n >> m;

vector<vector<pair<int, int>>> adj(n + 1);
for (int i = 0; i < m; i++) {
    int u, v, w;
    cin >> u >> v >> w;
    adj[u].push_back({v, w});
    adj[v].push_back({u, w});
}
```

## When to use what

- Use an adjacency list for BFS, DFS, Dijkstra, tree DFS, bridge finding, and
  almost everything else.
- Use an edge list for Kruskal and Bellman-Ford.
- Use a matrix for Floyd-Warshall or small dense graphs.

## How to choose quickly in a contest

1. If the graph is given as vertices with neighbor edges, start with an
   adjacency list.
2. If the algorithm repeatedly scans all edges in sorted or raw order, keep an
   edge list too.
3. If `n` is small and all-pairs reasoning is needed, consider a matrix.

Many problems use more than one representation at the same time. For example,
Kruskal wants an edge list, while Dijkstra wants an adjacency list.

## Complexity

- Adjacency list space: `O(n + m)`
- Adjacency matrix space: `O(n^2)`

The big difference is not just memory. Traversal also changes:

- with an adjacency list, total neighbor iteration over the whole graph is
  `O(n + m)`
- with an adjacency matrix, checking all possible neighbors of every node is
  often `O(n^2)` even if very few edges exist

## Common mistakes

- Forgetting that an undirected edge must be added in both directions
- Mixing `0`-indexed and `1`-indexed vertices
- Using an adjacency matrix for large sparse graphs
- Forgetting to store weights when the problem is weighted
- Losing edge ids when the problem later asks for bridges, Euler path, or edge
  reconstruction
