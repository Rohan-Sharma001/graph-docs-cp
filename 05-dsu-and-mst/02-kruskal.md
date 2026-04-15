# 2. Kruskal

**Difficulty band:** `1400-1700`

## What it solves

Finds a minimum spanning tree of a weighted undirected graph.

## Core idea

Sort edges by weight and greedily take an edge if it connects two different
components.

## Intuition

Imagine growing several small trees that slowly merge together. The cheapest
edge that connects two different components is always a safe choice.

Why? Because if you need to connect those two sides somehow, replacing a heavier
crossing edge with a lighter one can only improve the answer.

This is the cut-property intuition behind Kruskal.

## Template

```cpp
sort(edges.begin(), edges.end(), [&](auto a, auto b) {
    return a.w < b.w;
});

DSU dsu(n);
long long mst = 0;
int used = 0;

for (auto e : edges) {
    if (!dsu.unite(e.u, e.v)) continue;
    mst += e.w;
    used++;
}

if (used != n - 1) {
    // graph was disconnected
}
```

## Complexity

- Time: `O(m log m)`
- Memory: `O(n)`

The sort dominates the runtime. DSU operations are essentially constant-time.

## Why it works

At every step, Kruskal chooses the lightest edge that connects two different
components. That edge crosses some cut between the current components, and the
lightest edge across a cut is safe to include in at least one MST.

Repeating this until `n - 1` edges are chosen gives an MST.

## Common uses

- Minimum spanning tree
- Maximum spanning tree after reversing sort order
- Offline connectivity thinking
