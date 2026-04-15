# 1. Topological Sort

**Difficulty band:** `1400-1700`

## What it solves

Produces an ordering of vertices in a DAG so every directed edge `u -> v`
appears with `u` before `v`.

## Kahn's algorithm

```cpp
vector<int> indeg(n + 1);
for (int u = 1; u <= n; u++) {
    for (int v : adj[u]) indeg[v]++;
}

queue<int> q;
for (int i = 1; i <= n; i++) {
    if (indeg[i] == 0) q.push(i);
}

vector<int> topo;
while (!q.empty()) {
    int u = q.front();
    q.pop();
    topo.push_back(u);

    for (int v : adj[u]) {
        indeg[v]--;
        if (indeg[v] == 0) q.push(v);
    }
}
```

## Intuition

If a node has indegree `0`, nothing must come before it. So it is always safe
to place it next in the order.

After removing that node from the graph, some other nodes may now become
indegree `0`, and they become the next valid choices.

That is exactly what Kahn's algorithm simulates.

## Cycle check

If `topo.size() < n`, then the graph contains a cycle and no topological order
exists.

## Complexity

- Time: `O(n + m)`
- Memory: `O(n)`

## Why it works

Every time we pop a node with indegree `0`, we are choosing a node that has no
remaining prerequisites. Removing its outgoing edges updates the same fact for
future nodes.

If a cycle exists, then every node in that cycle always has at least one
incoming edge from inside the cycle, so none of them can ever become indegree
`0`. That is why Kahn's algorithm gets stuck exactly on cyclic graphs.

## Common uses

- Course scheduling
- Dependency resolution
- DAG DP
