# Tree

[TOC]

## Definition

In computer science, a tree is a widely used **abstract data type** that simulates a hierarchical tree structure, with a root value and substrees of children with a parent node, represented as a set of linked nodes.

A tree data structure can be defined recursively as a collection of nodes (starting at a root node), where each node is a data structure consisting of a value, together with a list of references to nodes (the "children"), with the constraints that no reference is duplicated, and none points to the root.

Alternatively, a tree can be defined abstractly as a whole (globally) as an ordered tree, with a value assigned to each node. Both these perspectives are useful: while a tree can be analyzed mathematically as a whole, when actually represented as a data structure it is usually represented and worked with separately by node (rather than as a set of nodes and an adjacency list of edges between nodes, as one may represent a digraph, for instance). For example, looking at a tree  as a whole, one can talk about "the parent node" of a given node, but in general, as a data structure, a given node only contains the list of its children but does not contain a reference to its parent (if any).

## Common uses

- Representing hierarchical data such as:
  - Abstract syntax trees for computer languages
  - Parse trees for human languages
  - Document Object Models of XML and HTML documents
  - JSON and YAML documents being processed
- Search trees store data in a way that makes an efficient search algorithm possible via tree traversal
  - A binary search tree is a type of binary tree
- Representing sorted lists of data
- As a workflow for compositing digital images for visual effects
- Storing Barnes-Hut trees used to simulate galaxies

## Terminology

A node is a struecture which may contain a value or condition, or represent a separate data structure (which could be a tree of its own). Each node in a tree has zero or more child nodes, which are below it in the tree (by convention, trees are drawn growing downwards). A node that has a child is called the child's **parent node** (or superior). A node has at most one parent, but possibly many ancestor nodes, such as the parent's parent. Child nodes with the same parent are sibling nodes.

An internal node (also known as an inner node, inode for short, or branch node) is any node of a tree that has child nodes. Similarly, an external node (also known as an outer node, leaf node, or terminal node) is any node that does not have child nodes.

The topmost node in a tree is called the root node. Depending on the definition, a tree may be required to have a root node (in which case all trees are non-empty), or may be allowed to be empty, in which case it does not necessarily have a root node. Being the topmost node, the root node will not have a parent. It is the node at which algorithms on the tree begin, since as a data structure, one can only pass from parents to children. Note that some algorithms (such as post-order depth-first search) begin at the root, but first visit leaf nodes (access the value of leaf nodes), only visit the root last (i.e., they first access the children of the root, but only access the value of the root last). All other nodes can be reached from it by following edges or links. (In the formal definition, each such path is also unique.) In diagrams, the root node is conventionally drawn at the top. In some trees, such as heaps, the root node has special properties. Every node in a tree can be seen as the root node of the subtree rooted at that node.

The **height** of a node is the length of the longest downward path to a leaf from that node. The height of the root is the height of the tree. The **depth** of a node is the length of the path to its root (i.e., its root path). This is commonly needed in the manipulation of the various self-balancing trees, AVL Trees in particular. The root node has depth zero, leaf nodes have height zero, and a tree with only a single node (hence both a root and leaf) has depth and height zero. Conventionally, and empty tree (tree with no nodes, if such are allowed) has height -1.

A **subtree** of a tree *T* is a tree consisting of a node in *T* and all of its descendants in *T*. Nodes thus correspond to subtrees (each node corresponds to the subtree of itself and all its descendants) - the subtree corresponding to the root node is the entire tree, and each node is the root node of the subtree it determines; the subtree corresponding to any other node is called a proper subtree (by analogy to a proper subset).

Other terms used with trees:

- **Neighbor**

  Parent or child

- **Ancestor**

  A node reachable by repeated proceeding from child to parent

- **Descendant**

  A node reachable by repeated proceeding from parent to child. Also known as *subchild*

- **Branchnode**

- **Internal node**

  A node with at least one child

- **Degree**

  For a given node, its number of children. A leaf has necessarily degree zero.

- **Degree of tree**

  The degree of a tree is maximum degree of a node in the tree

- **Distance**

  The number of edges along the shortest path between two nodes

- **Level**

  The level of a node is the number of edges along the unique path between it and the root node

- **Width**

  The number of nodes in a level

- **Breadth**

  The number of leaves

- **Forest**

  A set of n >= 0 disjoint trees.

- **Ordered tree**

  A rooted tree in which an ordering is specified for the children of each vertex

- **Size of a tree**

  Number of nodes in the tree

