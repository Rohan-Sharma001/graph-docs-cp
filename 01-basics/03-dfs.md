# 3. Depth First Search

**Difficulty band:** `800-1100`

## What it solves

- Traverses as deep as possible before backtracking
- Computes components, parent, depth, subtree size, entry times, and more
- Serves as the base for many advanced graph algorithms

## Intuition

DFS keeps choosing one unexplored edge and commits to that direction until it
cannot continue anymore. Only then does it backtrack.

This makes DFS good for problems where the structure of the exploration matters:

- building a DFS tree
- reasoning about ancestors and descendants
- carrying information from children back to the parent
- detecting back edges and cycles

## What state DFS naturally gives you

Even if the problem only asks for reachability, DFS can cheaply produce:

- parent of every node in the DFS tree
- depth in the DFS tree
- subtree sizes
- entry and exit order
- information aggregated from descendants

## Recursive template

```cpp
vector<int> vis(n + 1, 0);

void dfs(int u) {
    vis[u] = 1;
    for (int v : adj[u]) {
        if (vis[v]) continue;
        dfs(v);
    }
}
```

## Iterative template

```cpp
vector<int> vis(n + 1, 0);
stack<int> st;
st.push(src);

while (!st.empty()) {
    int u = st.top();
    st.pop();
    if (vis[u]) continue;
    vis[u] = 1;

    for (int v : adj[u]) {
        if (!vis[v]) st.push(v);
    }
}
```

## Complexity

- Time: `O(n + m)`
- Memory: `O(n)` plus recursion stack if recursive

Like BFS, DFS processes each vertex once and each edge only a constant number of
times. The main difference is the order of exploration, not the asymptotic cost.

## Why DFS is so useful in advanced graph algorithms

The moment you enter a node and the moment you leave it both mean something:

- entering means you are moving from ancestor to descendant
- leaving means the whole subtree has been processed

That is exactly why topological sort, SCC, bridges, articulation points, Euler
tour, subtree DP, and LCA preprocessing all build on DFS.

## How to think about recursive DFS

1. Enter node `u`.
2. Mark it visited.
3. Explore each unvisited neighbor `v`.
4. After returning from `v`, combine whatever `v` computed into `u`.
5. When all children are done, leave `u`.

## When DFS is especially useful

- Connected components
- Cycle detection
- Topological sort
- Tree DP and subtree calculations
- Bridges and articulation points

## Common mistakes

- Stack overflow on deep graphs when recursion depth is too large
- Forgetting to clear `vis` between test cases
- Using DFS when shortest path is required in an unweighted graph
- Forgetting to skip the parent in undirected tree-style DFS
