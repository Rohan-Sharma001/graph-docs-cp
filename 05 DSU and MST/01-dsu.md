# 1. Disjoint Set Union

**Difficulty band:** `1300-1600`

## What it solves

Maintains a partition of vertices into components while edges are added.

## Intuition

DSU treats each component as a rooted tree whose root is the component
representative.

- `find(x)` climbs to the representative
- `unite(a, b)` makes one representative point to the other

The magic is that path compression and union by size keep these trees extremely
flat in practice.

## Core operations

- `find(x)`: returns the representative of the component of `x`
- `unite(a, b)`: merges the two components if they are different

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

## Important limitation

Plain DSU does not support deleting edges or answering historical versions by
itself. That needs more advanced structures.
