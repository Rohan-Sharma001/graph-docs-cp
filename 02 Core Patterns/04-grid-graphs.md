# 4. Grid Graphs

**Difficulty band:** `1000-1300`

## What it solves

Many problems are not given as explicit graphs. A grid often hides a graph where
each cell is a node and moves define edges.

## Intuition

A grid problem becomes a graph problem the moment you say:

- each valid cell is a vertex
- each legal move is an edge

Once you do that, flood fill is just DFS/BFS, shortest path is just BFS or
Dijkstra, and connected regions are just connected components.

## Common directions

```cpp
int dx[4] = {-1, 0, 1, 0};
int dy[4] = {0, 1, 0, -1};
```

## BFS template on a grid

```cpp
queue<pair<int, int>> q;
vector<vector<int>> dist(n, vector<int>(m, -1));

dist[sx][sy] = 0;
q.push({sx, sy});

while (!q.empty()) {
    auto [x, y] = q.front();
    q.pop();

    for (int dir = 0; dir < 4; dir++) {
        int nx = x + dx[dir];
        int ny = y + dy[dir];

        if (nx < 0 || nx >= n || ny < 0 || ny >= m) continue;
        if (grid[nx][ny] == '#') continue;
        if (dist[nx][ny] != -1) continue;

        dist[nx][ny] = dist[x][y] + 1;
        q.push({nx, ny});
    }
}
```

## How to model a grid correctly

1. Decide what a node is: usually a cell, but sometimes it is a state like
   `(x, y, direction)`.
2. Decide what edges exist: 4-direction, 8-direction, knight moves, teleport
   edges, and so on.
3. Decide if moves are unweighted, binary-weighted, or generally weighted.
4. Then choose BFS, `0-1 BFS`, or Dijkstra accordingly.

## Why this viewpoint helps

It prevents you from memorizing "grid tricks" as separate ideas. Most of them
are just graph algorithms running on an implicit graph rather than an explicit
adjacency list.

## Common uses

- Flood fill
- Maze shortest path
- Counting connected regions
- Multi-source spreading processes

## Complexity

If each cell has only a constant number of possible moves, then the graph has
`O(nm)` vertices and `O(nm)` edges. That is why BFS/DFS on a grid is usually
linear in the number of cells.

## Common mistakes

- Forgetting bounds checks
- Forgetting walls or blocked cells
- Mixing row/column order
- Forgetting that state-space grids can multiply the number of nodes
