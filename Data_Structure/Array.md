# Array[^1]

[TOC]

## Definition

An array data structure, or simply an array, is a data structure consisting of a collection of elements (values or variables), each identified by at least one array index or key.

An array is stored such that the position of each element can be computed from its index tuple by a mathematical fomula.

The simplest type of data structure is a linear array, also called one-dimensional array.

> The memory address of the first element of an array is called first address, foundation address, or base address

## Applications

- Arrays are used to implement mathematical `vectors` and `matrices`, as well as other kinds of rectangular tables. Many databases, small and large, consist of (or include) one-dimensional arrays whose elements are records.
- Arrays are used to implement other data structures, such as `lists, heaps, hash tables, deques, queues, stacks, strings and VLists.` Array-based implementations of other data structures are frequently simple and space-efficient (`implicit data structures`), requiring little space overhead, but may have pool space complexity, particularly when modified, compared to tree-based data structures (compare a sorted array to a search tree).
- One or more large arrays are sometimes used to emulate in-program dynamic memory allocation, particularly memory pool allocation. Historically, this has sometimes been the only way to allocate "dynamic memory" protably.
- Arrays can be used to determine partial or complete control flow in programs, as a compact alternative to (otherwise repetitive) multiple `IF` statements. They are known in this context as control tables and are used in conjunction with a purpose built interpreter whose control flow is altered according to values contained in the array. The array may contain subroutine pointers (or relative subroutine numbers that can be acted upon by `SWITCH` statements) that direct the path of the execution.

## Element identifier and addressing formulas

- 0 (zero-based indexing)

  The first element of the array is indexed by subscript of 0

- 1 (one-based indexing)

  The first element of the array is indexed by subscript of 1

- n (n-based indexing)

  The base index of an array can be freely chosen. Usually programming languages allowing n-based indexing also allow negative index values and other scalar data types like enumerations, or characters may be used as an array index.

## Resizing

> Main article: Dynamic array[^2]

Static arrays have a size that is fixed when they are created and consequently do not allow elements to be inserted or removed.

By allocating a new array and copying the contents of the old array to it, it is possible to effectively implement a dynamic version of an array. If this operation is done infrequently, insertions at the end of the array require only amortized constant time.

## Efficiency

Both store and select take (deterministic worst case) constant time. Arrays take linear ($O(n)$) space in  the number of elements n that they hold.

In an array with element size k and on a machine with a cache line size of B bytes, iterating through an array of n elements requires the minimum of ceiling (nk/B) cache misses, because its elements occupy contiguous memory locations. This is roughly a factor of B/k better than the number of cache misses needed to access n elements at random memory locations. As a consequence, sequential iteration over an array is noticeably faster in practice than iteration over many other data structures, a property called locality of reference (this does not mean however, that using a perfect hash or trivial hash within the same (local) array, will not be even faster - and achievable in constant time). Libraries provide low-level optimized facilities for copying ranges of memory (such as memcpy) which can be used to move contiguous blocks of array elements significantly faster than can be achieved through individual element access. The speedup of such optimized routines varies by array element size, architecture, and implementation.

Memory-wise, arrays are compact data structures with no per-element overhead. There may be a per-array overhead (e.g., to store index bounds) but this is language-dependent. It can also happen that elements stored in an array require less memory than the same elements stored in individual variables, because several array elements can be stored in a single word; such arrays are often called `packed arrays`. An extreme (but commonly used) case is the bit array, where every bit represents a single element. A single octet can thus hold up to 256 different combinations of up to 8 different conditions, in the most compact form.

Array accesses with statically predictable access patterns are a major source of data parallelism.

## Comparison with other data structures

|                            | Linked List | Array       | Dynamic array | Balanced tree  | Random access list | Hashed array tree |
| -------------------------- | ----------- | ----------- | ------------- | -------------- | ------------------ | ------------------ |
| indexing                   | $\theta(n)$ | $\theta(1)$ | $\theta(1)$   | $\theta(logn)$ | $\theta(logn)$     |$\theta(1)$|
| insert/delete at beginning | $\theta(1)$ | N/A         | $\theta(n)$   | $\theta(logn)$ | $\theta(1)$ |$\theta(n)$|
| Insert/delete at end | $\theta(1)$ when last element is known;  $\theta(n)$ when last element is unknown; | N/A | $\theta(1)$ amortized | $\theta(logn)$ | N/A |$\theta(1)$ amortized|
| insert/delete in middle | search time + $\theta(1)$ | N/A | $\theta(n)$ | $\theta(logn)$ | N/A |$\theta(n)$|
| Wasted space (average) | $\theta(n)$ | 0 | $\theta(n)$ | $\theta(n)$ | $\theta(n)$ |$\theta(\sqrt{n})$|

## Dimension

The dimension of an array is the number of indices needed to select an element. Thus, if the array is seen as a function on a set of possible index combinations, it is the dimension of the space of which its domain is a discrete subset. Thus a one-dimensional array is a list of data, a two-dimensional array is a rectangle of data, a three-dimensional array a block of data.

This should not be confused with the dimension of the set of all matrices with a given domain, that is, the number of elements in the array. For example, an array with 5rows and 4 columns is two-dimensional, but such matrices form a 20-dimensional space. Similarly, a three-dimensional vector can be represented by a one-dimensional array of size three.