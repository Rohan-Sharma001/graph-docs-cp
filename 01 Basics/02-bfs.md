# 2. Breadth First Search

**Difficulty band:** `800-1100`

## What it solves

- Visits nodes level by level from a source
- Finds shortest distance in an unweighted graph
- Builds layers based on distance

## Core idea

Use a queue. When a node is popped, push all unvisited neighbors and assign
their distance as one more than the current node.

## Intuition

Think of BFS as a wave spreading outward from the source:

- distance `0`: only the source
- distance `1`: all nodes reachable in one edge
- distance `2`: all nodes reachable in two edges

Because the queue processes nodes in the same order they are discovered, BFS
finishes all nodes at distance `d` before moving to distance `d + 1`.

## What state BFS maintains

- `dist[u]`: shortest number of edges from `src` to `u`
- `parent[u]`: previous node on one shortest path
- queue: the current frontier of discovered but not fully processed vertices

## Template

```cpp
vector<int> dist(n + 1, -1), parent(n + 1, -1);
queue<int> q;

dist[src] = 0;
q.push(src);

while (!q.empty()) {
    int u = q.front();
    q.pop();

    for (int v : adj[u]) {
        if (dist[v] != -1) continue;
        dist[v] = dist[u] + 1;
        parent[v] = u;
        q.push(v);
    }
}
```

## Path reconstruction

```cpp
vector<int> path;
for (int v = target; v != -1; v = parent[v]) path.push_back(v);
reverse(path.begin(), path.end());
```

Only reconstruct the path if `dist[target] != -1`.

## Why BFS gives shortest paths

In an unweighted graph, every edge costs exactly `1`. So the best path is the
path with the fewest edges.

The first time BFS reaches a node `v`, it reaches it from some node at the
smallest possible distance. Any later attempt to reach `v` would come from the
same layer or a deeper layer, so it cannot be shorter. That is why the first
assignment to `dist[v]` is already optimal.

## Step-by-step way to think during implementation

1. Mark the source as visited by setting `dist[src] = 0`.
2. Put the source in the queue.
3. Pop the front node `u`.
4. For every neighbor `v`, if it is unseen, assign `dist[v] = dist[u] + 1`.
5. Push `v` into the queue immediately.
6. Repeat until the queue becomes empty.

## Complexity

- Time: `O(n + m)`
- Memory: `O(n)`

Every vertex enters the queue at most once, and every edge is checked a constant
number of times. That is why the total work is linear in the graph size.

## Common uses

- Shortest path in an unweighted graph
- Grid shortest path
- Multi-source BFS
- Bipartite checking by parity

## Common mistakes

- Marking a node visited after popping instead of when pushing
- Using BFS on weighted edges
- Forgetting that disconnected graphs need multiple BFS runs
- Forgetting that the graph may be directed, which changes which edges exist
