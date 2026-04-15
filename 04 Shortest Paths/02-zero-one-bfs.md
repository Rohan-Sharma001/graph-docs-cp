# 2. 0-1 BFS

**Difficulty band:** `1500-1700`

## What it solves

Single-source shortest path when every edge weight is either `0` or `1`.

## Core idea

Use a deque:

- push to the front for weight `0`
- push to the back for weight `1`

## Intuition

The deque keeps vertices almost sorted by distance without needing a full
priority queue.

If you traverse a `0` edge, the new node should be processed immediately,
because its distance did not increase. If you traverse a `1` edge, it belongs
after the current distance layer.

That is why a deque is enough: only two relative priorities ever exist.

## Template

```cpp
const int INF = 1e9;
vector<int> dist(n + 1, INF);
deque<int> dq;

dist[src] = 0;
dq.push_front(src);

while (!dq.empty()) {
    int u = dq.front();
    dq.pop_front();

    for (auto [v, w] : adj[u]) {
        if (dist[v] <= dist[u] + w) continue;
        dist[v] = dist[u] + w;
        if (w == 0) dq.push_front(v);
        else dq.push_back(v);
    }
}
```

## Complexity

- Time: `O(n + m)`
- Memory: `O(n)`

## Why it works

At any moment, the deque stores vertices in nondecreasing distance order up to a
difference of at most `1` between neighboring layers.

- `0` edges keep you in the same layer, so push front
- `1` edges move you to the next layer, so push back

This is exactly the weighted analogue of BFS for binary weights.

## Common uses

- Reversal-cost problems
- Grid problems with cheap and expensive moves
- Binary-weight transitions between states

## When to prefer it over Dijkstra

If all weights are `0` or `1`, this is both simpler and faster than Dijkstra.
