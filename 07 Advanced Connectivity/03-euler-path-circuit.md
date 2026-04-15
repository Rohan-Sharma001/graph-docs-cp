# 3. Euler Path and Circuit

**Difficulty band:** `1600-1800`

## What it solves

An Euler path uses every edge exactly once. An Euler circuit does the same and
ends where it starts.

## Conditions in an undirected graph

- Euler circuit: every non-isolated vertex has even degree
- Euler path: exactly `0` or `2` vertices have odd degree

The graph also has to be connected after ignoring isolated vertices.

## Why the degree conditions are true

Every time you enter a vertex along one edge, you must leave it along another
edge, except possibly at the path endpoints.

So:

- in a circuit, every used vertex must have even degree
- in an open path, exactly two vertices can have odd degree: the start and end

## Hierholzer's algorithm

Keep walking unused edges until you get stuck, then backtrack. The backtracking
order gives the answer.

## Intuition

If all unused edges around the current vertex are followed greedily, you create
some cycle or partial trail. If there are still unused edges elsewhere on the
current route, you can start another trail from that vertex and splice it in.

Hierholzer's algorithm does this splicing implicitly through DFS/backtracking.

## Template for an undirected graph

```cpp
vector<vector<pair<int, int>>> adj(n + 1);
vector<int> used(m, 0), ptr(n + 1, 0), path;

void dfs(int u) {
    while (ptr[u] < (int)adj[u].size()) {
        auto [v, id] = adj[u][ptr[u]++];
        if (used[id]) continue;
        used[id] = 1;
        dfs(v);
    }
    path.push_back(u);
}
```

After running `dfs(start)`, reverse `path`.

Pick `start` like this:

- if exactly two vertices have odd degree, start from either odd vertex
- otherwise start from any non-isolated vertex

## Why it works

An edge is used exactly once because of the `used[id]` check. A vertex is added
to the answer only after all of its unused outgoing edges are exhausted, so the
backtracking order naturally stitches smaller closed pieces into one valid Euler
trail.

## Complexity

- Time: `O(n + m)`
- Memory: `O(n + m)`

## Common uses

- Reconstructing a route that uses every road exactly once
- String chaining / domino style problems
