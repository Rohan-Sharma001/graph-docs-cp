# 1. Disjoint Set Union

**Difficulty band:** `1300-1600`

## What it solves

Maintains which vertices currently belong to the same connected component while
connections are only added over time.

This is useful when you keep merging groups and want to quickly answer:

- are `u` and `v` already in the same group?
- if I connect `u` and `v`, does that merge two different groups?

## Intuition

DSU treats each component as a rooted tree whose root is the component
representative.

- `find(x)` climbs to the representative
- `unite(a, b)` makes one representative point to the other

The magic is that path compression and union by size keep these trees extremely
flat in practice.

You can think of it as a very compact way to store components:

- each node points upward through `p[x]`
- the root is the representative of that whole component
- two nodes are in the same component exactly when their representatives match

DSU does **not** store the full graph structure. It only stores enough
information to know which nodes have been merged together.

## Core operations

- `find(x)`: returns the representative of the component of `x`
- `unite(a, b)`: merges the two components if they are different

In practice, checking whether two nodes are connected is just:

```cpp
find(a) == find(b)
```

## What state this implementation stores

- `p[x]`: parent of `x` in the DSU forest; if `p[x] == x`, then `x` is a root
- `sz[x]`: size of the component whose root is `x`

Only roots have meaningful component sizes. After merging, the losing root no
longer represents a component.

## Template

```cpp
struct DSU {
    vector<int> p, sz;

    DSU(int n) {
        p.resize(n + 1);
        sz.assign(n + 1, 1);
        iota(p.begin(), p.end(), 0);
    }

    int find(int x) {
        if (p[x] == x) return x;
        return p[x] = find(p[x]);
    }

    bool unite(int a, int b) {
        a = find(a);
        b = find(b);
        if (a == b) return false;
        if (sz[a] < sz[b]) swap(a, b);
        p[b] = a;
        sz[a] += sz[b];
        return true;
    }
};
```

## What the template is actually doing

### 1. Constructor

```cpp
DSU(int n) {
    p.resize(n + 1);
    sz.assign(n + 1, 1);
    iota(p.begin(), p.end(), 0);
}
```

Initially every vertex is alone:

- `p[x] = x`: every node is its own representative
- `sz[x] = 1`: every component has size `1`

So before any merges, the components are `{1}`, `{2}`, `{3}`, ...

### 2. `find(x)`

```cpp
int find(int x) {
    if (p[x] == x) return x;
    return p[x] = find(p[x]);
}
```

This walks from `x` to the root of its component and returns that root.

The line

```cpp
p[x] = find(p[x]);
```

is path compression. It rewires `x` to point directly to the root, making later
queries faster.

### 3. `unite(a, b)`

```cpp
bool unite(int a, int b) {
    a = find(a);
    b = find(b);
    if (a == b) return false;
    if (sz[a] < sz[b]) swap(a, b);
    p[b] = a;
    sz[a] += sz[b];
    return true;
}
```

This first converts `a` and `b` into their component representatives.

Then:

- if the representatives are equal, both nodes are already in the same
  component, so nothing changes
- otherwise, attach the smaller component under the bigger one
- update the new root's size

The return value tells you whether a merge actually happened:

- `true`: two different components were merged
- `false`: they were already in the same component

## Small example

Suppose we start with `1, 2, 3, 4` as separate components.

1. `unite(1, 2)` merges `{1}` and `{2}`
2. `unite(3, 4)` merges `{3}` and `{4}`
3. `unite(2, 4)` merges those two larger components

After that, all four nodes have the same representative, so they are in one
component.

## Complexity

Almost constant per operation in practice, usually written as `O(alpha(n))`.

## Why path compression helps

Whenever `find(x)` walks up to the root, it rewires the nodes on that path to
point directly to the root. Future queries on those nodes become much faster.

## Why union by size helps

Always attach the smaller tree under the bigger tree. This prevents the parent
chains from becoming long in the first place.

## Common uses

- Dynamic connectivity with only additions
- Kruskal's algorithm
- Grouping equal or linked items

## What DSU is not doing

- It is not storing adjacency lists or all edges
- It is not finding shortest paths or traversing the graph
- It is not good for edge deletions
- It does not directly list all vertices in a component unless you maintain that
  separately

## Important limitation

Plain DSU does not support deleting edges or answering historical versions by
itself. That needs more advanced structures.
