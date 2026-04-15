# 3. Prim

**Difficulty band:** `1500-1700`

## What it solves

Another minimum spanning tree algorithm, often convenient when the graph is
already stored as an adjacency list.

## Core idea

Grow the tree one vertex at a time by always taking the cheapest edge from the
current tree to an outside vertex.

## Intuition

Prim maintains one connected growing tree. At any moment, consider the cut:

- inside: vertices already chosen
- outside: vertices not chosen yet

The cheapest edge crossing that cut is always safe to take. So we keep a
priority queue of candidate crossing edges and repeatedly choose the cheapest
one.

## Template

```cpp
vector<int> used(n + 1, 0);
priority_queue<pair<int, int>,
               vector<pair<int, int>>,
               greater<pair<int, int>>> pq;

long long mst = 0;
pq.push({0, 1});

while (!pq.empty()) {
    auto [w, u] = pq.top();
    pq.pop();
    if (used[u]) continue;
    used[u] = 1;
    mst += w;

    for (auto [v, cost] : adj[u]) {
        if (!used[v]) pq.push({cost, v});
    }
}
```

If some vertex is never marked `used`, then the graph was disconnected and a
spanning tree for the whole graph does not exist.

## Complexity

- Time: `O(m log n)`
- Memory: `O(n)`

## Why it works

Just like Kruskal, Prim relies on the cut property. The difference is only in
how the cut is maintained:

- Kruskal works with many components
- Prim works with one growing component

Both are greedy MST algorithms because the cheapest crossing edge is safe.

## Difference from Kruskal

- Prim grows from vertices
- Kruskal grows from sorted edges
