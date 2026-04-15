# 1. Tree Basics

**Difficulty band:** `1200-1400`

## Core facts

- A tree with `n` vertices has exactly `n - 1` edges.
- A tree is connected and acyclic.
- Between any two vertices, there is exactly one simple path.

## Why trees are easier than general graphs

The unique-path property removes a lot of ambiguity:

- there is only one parent choice once the root is fixed
- every child subtree is disjoint from the others
- information can be passed cleanly from children to parent

That is why so many graph problems become much simpler on trees.

## Standard DFS for parent, depth, subtree size

```cpp
vector<int> parent(n + 1), depth(n + 1), sub(n + 1);

void dfs(int u, int p) {
    parent[u] = p;
    sub[u] = 1;

    for (int v : adj[u]) {
        if (v == p) continue;
        depth[v] = depth[u] + 1;
        dfs(v, u);
        sub[u] += sub[v];
    }
}
```

## What this data gives you

- `parent[u]`: parent in the rooted tree
- `depth[u]`: distance from root in edges
- `sub[u]`: size of the subtree of `u`

## Why one DFS is enough

When DFS enters `u`, it knows who the parent is. Then it recursively computes
the same information for every child. After all children return, their subtree
sizes can simply be added into `sub[u]`.

That is the fundamental tree-DP pattern:

1. go from parent to children
2. solve children
3. combine child answers at the parent

## Common uses

- Counting nodes in subtrees
- Tree DP
- LCA preprocessing
- Rerooting style problems

## Complexity

Because a tree has exactly `n - 1` edges, the DFS runs in `O(n)`.
