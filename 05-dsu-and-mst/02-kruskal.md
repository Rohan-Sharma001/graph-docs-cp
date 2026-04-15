# 2. Kruskal

**Difficulty band:** `1400-1700`

## What it solves

Finds a minimum spanning tree of a weighted undirected graph.

## What MST means

MST stands for **minimum spanning tree**.

Break that into parts:

- **spanning**: it includes all `n` vertices
- **tree**: it is connected and has no cycle
- **minimum**: among all spanning trees, its total edge weight is as small as
  possible

So in a connected graph, an MST is a way to connect every vertex using exactly
`n - 1` edges with minimum total cost.

If the graph is disconnected, a full spanning tree does not exist. In that
case, Kruskal gives a minimum spanning **forest** instead.

## Core idea

Sort edges by weight and greedily take an edge if it connects two different
components.

## Intuition

Imagine growing several small trees that slowly merge together. The cheapest
edge that connects two different components is always a safe choice.

Why? Because if you need to connect those two sides somehow, replacing a heavier
crossing edge with a lighter one can only improve the answer.

This is the cut-property intuition behind Kruskal.

Another way to think about it:

- we want all vertices connected in the end
- we never want to create a cycle, because a tree cannot contain cycles
- so we only take an edge when it joins two different components

That is exactly why DSU fits Kruskal so well: it quickly tells us whether an
edge connects two separate components or just creates a useless cycle.

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
