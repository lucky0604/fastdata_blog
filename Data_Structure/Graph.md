# Graph

[TOC]

## Definition

A graph is an abstract data type that is meant to implement the undirected graph and directed graph concepts from the field of graph theory within mathematics.

A graph data structure consists of a finite (and possibly mutable) set of vertices (also called nodes or points), together with a set of unordered pairs of these vertices for an undirected graph of a set of ordered pairs for a directed graph. These pairs are known as edges (also called links or lines), and for a directed graph are also known as arrows. The vertices may be part of the graph structure, or may be external entities represented by integer indices or references.

A graph data structure may also associate to each edge some edge value, such as a symbolic label or a numeric attribute (cost, capacity, length, etc.)

## Operations

The basic operations provided by a graph data structure *G* usually include:

- `adjacent`(G, x, y): tests whether there is an edge from the vertex x to the vertex y;
- `neighbors`(G, x): lists all vertices y such that there is an edge from the vertex x to the vertex y;
- `add_vertext`(G, x): adds the vertex x, if it is not there;
- `remove_vertex`(G, x): removes the vertex x, if it is there;
- `add_edge`(G, x, y): adds the edge from the vertex x to the vertex y, if it is not there;
- `remove_edge`(G, x, y): removes the edge from the vertex x to the vertex y, if it is there;
- `get_vertex_value`(G, x): returns the value associated with the vertex x;
- `set_vertex_value`(G, x, v): sets the value associated with the vertex x to v;

Structures that associate values to the edges usually also provide:

- `get_edge_value`(G, x, y): returns the value associated with the edge (x, y);
- `set_edge_value`(G, x, y): sets the value associated with the edge (x, y) to v

## Representations

Different data structures for the representation of graphs are used in practice:

### Adjacency list

Vertices are stored as records or objects, and every vertex stores a list of adjacent vertices. This data structure allows the storage of additional data on the vertices. Additional data can be stored if edges are also stored as objects, in which case each vertex stores its incident edges and each edge stores its incident vertices.

### Adjacency matrix

A two-dimensional matrix, in which the rows represent source vertices and columns represent destination vertices. Data on edges and vertices must be stored externally. Only the cost for one edge can be stored between each pair of vertices.

### Incidence matrix

A two-dimensional Boolean matrix, in which the rows represent the vertices and columns represent the edges. The entries indicate whether the vertex at  a row is incident to the edge at a column.

---

The following table gives the time complexity cost of performing various operations on graphs, for each of these representations, with |V| the number of vertices and |E| the number of edges. In the matrix representations, the entries encode the cost of following an edge. The cost of edges that are not present are assumed to be $\infty$

|                                                              | Adjacency list                                               | Adjacency matrix                                             | Incidence matrix                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Store graph                                                  | $O(|V| + |E|)$                                               | $O(|V|^2)$                                                   | $O(|V| * |E|)$                                               |
| Add vertex                                                   | $O(1)$                                                       | $O(|V|^2)$                                                   | $O(|V| * |E|)$                                               |
| Add edge                                                     | $O(1)$                                                       | $O(1)$                                                       | $O(|V| * |E|)$                                               |
| Remove vertex                                                | $O(|E|)$                                                     | $O(|V|^2)$                                                   | $O(|V| * |E|)$                                               |
| Remove edge                                                  | $O(|V|)$                                                     | $O(1)$                                                       | $O(|V| * |E|)$                                               |
| Are vertices x and y adjacent (assumingthat their storage positions are known)? | $O(|V|)$                                                     | $O(1)$                                                       | $O(|E|)$                                                     |
| Remarks                                                      | Slow to remove vertices and edges, because it needs to find all vertices or edges | Slow to add or remove vertices,  because matrix must be resized/copied | Slow to add or remove vertices and edges, because matrix must be resized/copied |

Adjacency lists are generally preferred because they efficiently represent sparse graphs. An adjacency matrix is preferred if the graph is dense, that is the number of edges |E| is close to the number of vertices squared, $|V|^2$, or if one must be able to quickly look up if there is an edge connecting two vertices.

## Parallel Graph Representations

The parallelization of graph problems faces significant challenges: Data-driven computations, unstructured problems, pool locality and high data access to computation ratio. The graph representation used for parallel architectures plays a significant role in facing those challenges. Poorly chosen representations may unnecessarily drive up the communication cost of the algorithm, which will decrease its scalability. In the following, shared and distributed memory architectures are considered.

### Shared memory

In the case of a shared memory model, the graph representations used for parallel processing are the same as in the sequential case, since parallel read-only access to the graph representation (e.g. an adjacency list) is efficient in shared memory.

### Distributed memory

In the distributed memory model, the usual approach is to partition the vertex set V of the graph into *p* sets $V_0, ..., V_{p-1}$. Here, *p* is the amount of available processing elements (PE). The vertex set partitions are then distributed to the PEs with matching index, additionally to the corresponding edges. Every PE has its own subgraph representation, where edges with an endpoint in another partition require special attention. For standard communication interfaces like MPI, the ID of the PE owning the other endpoint has to be identifiable. During computation in a distributed graph algorithms, passing information along these edges implies communication.

Partitioning the graph needs to be done carefully - there is a trade-off between low communication and even size partitioning. But partitioning a graph is a NP-hard problem, so it is not feasible to calculate them. Instead, the following heuristics are used.

1D partitioning: Every processor gets *n/p* vertices and the corresponding outgoing edges. This can be understood as a row-wise or column-wise decomposition of the adjacency matrix. For algorithms operating on this representation, this requires an AII-to AII communication step as well as *O(m)* message buffer sizes, as each PE potentially has outgoing edges to every other PE.

2D partitioning: Every processor gets a submatrix of the adjacency matrix. Assume the processors are aligned in a rectangle $p = p_r * p_c$, where $p_r$ and $p_c$ are the amount of processing elements in each row and column, respectively. Then each processor gets a submatrix of the adjacency matrix of dimension $(n/p_r) * (n/p_c)$. This can be visualized as a checkerboard pattern in a matrix. Therefore, each processing unit can only have outgoing edges to PEs in the same row and column. This bounds the amount of communication partners for each PE to $p_r + p_c - 1$ out of $p = p_r * p_c$ possible ones.