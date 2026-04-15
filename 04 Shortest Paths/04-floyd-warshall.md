# 4. Floyd-Warshall

**Difficulty band:** `1600-1800`

## What it solves

All-pairs shortest path for small graphs.

## Core idea

Try every vertex as an intermediate vertex between every ordered pair.

## The DP viewpoint

Let `dist[i][j]` mean the best distance from `i` to `j` using only intermediate
vertices from some allowed set.

When you consider a new intermediate vertex `k`, every shortest path from `i` to
`j` either:

- does not use `k`, so the old answer stays
- or uses `k`, so the path becomes `i -> k -> j`

That is why the transition is:

```cpp
dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
```

## Template

```cpp
const long long INF = (long long)4e18;
vector<vector<long long>> dist(n, vector<long long>(n, INF));

for (int i = 0; i < n; i++) dist[i][i] = 0;
for (auto [u, v, w] : edges) {
    dist[u][v] = min(dist[u][v], w);
}

for (int k = 0; k < n; k++) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (dist[i][k] == INF || dist[k][j] == INF) continue;
            dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
        }
    }
}
```

## Complexity

- Time: `O(n^3)`
- Memory: `O(n^2)`

## Why it works

The outer loop fixes which intermediate vertices are allowed so far. After
processing `k`, every `dist[i][j]` is correct assuming only vertices
`0..k` may appear as intermediates.

That induction is the whole proof.

## When it is worth using

- `n` is small, often `<= 500`
- You need many path queries after preprocessing
- The graph can be dense

## Extra use

The same structure is also useful for transitive closure, minimum cycle work,
and many small-`n` DP-on-graph problems.
