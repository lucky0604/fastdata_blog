# Heap

[TOC]

## Definition

In computer science, a heap is a specialized tree-based data structure which is essentially an almost complete tree that satisfies the heap property: in a max heap, for any given node C, if P is a parent node of C, then the key (the value) of P is greater than or equal to the key of C. In a min heap, the key of P is less than or equal to the key of C. The node at the "top" of the heap (with no parents) is called the root node.

The heap is one maximally efficient implementation of an abstract data type called a priority queue, and in fact, priority queues are often referred to as "heaps", regardless of how they may be implemented. In a heap, the highest (or lowest) priority element is always stored at the root. However, a heap is not a sorted structure; it can be regarded as being partially ordered. A heap is a useful data structure when it is necessary to repeatedly remove the object with the highest (or lowest) priority.

A common implementation of a heap is the binary heap, in which the tree is a binary tree. The heap data structure, specifically the binary heap, was introduced by **J. W. J. Williams** in 1964, as a data structure for the heapsort sorting algorithm. Heaps are also crucial in several efficient graph algorithms such as **Dijkstra's algorithm**. When a heap is a complete binary tree, it has a smallest possible height - a heap with *N* nodes and for each node a branches always has $log_aN$ height.

Note that, as shown in the graphic, there is no implied ordering between siblings or cousins and no implied sequence for an in-order traversal (as there would be in, e.g., a binary search tree). The heap relation mentioned above applies only between nodes and their parents, grandparents, etc. The maximum number of children each node can have depends on the type of heap.

## Implementation

Heaps are usually implemented with an array, as follows:

- Each element in the array represents a node of the heap, and
- The parent / child relationship is defined implicity by the elements' indices in the array

In the array, the first index contains the root element. The next two indices of the array contain the root's children. The next four indices contain the four children of the root's two child nodes, and so on. Therefore, given a node at index i, its children are at indices **2i + 1** and **2i + 2**, and its parent is at index **floor((i - 1) / 2)**. This simple indexing scheme makes it efficient to move "up" or "down" the tree.

Balancing a heap is done by sift-up or sift-down operations (swapping elements which are out of order). As we can build a heap from an array without requiring extra memory (for the nodes, for example), heapsort can be used to sort an array in-place.

After an element is inserted into or deleted from a heap, the heap property may be violated, and the heap must be re-balanced by swapping elements within the array.

Different types of heaps implement the operations in different ways, but notably, insertion is often doen by adding the new element at the end of the heap in the first available free space. This will generally violate the heap property, and so the elements are then shifted up until the heap property has been reestablished. Similarly, deleting the root is done by removing the root and then putting the last element in the root and sifting down to re-balance. Thus replacing is done by deleting the root and putting the new element in the root and sifting down, avoiding a sifting up step compared to pop (sift down of last element) followed by push (sift up of new element).

Construction of a binary (or d-ary) heap out of a given array of elements may be performed in linear time using the classic Floyd algorithm, with the worst-case number of comparisons equal to $2N - 2s_2(N) - e_2(N)$ (for a binary heap), where $s_2(N)$ is the sum of all digits of the binary representation of N and e_2(N) is the exponent of 2 in the prime factorization of N. This is faster than a sequence of consecutive insertions into an originally empty heap, which is log-linear.

### Java

