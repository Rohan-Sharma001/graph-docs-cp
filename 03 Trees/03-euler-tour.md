# 3. Euler Tour on Trees

**Difficulty band:** `1400-1600`

## What it solves

Flatten a tree into an array so subtree queries become segment queries on a
contiguous range.

## Intuition

During DFS, you enter a node, fully process its entire subtree, and only then
leave it. That means all nodes of one subtree are visited as one uninterrupted
block in DFS order.

## Template

```cpp
vector<int> tin(n + 1), tout(n + 1), order;
int timer = 0;

void dfs(int u, int p) {
    tin[u] = timer++;
    order.push_back(u);

    for (int v : adj[u]) {
        if (v == p) continue;
        dfs(v, u);
    }

    tout[u] = timer - 1;
}
```

## Important property

All nodes in the subtree of `u` lie in the range:

```cpp
[tin[u], tout[u]]
```

## Why the range is contiguous

When DFS enters `u`, it cannot visit anything outside the subtree of `u` until
the subtree is completely finished. So every node visited between `tin[u]` and
`tout[u]` belongs to that subtree, and every subtree node appears in that same
block.

That is exactly why subtree queries often become range queries after flattening.

## Common uses

- Subtree sum queries
- Ancestor checks
- Tree updates with Fenwick tree or segment tree

## Ancestor check trick

Node `u` is an ancestor of node `v` if and only if:

```cpp
tin[u] <= tin[v] && tout[v] <= tout[u]
```

## Common mistake

Do not confuse this with an Euler path/circuit in a general graph. This one is
just a DFS ordering technique on trees.
