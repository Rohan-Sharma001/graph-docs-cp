# 1. Connected Components

**Difficulty band:** `1000-1200`

## What it solves

In an undirected graph, a connected component is a maximal set of vertices
where every pair is reachable from each other.

## Core idea

Run BFS or DFS from every unvisited node. Each run marks exactly one component.

## Intuition

If you start from one node and keep following every possible reachable edge, you
will discover exactly the nodes that belong to the same component and nothing
outside it.

That means:

- one traversal gives you one whole component
- starting from an unvisited node always starts a brand-new component

## Template

```cpp
vector<int> comp(n + 1, -1);
int cid = 0;

for (int i = 1; i <= n; i++) {
    if (comp[i] != -1) continue;

    queue<int> q;
    q.push(i);
    comp[i] = cid;

    while (!q.empty()) {
        int u = q.front();
        q.pop();

        for (int v : adj[u]) {
            if (comp[v] != -1) continue;
            comp[v] = cid;
            q.push(v);
        }
    }

    cid++;
}
```

## Complexity

- Time: `O(n + m)`
- Memory: `O(n)`

Across all component traversals combined, each node is still visited once and
each edge is still checked only a constant number of times.

## Why this is correct

Suppose a BFS/DFS started from `s`.

- Every node it visits is reachable from `s`, so it must belong to the same
  component.
- Every node in the same component is reachable from `s`, so the traversal will
  eventually visit it.

So the visited set is exactly one connected component.

## Common uses

- Counting components
- Checking if the whole graph is connected
- Grouping vertices before answering queries
- Counting how many extra edges are needed to connect the whole graph
