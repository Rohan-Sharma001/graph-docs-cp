# 2. Articulation Points

**Difficulty band:** `1700-1800`

## What it solves

An articulation point is a vertex whose removal increases the number of
connected components.

## Core idea

Use the same `tin` and `low` arrays as bridges.

- For a non-root vertex `u`, if some child `v` has `low[v] >= tin[u]`, then `u`
  is an articulation point.
- For the DFS root, it must have at least two DFS children.

## Intuition

Ask whether removing vertex `u` separates some child subtree from the rest of
the graph.

For a child `v`:

- if `low[v] < tin[u]`, then the subtree of `v` can escape upward without using
  `u`
- if `low[v] >= tin[u]`, then that subtree depends on `u` to stay connected

So in the second case, `u` is a cut vertex.

## Template

```cpp
vector<int> tin(n + 1, -1), low(n + 1, -1), is_cut(n + 1, 0);
int timer = 0;

void dfs(int u, int p) {
    tin[u] = low[u] = timer++;
    int children = 0;

    for (int v : adj[u]) {
        if (v == p) continue;

        if (tin[v] != -1) {
            low[u] = min(low[u], tin[v]);
        } else {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
            if (p != -1 && low[v] >= tin[u]) is_cut[u] = 1;
            children++;
        }
    }

    if (p == -1 && children > 1) is_cut[u] = 1;
}
```

## Complexity

- Time: `O(n + m)`
- Memory: `O(n)`

## Why the root is special

The DFS root has no parent above it. So the condition `low[v] >= tin[u]` is not
enough by itself. The root is an articulation point only if it has at least two
independent DFS children, because then removing it separates those child
subtrees from each other.

## Common uses

- Critical city / server problems
- Splitting networks into fragile points
