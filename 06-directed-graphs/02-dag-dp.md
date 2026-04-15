# 2. DAG DP

**Difficulty band:** `1500-1800`

## What it solves

Dynamic programming on a directed acyclic graph after topological sorting.

## Core idea

Once vertices are processed in topological order, all predecessors of a vertex
are already finalized.

## Intuition

DAG DP works because a DAG has a natural dependency order. If every edge goes
from earlier to later in topological order, then when you process `u`, all ways
to reach `u` have already been accounted for.

That turns graph DP into ordinary left-to-right DP on the topological order.

## Longest path in a DAG template

```cpp
const long long NEG = -(long long)4e18;
vector<long long> dp(n + 1, NEG);
dp[src] = 0;

for (int u : topo) {
    if (dp[u] == NEG) continue;
    for (auto [v, w] : adj[u]) {
        dp[v] = max(dp[v], dp[u] + w);
    }
}
```

## Common uses

- Longest path in a DAG
- Counting paths in a DAG
- Earliest finish time style dependency problems

## Why it works

The topological order guarantees that every transition into `u` comes from a
node that was processed earlier. So when you update neighbors from `u`, the
value of `dp[u]` is already final.

## Common mistakes

- Forgetting to ensure the graph is acyclic first
- Using a normal DFS order instead of a true topological order
