# 3. Bellman-Ford

**Difficulty band:** `1600-1800`

## What it solves

Single-source shortest path with negative edges, and it can detect reachable
negative cycles.

## Core idea

Relax all edges `n - 1` times. If one more relaxation is still possible, then a
negative cycle is reachable from the source.

## The key dynamic programming idea

After `k` full relaxation rounds, all shortest paths that use at most `k` edges
have been handled correctly.

Why is that useful? A shortest simple path can use at most `n - 1` edges,
because a simple path cannot repeat vertices. So after `n - 1` rounds, every
normal shortest path must already be finalized unless a reachable negative cycle
exists.

## Template

```cpp
struct Edge {
    int u, v;
    long long w;
};

const long long INF = (long long)4e18;
vector<long long> dist(n + 1, INF);
dist[src] = 0;

for (int i = 1; i <= n - 1; i++) {
    bool changed = false;
    for (auto [u, v, w] : edges) {
        if (dist[u] == INF) continue;
        if (dist[v] > dist[u] + w) {
            dist[v] = dist[u] + w;
            changed = true;
        }
    }
    if (!changed) break;
}

bool has_neg_cycle = false;
for (auto [u, v, w] : edges) {
    if (dist[u] == INF) continue;
    if (dist[v] > dist[u] + w) has_neg_cycle = true;
}
```

## Complexity

- Time: `O(nm)`
- Memory: `O(n)`

## Why the extra round detects negative cycles

If after `n - 1` rounds some edge can still improve a distance, then there must
be a path with more than `n - 1` useful edges.

Any path with more than `n - 1` edges repeats a vertex, which means it contains
a cycle. If using that cycle helps reduce the distance, then that cycle must
have negative total weight.

## Common uses

- Problems with negative edges
- Detecting profitable cycles
- Difference constraints

## When not to use it

If all weights are non-negative, Dijkstra is usually much faster.
