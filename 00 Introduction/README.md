# 00. TF is a graph?

In competitive programming, graphs are just a way to represent
**connections**. We have **nodes** (things) and **edges** (relationships
between those things).

Those things could be cities, people, tasks, grid cells, or even abstract
states. If a problem is really about reachability, shortest paths, cycles, or
dependencies, there is a good chance a graph is hiding underneath.

This is why the idea of graphs is so common when it comes to competitive programming and even development.

 Let's take an example: 
||| Illustration
![](/media/image.png)
||| Statement
Let there be cities 1,2,3,4,5,6 and we want to represent how these cities are connected by roads to each other.<br>
In this diagram each node represents a city and edge between two nodes exists **if and only if** the corresponding cities are directly connected to each other.<br>
This makes it easy to visualise the connection and do any required operations. Want to find the minimum distance between two cites? Easy. Want to find the city that can serve as a hub? No problem.
|||

### Types of graphs
#### Based on edge direction
- **Undirected graph**: In an undirected graph the order in which nodes are connected doesn't matter. The example you saw above is an undirected graph. The order of cities doesn't change the fact whether they are connected or not.
- **Directed graph**: Here order of nodes matters. If we want to represent a family tree we can use an arrow from one node to another if the latter is the child of the former.

#### Based on edge weights
- **Weighted graph**: Each edge may have an associated cost. Length of the edge is one way to represent it. Maps use weighted graphs under the hood to represent distance between two places.
- **Unweighted graph**: A graph in which all edges are treated equally, usually with weight `1`.

#### Based on structure or special property
- **Simple graph**: A graph with no self-loops and no multiple edges between the same pair of vertices.
- **Multigraph**: A graph that may contain multiple edges between the same pair of vertices.
- **Complete graph**: A graph in which every pair of distinct vertices is connected by an edge.
- **Bipartite graph**: A graph whose vertices can be divided into two sets such that every edge joins a vertex from one set to the other.
  - **Complete bipartite graph**: A bipartite graph where every vertex of one set is connected to every vertex of the other set.
- **Tree**: A connected acyclic undirected graph.
- **Forest**: A collection of one or more disjoint trees, or equivalently an acyclic undirected graph.
- **Directed Acyclic Graph(DAG)**: A directed graph with no directed cycles.
- **Regular graph**: A graph in which all vertices have the same degree.

#### Based on connectivity or cycles
- **Connected graph**: In an undirected graph, every vertex is reachable from every other vertex.
- **Disconnected graph**: A graph that is not connected.
- **Strongly connected graph**: In a directed graph, every vertex is reachable from every other vertex by following edge directions.
- **Weakly connected graph**: A directed graph that becomes connected if we ignore edge directions.
- **Cyclic graph**: A graph in which at least one cycle exists.
- **Acyclic graph**: A graph in which no cycle exists.

#### Based on number of edges
- **Sparse graph**: A graph with relatively few edges compared to the maximum possible number of edges.
- **Dense graph**: A graph with an edge count close to the maximum possible number of edges.

### Characteristics of graphs
- **Order of graph**: Defined as the number of vertices present in the graph.
- **Size of graph**: Defined as the number of edges present in the graph.
- **Degree(of vertex)**: Defined as the number of edges incident on a given vertex in an undirected graph.
  - **Indegree**: Number of incoming edges to a vertex in a directed graph.
  - **Outdegree**: Number of outgoing edges from a vertex in a directed graph.
  - **Isolated vertex**: A vertex with degree `0`.
  - **Leaf / Pendant vertex**: A vertex with degree `1` in an undirected graph.
- **Subgraph**: From the original graph take some of the vertices. If an edge connects vertices that aren't included, remove it. Then remove some(or none or all) of the remaining edges.
- **Walk**: Sequence of vertices where each consecutive pair is connected by an edge. Vertices and edges may repeat.
- **Trail**: A walk in which no edge is repeated.
- **Path**: Sequence of vertices where each vertex is connected to the next vertex. In a simple path all vertices are distinct.
- **Cycle**: A path where the first and last vertices are the same.
- **Connected component**: A maximal connected subgraph.
- **Self-loop**: An edge that starts and ends at the same vertex.
- **Parallel edges**: Two or more edges connecting the same pair of vertices.
- **Bridge / Cut-edge**: An edge whose removal increases the number of connected components of the graph.
- **Articulation point / Cut-vertex**: A vertex whose removal increases the number of connected components of the graph.
- **Euler path**: A path that uses every edge exactly once.
- **Euler circuit**: A cycle that uses every edge exactly once.
- **Hamiltonian path**: A path that visits every vertex exactly once.
- **Hamiltonian cycle**: A cycle that visits every vertex exactly once and returns to the starting vertex.
