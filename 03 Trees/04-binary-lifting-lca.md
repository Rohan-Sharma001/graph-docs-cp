# 4. Binary Lifting for LCA

**Difficulty band:** `1500-1800`

## What it solves

The lowest common ancestor of `u` and `v` is the deepest node that is an
ancestor of both.

## Preprocessing idea

Store `up[v][j]`, the `2^j`-th ancestor of `v`.

## Intuition

Instead of moving upward one edge at a time, binary lifting lets you jump in
powers of two:

- `up[v][0]`: `1` step up
- `up[v][1]`: `2` steps up
- `up[v][2]`: `4` steps up

This is the same idea as binary exponentiation: build large jumps from smaller
ones so queries become logarithmic.

## Template

```cpp
const int LOG = 20;
vector<array<int, LOG>> up(n + 1);
vector<int> depth(n + 1);

void dfs(int u, int p) {
    up[u][0] = p;
    for (int j = 1; j < LOG; j++) {
        up[u][j] = up[up[u][j - 1]][j - 1];
    }

    for (int v : adj[u]) {
        if (v == p) continue;
        depth[v] = depth[u] + 1;
        dfs(v, u);
    }
}

int lift(int u, int k) {
    for (int j = 0; j < LOG; j++) {
        if (k & (1 << j)) u = up[u][j];
    }
    return u;
}

int lca(int u, int v) {
    if (depth[u] < depth[v]) swap(u, v);
    u = lift(u, depth[u] - depth[v]);
    if (u == v) return u;

    for (int j = LOG - 1; j >= 0; j--) {
        if (up[u][j] != up[v][j]) {
            u = up[u][j];
            v = up[v][j];
        }
    }

    return up[u][0];
}
```

## Complexity

- Preprocessing: `O(n log n)`
- Each query: `O(log n)`

## Why the query works

1. If one node is deeper, lift it until both nodes are at the same depth.
2. If they become equal, that node is the LCA.
3. Otherwise, try the largest jump downward from `LOG - 1` to `0`.
4. Whenever `up[u][j] != up[v][j]`, lift both nodes by `2^j`.
5. After this process, `u` and `v` are children of the LCA, so `up[u][0]` is
   the answer.

The downward loop works because it preserves the invariant that the real LCA is
still above both nodes, while making them climb as much as possible without
crossing it.

## Common uses

- Distance queries on trees
- K-th ancestor queries
- Path logic that needs ancestor relationships

## Common mistakes

- Choosing `LOG` too small
- Forgetting to initialize the root's parent consistently
- Mixing `0`-indexed and `1`-indexed ancestor tables
