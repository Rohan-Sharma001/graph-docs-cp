# 3. Bipartite Check

**Difficulty band:** `1100-1300`

## What it solves

A graph is bipartite if we can color every node with one of two colors so that
every edge connects different colors.

## Core idea

Run BFS or DFS and color neighbors with the opposite color. If an edge connects
two vertices with the same color, the graph is not bipartite.

## Intuition

Pick any node and call it color `0`. Then all its neighbors must be color `1`.
Then all their neighbors must be color `0`, and so on.

So bipartite checking is really a consistency check:

- can this parity assignment continue forever without contradiction?
- or does some edge force two same-colored nodes to be adjacent?

## Important theorem to remember

An undirected graph is bipartite if and only if it has no odd cycle.

That is why bipartite problems often secretly become parity or odd/even path
problems.

## Template

```cpp
vector<int> color(n + 1, -1);
bool ok = true;

for (int i = 1; i <= n; i++) {
    if (color[i] != -1) continue;

    queue<int> q;
    q.push(i);
    color[i] = 0;

    while (!q.empty()) {
        int u = q.front();
        q.pop();

        for (int v : adj[u]) {
            if (color[v] == -1) {
                color[v] = color[u] ^ 1;
                q.push(v);
            } else if (color[v] == color[u]) {
                ok = false;
            }
        }
    }
}
```

## Complexity

- Time: `O(n + m)`
- Memory: `O(n)`

## Why it works

Whenever we traverse an edge `u - v`, we demand:

```cpp
color[v] = color[u] ^ 1
```

If that assignment is impossible because `color[v]` already equals `color[u]`,
then there is no valid 2-coloring.

If the traversal finishes without contradiction, then every edge joins opposite
colors, so the graph is bipartite.

## Common uses

- Splitting people into two groups with conflict edges
- Checking odd cycle existence
- Building parity-based constraints
