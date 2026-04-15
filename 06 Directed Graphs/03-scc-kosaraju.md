# 3. Strongly Connected Components

**Difficulty band:** `1700-1800`

## What it solves

In a directed graph, an SCC is a maximal set of vertices where every vertex can
reach every other vertex.

## Kosaraju's algorithm

1. Run DFS on the original graph and store vertices in finish order.
2. Reverse all edges.
3. Process vertices in reverse finish order on the reversed graph.
4. Each DFS in step `3` gives one SCC.

## Intuition

Inside one SCC, every node can circulate to every other node. Between different
SCCs, the graph behaves like a DAG.

The first DFS computes a finishing order that places "downstream" SCCs earlier
and "upstream" SCCs later. Reversing the graph flips all SCC-to-SCC edges, so
processing nodes in reverse finishing order makes each DFS in the reversed graph
stay inside exactly one SCC before leaking anywhere else.

## Template

```cpp
vector<vector<int>> rg(n + 1);
vector<int> order, comp(n + 1, -1), vis(n + 1, 0);

for (int u = 1; u <= n; u++) {
    for (int v : adj[u]) rg[v].push_back(u);
}

void dfs1(int u) {
    vis[u] = 1;
    for (int v : adj[u]) if (!vis[v]) dfs1(v);
    order.push_back(u);
}

void dfs2(int u, int cid) {
    comp[u] = cid;
    for (int v : rg[u]) if (comp[v] == -1) dfs2(v, cid);
}

for (int i = 1; i <= n; i++) {
    if (!vis[i]) dfs1(i);
}

reverse(order.begin(), order.end());
int cid = 0;
for (int u : order) {
    if (comp[u] != -1) continue;
    dfs2(u, cid++);
}
```

## Complexity

- Time: `O(n + m)`
- Memory: `O(n + m)`

## Why it works

The SCC condensation graph is a DAG. The finish order from the first DFS gives a
reverse topological-style order on that DAG. When edges are reversed, starting
from the right SCC in that order ensures DFS captures one whole component at a
time.

## Common uses

- Condensation DAG
- Reachability compression
- Contradiction-style problems on directed graphs

## Follow-up idea

Once `comp[u]` is known for every vertex, you can compress every SCC into one
node and build the condensation DAG.
