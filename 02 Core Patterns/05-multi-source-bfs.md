# 5. Multi-Source BFS

**Difficulty band:** `1200-1400`

## What it solves

Find the shortest distance from every node to the nearest source when there are
many starting points.

## Core idea

Push all sources into the queue initially with distance `0`. Then run normal
BFS.

## Intuition

Imagine all sources start spreading at the same time. The first source to reach
a node is the closest one, because BFS expands in increasing order of distance.

Another clean mental model is this:

- create a fake super-source
- connect it to every real source with an edge of weight `0`
- run BFS once from the super-source

Multi-source BFS is just that idea without explicitly adding the extra node.

## Template

```cpp
vector<int> dist(n + 1, -1);
queue<int> q;

for (int s : sources) {
    dist[s] = 0;
    q.push(s);
}

while (!q.empty()) {
    int u = q.front();
    q.pop();

    for (int v : adj[u]) {
        if (dist[v] != -1) continue;
        dist[v] = dist[u] + 1;
        q.push(v);
    }
}
```

## Complexity

- Time: `O(n + m)`
- Memory: `O(n)`

## Why it works

All sources are inserted with distance `0`, so the queue initially contains the
entire first frontier. From then on, BFS expands distance `1`, then distance
`2`, and so on exactly as usual.

So the first time a node is visited, it has been reached from the closest
possible source.

## Common uses

- Nearest hospital / police station type problems
- Spread of fire, infection, or monsters
- Distance to nearest marked node in an unweighted graph
