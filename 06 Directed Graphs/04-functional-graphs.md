# 4. Functional Graphs

**Difficulty band:** `1600-1800`

## What they are

A functional graph is a directed graph where every vertex has exactly one
outgoing edge.

## Why they matter

They appear constantly in Codeforces because they combine trees and cycles in a
very structured way.

## Core structure

Every connected component of a functional graph consists of:

- one directed cycle
- zero or more directed trees pointing into that cycle

## Intuition

Because every node has exactly one outgoing edge, once you start walking from a
node, you can never branch. Since the graph is finite, repeated walking must
eventually revisit a node, which creates a cycle.

Everything not on the cycle is just a chain or tree feeding into that cycle.

## Useful peeling trick

If you repeatedly remove vertices with indegree `0`, the remaining vertices are
exactly the cycle vertices.

```cpp
vector<int> indeg(n + 1), in_cycle(n + 1, 1);
for (int u = 1; u <= n; u++) indeg[to[u]]++;

queue<int> q;
for (int i = 1; i <= n; i++) {
    if (indeg[i] == 0) q.push(i);
}

while (!q.empty()) {
    int u = q.front();
    q.pop();
    in_cycle[u] = 0;

    int v = to[u];
    indeg[v]--;
    if (indeg[v] == 0) q.push(v);
}
```

## Why peeling works

Any node with indegree `0` cannot lie on a directed cycle, because a cycle node
must have at least one incoming edge from the previous cycle node.

When you remove a non-cycle node, its successor may also drop to indegree `0`,
so the process keeps stripping away tree parts until only cycle nodes remain.

## Common uses

- Distance to cycle
- Cycle length
- K-th jump with binary lifting
- Simulation problems with huge number of moves
