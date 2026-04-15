# 1. Bridges

**Difficulty band:** `1700-1800`

## What it solves

A bridge is an edge whose removal increases the number of connected components.

## Core idea

Use DFS timestamps:

- `tin[u]`: first time `u` is visited
- `low[u]`: earliest reachable entry time from the subtree of `u`

An edge `u - v` is a bridge if `low[v] > tin[u]`.

## Intuition

Suppose DFS goes from `u` to child `v`.

- if the subtree of `v` can climb back to `u` or above using some back edge,
  then removing `u - v` does not disconnect anything
- if it cannot, then `u - v` is the only connection from that subtree to the
  rest of the graph, so it is a bridge

That exact question is what `low[v]` answers.

## Template

```cpp
vector<int> tin(n + 1, -1), low(n + 1, -1);
int timer = 0;

void dfs(int u, int p) {
    tin[u] = low[u] = timer++;

    for (int v : adj[u]) {
        if (v == p) continue;

        if (tin[v] != -1) {
            low[u] = min(low[u], tin[v]);
        } else {
            dfs(v, u);
            low[u] = min(low[u], low[v]);

            if (low[v] > tin[u]) {
                // (u, v) is a bridge
            }
        }
    }
}
```

## Complexity

- Time: `O(n + m)`
- Memory: `O(n)`

## What `low[u]` really means

`low[u]` is the minimum discovery time reachable from:

- `u` itself
- zero or more tree edges downward
- at most one back edge upward

So if `low[v] > tin[u]`, the subtree of `v` has no alternate route back to `u`
or any ancestor of `u`.

## Common uses

- Finding critical roads
- Compressing a graph into bridge-connected components

## Note

If parallel edges are allowed, skip the parent edge by edge id instead of only
by parent vertex.
