[[00_DSA]]
#weighted, #directed, #undirected, #degree, #path, #cycle, #isolated, #DAG, #connected, #edges, #simple_graph, #sub_graph, #bipartite , #complete_graph, #adjcency 

### 1. **Graph**:

A collection of nodes (or vertices) and edges connecting pairs of nodes.

### 2. **Vertices (Nodes)**:

The fundamental units of a graph, representing points or objects.

### 3. **Edges (Links)**:

The connections between vertices in a graph. An edge may or may not have a direction and weight.

---

### **Types of Graphs:**

### 4. **Directed Graph (Digraph)**:

A graph in which edges have a direction. That is, edges go from one vertex to another, indicating a one-way relationship (e.g., links on a website).

### 5. **Undirected Graph**:

A graph in which edges do not have a direction. This means the relationship between vertices is two-way (e.g., a friendship in a social network).

### 6. **Weighted Graph**:

A graph where edges have weights associated with them. The weights can represent distances, costs, capacities, etc.

---

### **Graph Properties:**

### 7. **Degree**:

The number of edges connected to a vertex.

- **In-degree** (for directed graphs): The number of edges directed _toward_ the vertex.
- **Out-degree** (for directed graphs): The number of edges directed _out of_ the vertex.
- **Degree** (for undirected graphs): The total number of edges connected to the vertex.

### 8. **Path**:

A sequence of edges connecting a sequence of vertices. In a path, a vertex can appear at most once.

### 9. **Cycle**:

A path that starts and ends at the same vertex. In a directed graph, the direction of edges must be respected.

### 10. **Isolated Vertex**:

A vertex with no edges connected to it, meaning its degree is zero.

---

### **Special Types of Graphs:**

### 11. **DAG (Directed Acyclic Graph)**:

A directed graph that contains no cycles. DAGs are often used in scheduling problems, dependency resolution, and more.

### 12. **Connected Graph**:

An undirected graph in which there is a path between every pair of vertices. In a directed graph, this property is referred to as "strongly connected" if all vertices are reachable from one another.

### 13. **Simple Graph**:

A graph with no loops (edges connecting a vertex to itself) and no multiple edges between the same pair of vertices.

### 14. **Subgraph**:

A graph formed from a subset of the vertices and edges of another graph. The subgraph must inherit the connectivity of the original graph.

### 15. **Bipartite Graph**:

A graph whose vertices can be divided into two disjoint sets such that every edge connects a vertex in one set to a vertex in the other set. There are no edges within a set.

### 16. **Complete Graph**:

A graph where every pair of distinct vertices is connected by a unique edge. In an undirected graph, this means every node is connected to every other node.

---

### **Graph Representations:**

### 17. **Adjacency Matrix**:

A 2D matrix representation of a graph where the cell `(i, j)` represents the presence (and possibly weight) of an edge between vertex `i` and vertex `j`. In an unweighted graph, the cell can be 1 (for an edge) or 0 (no edge).

### 18. **Adjacency List**:

A list where each vertex has a list of adjacent vertices (neighbors). This is often a more efficient representation for sparse graphs (graphs with fewer edges).

### 19. **Edge List**:

A list of edges, where each edge is represented as a pair (or tuple) of vertices, often with an associated weight. It's a compact representation for small graphs.

---

### 20. **Tree**:

A special type of graph that is connected and acyclic (i.e., no cycles). A tree with `n` nodes has `n-1` edges.

### 21. **Multigraph**:

A graph where multiple edges can exist between the same set of vertices.

### 22. **Loop**:

An edge that connects a vertex to itself.

---

### Summary of Key Differences:

- **Directed vs. Undirected**: Directed graphs have edges with direction, while undirected graphs do not.
- **Weighted vs. Unweighted**: Weighted graphs have edges with associated weights, while unweighted graphs have equal weight or no weight for edges.
- **Bipartite vs. Complete**: Bipartite graphs divide vertices into two sets, while complete graphs connect every pair of vertices.
- **Adjacency Matrix vs. Adjacency List**: An adjacency matrix uses a 2D array for edge representation, while an adjacency list uses a list of neighbors for each vertex.