## Preliminary definition

A tree is a nonlinear data structure, compared to arrays, linked lists, linked lists, stacks and queues which are linear data structures. A tree can be empty with no nodes or a tree is a structure consisting of one node called the root and zero or one or more subtrees.

![](/Users/lucky/Documents/Code/fastdata/Data_Structure/130px-Directed_graph,_disjoint.svg.png)

Not a tree: Two non-connected parts, A->B and C->D->E. There is more than one root.

![](/Users/lucky/Documents/Code/fastdata/Data_Structure/101px-Directed_graph_with_branching_SVG.svg.png)

Not a tree: undirected cycle 1-2-4-3. 4 has more than one parent (inbound edge).

![](/Users/lucky/Documents/Code/fastdata/Data_Structure/211px-Directed_graph,_cyclic.svg.png)

Not a tree: cycle B->C->E->D->B. B has more than one parent (inbound edge).

![](/Users/lucky/Documents/Code/fastdata/Data_Structure/92px-Graph_single_node.svg.png)

Not a tree: cycle A->A. A is the root but it also has a parent.

![](/Users/lucky/Documents/Code/fastdata/Data_Structure/173px-Directed_Graph_Edge.svg.png)

A tree: each linear list is trivially a tree

## Drawing trees

Trees are often drawn in the plane. Ordered trees can be represented essentially uniquely in the plane, and are hence called plane trees, as follows: if one fixes a conventional order (say, counterclockwise), and arranges the child nodes in that order (first incoming parent edge, then first child edge, etc.), this yields an embedding of the tree in the plane, unique up to ambient isotopy. Conversely, such an embedding determines an ordering of the child nodes.

If one places the root at the top (parents above children, as in a family tree) and places all nodes that are a given distance from the root (in terms of number of edges: the "level" of a tree) on a given horizontal line, one obtains a standard drawing of the tree. Given a binary tree, the first child is on the left (the "left node"), and the second child is on the right (the "right node").

## Common operations

- Enumerating all the iterms
- Enumerating a section of a tree
- Searching for an item
- Adding a new item at a certain position on the tree
- Deleting an item
- Pruning: Removing a whole section of a tree
- Grafting: Adding a whole section to a tree
- Finding the root for any node
- Finding the lowest common anchestor of two nodes

### Traversal and search methods

Stepping through the items of a tree, by means of the connections between parents and children, is called walking the tree, and the action is a walk of the tree. Often, an operation might be performed when a pointer arrives at a particular node. A walk in which each parent node is traversed before its children is called a **pre-order** walk; a walk in which the children are traversed before their respective parents are traversed is called a post-order walk; a walk in which a node's left subtree, then the node itself, and finally its right subtree are traversed is called an **in-order** traversal. (This last scenario, referring to exactly two subtrees, a left subtree and a right subtree, assumes specifically a binary tree). A **level-order** walk effectively performs a breadth-first search over the entirely of a tree; nodes are traversed level by level, where the root node is visited first, followed by its direct child nodes and their siblings, followed by its grandchild nodes and their siblings, etc., until all nodes in the tree have been traversed.

## Representations

There are many different ways to represent trees; common representations represent the nodes as dynamically allocated records with pointers to their children, their parents, or both, or as items in an array, with relationships between them determined by their positions in the array (e.g., binary heap)

Indeed, a binary tree can be implemented as a list of lists (a list where the values are lists): the head of a list (the value of the first term) is the left child (subtree), while the tail (the list of second and subsequent terms) is the right child (subtree). This can be modified to allow values as well, as in List S-expressions, where the head (value of first term) is the value of the node, the head of the tail (value of second term) is the left child, and the tail of the tail (list of third and subsequent terms) is the right child.

In general a node in a tree will not have pointers to its parents, but this information can be included (expanding the data structure to also include a pointer to the parent) or stored separately. Alternatively, upward links can be inlcuded in the child node data, as in a threaded binary tree.

## Generalizations

### Digraphs

If edges (to child nodes) are thought of as references, then a tree is a special case of a digraph, and the tree data structure can be generalized to represent directed graphs by removing the constraints that a node may have at most one parent, and that nocycles are allowed. Edges are still abstractly considered as pairs of nodes, however, the terms parent and child are usually replaced by different terminology (for example, source and target). Different implementation strategies exist: a digraph can be represented by the same local data structure as a tree (node with value and list of children), assuming that "list of children" is a list of references, or globally by such structures as adjacency lists.