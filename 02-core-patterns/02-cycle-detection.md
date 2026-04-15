# 2. Cycle Detection

**Difficulty band:** `1100-1400`

## Undirected graph

Use DFS or BFS and keep track of the parent. If you see a visited neighbor that
is not the parent, then a cycle exists.

### Why the parent check matters

In an undirected graph, if you are at `u` and look at neighbor `p`, you will
always see the edge back to where you just came from. That does **not** mean a
cycle exists.

So:

- visited neighbor equal to parent: ignore it
- visited neighbor different from parent: you found another route back into the
  already explored part, which creates a cycle

```cpp
bool has_cycle = false;

void dfs(int u, int p) {
    vis[u] = 1;
    for (int v : adj[u]) {
        if (v == p) continue;
        if (vis[v]) {
            has_cycle = true;
            continue;
        }
        vis[v] = 1;
        dfs(v, u);
    }
}
```

## Directed graph

Use `3` states:

- `0`: unvisited
- `1`: currently in recursion stack
- `2`: finished

If you reach a vertex with state `1`, you found a back edge and therefore a
cycle.

### Why `3` states are needed

In a directed graph, just knowing "visited or not" is not enough.

- state `0`: we have never seen this node
- state `1`: this node is currently on the recursion stack
- state `2`: this node is fully processed

An edge to state `1` means you found a path from the current node back to an
ancestor in the current DFS chain, which is exactly a directed cycle.

### Why the undirected algorithm fails here

Consider this directed graph:

```text
1 -> 2
1 -> 3
2 -> 3
```

This graph is a DAG, so it has no cycle.

Now suppose you incorrectly use the undirected rule:

- if a neighbor is visited and is not the parent, declare a cycle

Run DFS from `1`:

1. Go from `1` to `2`
2. Go from `2` to `3`
3. Finish `3`, then finish `2`
4. Return to `1` and inspect edge `1 -> 3`

At this point, `3` is already visited and is not the parent of `1`, so the
undirected rule would wrongly say "cycle".

But there is no path from `3` back to `1`, so no directed cycle exists.

This is the key difference:

- in an undirected graph, a visited non-parent neighbor really does imply a
  cycle
- in a directed graph, an edge to an already finished node may just be a
  forward or cross edge, which is completely valid in a DAG

That is why directed cycle detection must distinguish:

- a node currently in the recursion stack, which means cycle
- a node already fully processed, which does not imply cycle

```cpp
bool has_cycle = false;
vector<int> state(n + 1, 0);

void dfs(int u) {
    state[u] = 1;
    for (int v : adj[u]) {
        if (state[v] == 0) dfs(v);
        else if (state[v] == 1) has_cycle = true;
    }
    state[u] = 2;
}
```

## Complexity

- Time: `O(n + m)`
- Memory: `O(n)`

## How to think about correctness

- In the undirected case, a cycle exists exactly when DFS finds a non-parent
  back connection.
- In the directed case, a cycle exists exactly when DFS finds an edge into the
  current recursion stack.

## Common uses

- Detecting whether a graph is a tree
- Validating DAG assumptions
- Finding redundant edges

## Note

For undirected multigraphs with parallel edges, parent-by-vertex is not enough.
In that case, skip the parent edge by edge id instead.
