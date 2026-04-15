# 1. Dijkstra

**Difficulty band:** `1400-1700`

## What it solves

Single-source shortest path in a graph with non-negative edge weights.

## Core idea

Always expand the currently known closest vertex using a min-priority queue.

## Intuition

Suppose the queue says node `u` currently has the smallest tentative distance.
If all edge weights are non-negative, then any path that reaches `u` later must
be at least as expensive, not cheaper.

So once `u` is popped with its best-known distance, that distance is final.
This is the greedy heart of Dijkstra.

## What state Dijkstra maintains

- `dist[u]`: best distance found so far
- priority queue: candidates ordered by tentative distance
- optional `parent[u]`: for path reconstruction

## Template

```cpp
const long long INF = (long long)4e18;
vector<long long> dist(n + 1, INF);
priority_queue<pair<long long, int>,
               vector<pair<long long, int>>,
               greater<pair<long long, int>>> pq;

dist[src] = 0;
pq.push({0, src});

while (!pq.empty()) {
    auto [d, u] = pq.top();
    pq.pop();
    if (d != dist[u]) continue;

    for (auto [v, w] : adj[u]) {
        if (dist[v] > d + w) {
            dist[v] = d + w;
            pq.push({dist[v], v});
        }
    }
}
```

## Complexity

- Time: `O((n + m) log n)`
- Memory: `O(n)`

Each successful relaxation may push a new pair into the heap, and every heap
operation costs `O(log n)`. That is where the complexity comes from.

## Why it works

The correctness invariant is:

- when a node `u` is popped with `d == dist[u]`, there is no shorter path to
  `u`

Why? If a shorter path existed, then some vertex on that path with even smaller
distance would have had to be processed earlier and would already have relaxed
`u` to that better value.

This argument breaks the moment negative edges appear, because a path that looks
bad now might become cheaper later through a negative edge.

## Common uses

- Road networks
- State graphs with positive transition costs
- Shortest path with path reconstruction

## Common mistakes

- Using Dijkstra when negative edges exist
- Forgetting the stale-state check `if (d != dist[u]) continue`
- Using `int` when distances need `long long`
- Forgetting that multi-source Dijkstra starts by pushing all sources with `0`
