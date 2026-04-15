# 2. Tree Diameter

**Difficulty band:** `1300-1500`

## What it solves

The diameter of a tree is the maximum distance between any pair of vertices.

## Double BFS / DFS trick

1. Start from any node and find the farthest node `a`.
2. Start from `a` and find the farthest node `b`.
3. Distance from `a` to `b` is the diameter.

## Why this trick works

Trees have unique simple paths. If you start from any node and walk to a
farthest node `a`, that node must lie on some diameter endpoint. Intuitively,
if it were not near an extreme end of the tree, there would still be a longer
way to go.

Then, once you start from one diameter endpoint, the farthest reachable node is
the other endpoint. So the second BFS/DFS recovers the full diameter.

## BFS helper

```cpp
pair<int, int> bfs_far(int src) {
    vector<int> dist(n + 1, -1);
    queue<int> q;
    q.push(src);
    dist[src] = 0;

    int best = src;
    while (!q.empty()) {
        int u = q.front();
        q.pop();

        if (dist[u] > dist[best]) best = u;

        for (int v : adj[u]) {
            if (dist[v] != -1) continue;
            dist[v] = dist[u] + 1;
            q.push(v);
        }
    }

    return {best, dist[best]};
}
```

## Complexity

- Time: `O(n)`
- Memory: `O(n)`

You only run a linear traversal twice, and each traversal touches every edge
once.

## Common uses

- Longest path in a tree
- Tree center problems
- Distance to one of the diameter endpoints