```java
// Max Heap
public class MaxHeap {
  private int[] heap;
  private int size;
  private int maxSize;
  
  // initialization
  public MaxHeap(int maxSize) {
    this.maxSize = maxSize;
    this.size = 0;
    heap = new int[this.maxSize + 1];
    heap[0] = Integer.MAX_VALUE;
  }
  
  // parent position
  private int parent(int position) {
    return position / 2;
  }
  
  private int leftLeaf(int position) {
    return 2 * position;
  }
  
  private int rightLeaf(int position) {
    return 2 * position + 1;
  }
  
  // whether the node is leaf
  private boolean isLeaf(int position) {
    if (position > (size / 2) && position <= size) {
      return true;
    }
    return false;
  }
  
  private void swap(int firstPosition, int secondPosition) {
    int temp;
    temp = heap[firstPosition];
    heap[firstPosition] = heap[secondPosition];
    heap[secondPosition] = temp;
  }
  
  // function to heapify the given subtree.
  // assumes that the left and right subtrees are already heapified, only need to fix the root
  private void maxHeapify(int position) {
    if (isLeaf(position)) return;
    
    if (heap[position] < heap[leftLeaf(position)] || heap[position] < heap[rightLeaf(position)]) {
      if (heap[leftLeaf(position)] > heap[rightLeaf(position)]) {
        swap(position, leftLeaf(position));
        maxHeapify(leftLeaf(position));
      } else {
        swap(position, rightLeaf(position));
        maxHeapify(rightLeaf(position));
      }
    }
  }
  
  public void add(int elem) {
    heap[++size] = elem;
    
    int curr = size;
    while (heap[curr] > heap[parent(curr)]) {
      swap(curr, parent(curr));
      curr = parent(curr);
    }
  }
  
  public int remove() {
    int elem = heap[1];
    heap[1] = heap[size --];
    maxHeapify(1);
    return elem;
  }
}
```

### Python

```python
class MaxHeap:
  def __init__(self):
    self.heap = []
  
  def left_leaf(self, i):
    return 2 * i + 1
  
  def right_leaf(self, i):
    return 2 * i + 2
  
  def Length(self):
    return len(self.heap)
  
  def Parent(self, i):
    return (i - 1) // 2;
  
  def Get_Index(self, i):
    return self.heap[i]
  
  def get_max(self):
    if self.Length() == 0:
      return None
    return self.heap[0]
  
  def remove(self):
    if self.Length() == 0:
      return None
    largest = self.get_max()
    self.heap[0] = self.heap[-1]
    del self.heap[-1]
    self.max_heapify(0)
    return largest
  
  def max_heapify(self, i):
    l = self.left_leaf(i)
    r = self.right_leaf(i)
    if (l <= self.Length() - 1 and self.Get_Index(l) > self.Get_Index(i)):
      largest = l
    else:
    	largest = i
      
    if (r <= self.Length() - 1 and self.Get_Index(r) > self.Get_Index(largest)):
      largest = r
    if (largest != i):
      self.swap(largest, i)
      self.max_heapify(largest)
      
  def swap(self, i, j):
    self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
    
  def add(self, key):
    index = self.Length()
    self.heap.append(key)
    
    while (index != 0):
      p = self.Parent(index)
      if self.Get_Index(p) < self.Get_Index(index):
        self.swap(p, index)
      index = p
```

## Comparison of theoretic bounds for variants

Here are time complexities of various heap data structures. Function names assume a **max-heap**. For the meaning of "*O(f)*"and "$\Theta(f)$" see [Big O notation](https://en.wikipedia.org/wiki/Big_O_notation).

| Operation        | find-max    | delete-max      | insert          | increase-key    | meld            |
| ---------------- | ----------- | --------------- | --------------- | --------------- | --------------- |
| Binary           | $\Theta(1)$ | $\Theta(log n)$ | $O(log n)$      | $O(log n)$      | $\Theta(n)$     |
| Leftist          | $\Theta(1)$ | $\Theta(log n)$ | $\Theta(log n)$ | $O(log n)$      | $\Theta(log n)$ |
| Binomial         | $\Theta(1)$ | $\Theta(log n)$ | $\Theta(1)$     | $\Theta(log n)$ | $O(log n)$      |
| Fibonacci        | $\Theta(1)$ | $O(log n)$      | $\Theta(1)$     | $\Theta(1)$     | $\Theta(1)$     |
| Pairing          | $\Theta(1)$ | $O(log n)$      | $\Theta(1)$     | $o(log n)$      | $\Theta(1)$     |
| Brodal           | $\Theta(1)$ | $O(log n)$      | $\Theta(1)$     | $\Theta(1)$     | $\Theta(1)$     |
| Rank-pairing     | $\Theta(1)$ | $O(log n)$      | $\Theta(1)$     | $\Theta(1)$     | $\Theta(1)$     |
| Strict Fibonacci | $\Theta(1)$ | $O(log n)$      | $\Theta(1)$     | $\Theta(1)$     | $\Theta(1)$     |
| 2-3 heap         | $O(log n)$  | $O(log n)$      | $O(log n)$      | $\Theta(1)$     | ?               |

