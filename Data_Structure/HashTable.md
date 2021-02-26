# Hash Table

[TOC]

## Definition

A hash table (hash map) is a data structure that implements an associative array abstract type, a structure that can map keys to values. A hash table uses a hash function to compute an index, also called a hash code, into an array of buckets or slots, from which the desired value can be found. During lookup, the key is hashed and the resulting hash indicates where the corresponding value is stored.

Ideally, the hash function will assign each key to a unique bucket, but most hash table designs employ an imperfect hash function, which might cause hash collisions where the hash function generates the same index for more than one key. Such collisions are typically accommodated in some way.

In a well-dimensioned hash table, the average cost (number of instructions) for each lookup is independent of the number of elements stored in the table. Many hash table designs also allow arbitrary insertions and deletions of key-value parts, at (amortized) constant average cost per operation.

In many situations, hash tables turn out to be on average more efficient than search trees or any other table lookup structure. For this reason, they are widely used in many kinds of computer software, particularly for associative arrays, database indexing, caches, and sets.

### Hashing

The idea of hashing is to distribute the entries (key/value pairs) across an array of buckets. Given a key, the algorithm computes an index that suggests where the entry can be found:

```python
index = f(key, array_size)
```

Often this is done in two steps:

```python
hash = hashfunc(key)
index = hash % array_size
```

In this method, the hash is independent of the array size, and it is then reduced to an index (a number between `0` and `array_size - 1`) using the modulo operator`(%)`.

In the case that the array size is a power of two, the remainder operation is reduced to masking, which improves speed, but can increase problems with a poor hash function.

#### Choosing a hash function

A basic requirement is that the function should provide a `uniform distribution` of hash values. A non-uniform distribution increases the number of collisions and the cost of resolving them. Uniformity is sometimes difficult to ensure by design, but may be evaluated empirically using statistical tests, e.g., a Pearson's chi-squared test for discrete uniform distributions.

The distribution needs to be uniform only for table sizes that occur in the application. In particular, if one uses dynamic resizing with exact doubling and halving of the table size, then the hash function needs to be uniform only when the size is a power of two. Here the index can be computed as some range of bits of the hash function. On the other hand, some hashing algorithms prefer to have the size be a prime number. The modulus operation may provide some additional mixing; this is especially useful with a poor hash function.

For open addressing schemes, the hash function should also avoid clustering, the mapping of two or more keys to consecutive slots. Such clustering may cause the lookup cost to skyrocket, even if the load factor is low and collisions are infrequent. The popular multiplicative hash is claimed to have particularly poor clustering behavior.

Cryptographic hash functions are believed to provide good hash functions for any table size, either by modulo reduction or by bit masking. They may also be appropriate if there is a risk of malicious users trying to sabotage a network service by submitting requests designed to generate a large number of collisions in the server's hash tables. However, the risk of sabotage can also be avoided by cheaper methods (such as applying a secret salt to the data, or using a universal hash function). A drawback of cryptographic hashing functions is that they are often slower to compute, which means that in cases where the uniformity for any size is not necessary, a non-cryptographic hashing function might be preferable.

#### Perfect hash function

If all keys are known ahead of time, a perfect hash function can be used to create a perfect hash table that has no collisions. If minimal perfect hashing is used, every location in the hash table can be used as well.

Perfect hashing allows for constant time lookups in all cases. This is in contrast to most chaining and open addressing methods, where the time for lookup is low on average, but may be very large, $O(n)$, for instance when all the keys hash to a few values.

### Key statistics

A critical statistic for a hash table is the load factor, defined as
$$
load  factor = \frac{n}{k}
$$
where

- *n* is the number of entries occupied in the hash table
- *k* is the number of buckets

As the load factor grows larger, the hash table becomes slower, and it may even fail to work (depending on the method used). The expected constant time property of a hash table assumes that the load factor be kept below some bound. For a *fixed* number of bucket, the time for a lookup grows with the number of entries, and therefore the desired constant time is not achieved. In some implementations, the solution is to automatically grow (usually, double) the size of the table when the load factor bound is reached, thus forcing to re-hash all entries. As a real-word example, the default load factor for a HashMap in Java 10 is 0.75, which offers a good trade-off between time and space costs.

Second to the load factor, one can examine the variance of number of entries per bucket. For example, two tables both have 1,000 entries and 1,000 buckets; one has exactly one entry in each bucket, the other has all entries in the same bucket. Clearly the hashing is not working in the second one.

A low load factor is not especially beneficial. As the load factor approaches 0, the proportion of unused areas in the hash table increases, but there is no necessarily any reduction in search cost. This results in wasted memory.



### Collision resolution

Hash collisions are practically unavoidable when hashing a random subset of a large set of possible keys. For example, if 2,450 keys are hashed into a million buckets, even with a perfectly uniform random distribution, according to the [Birthday problem](https://en.wikipedia.org/wiki/Birthday_problem) there is approximately a 95% chance of at least two of the keys being hashed to the same slot.

Therefore, almost all hash table implementations have some collision resolution strategy to handle such events. Some common strategies are described below. All these methods require that the keys (or pointer to them) be stored in the table, together with the associated values.

#### Separate chaining

In the method known as separate chaining, each bucket is independent, and has some sort of list of entries with the same index. The time for hash table operations is the time to find the bucket (which is constant) plus the time for the list operation.

In most implementations buckets will have few entries, if the hash function is working properly. Therefore, structures that are efficient in time and space for these cases are preferred. Structures taht are efficient for a fairly large number of entries per bucket are not needed or desirable. If these cases happens often, the hashing function needs to be fixed.

There are some implementations which give excellent performance for both time and space, with the average number of elements per bucket ranging between 5 and 100.

![hash collision resolved by separate chaining](/Users/lucky/Documents/Code/fastdata/Data_Structure/1280px-Hash_table_5_0_1_1_1_1_1_LL.svg.png)

##### Separate chaining with linked lists

Chained hash tables with linked lists are popular because they require only basic data structures with simple algorithms, and can use simple hash functions that are unsuitable for other methods.

The cost of a table operation is that of scanning the entries of the selected bucket for the desired key. If the distribution of keys is sufficiently uniform, the average cost of a lookup depends only on the average number of keys per bucket -- that is, it is roughly proportional to the load factor.

For this reason, chained hash tables remain effective even when the number of table entries *n* is much higher than the number of slots. For example, a chained hash table with 1000 slots and 10,000 stored keys (load factor 10) is five to ten times slower than a 10,000-slot table (load factor 1); but still 1000 times faster than a plain sequential list.

For separate-chaining, the worst-case scenario is when all entries are inserted into the same bucket, in which case the hash table if ineffective and the cost is that of searching the bucket data structure. If the latter is a linear list, the lookup procedure may have to scan all its entries, so the worst-case coast is proportional to the number *n* of entries in the table.

The bucket chains are often searched sequentially using the order the entries were added to the bucket. If the load factor is large and some keys are more likely to come up than others, then rearranging the chain with a move-to-front heuristic may be effective. More sophisticated data structures, such as balanced search trees, are worth considering only if the load factor is large (about 10 or more), or if the hash distribution is likely to be very non-uniform, or if one must guarantee good performance even in a worst-case scenario. However, using a large table and/or a better hash function may be even more effective in those cases.

Chained hash tables also inherit the disadvantages of linked lists. When storing small keys and values, the space overhead of the `next` pointer in each entry record can be significant. An additional disadvantage is that traversing a linked list has poor cache performance, making the processor cache ineffective.

##### Separate chaining with list head cells

Some chianing implementations store the first record of each chain in the slot array itself. The number of pointer traversals is decreased by one for most cases. The purpose is to increase cache efficiency of hash table access.

The disadvantage is that an empty bucket takes the same space as a bucket with one entry. To save space, such hash tables often have about as many slots as stored entries, meaning that many slots have two or more entries.

##### Separate chaining with other structures

Instead of a list, one can use any other data structure that supports the required operations. For example, by using a self-balancing binary search tree, the theoretical worst-case time of common hash table operations (insertion, deletion, lookup) can be brought down to $O(log n)$ rather than $O(n)$. However, this introduces extra complexity into the implementation, and may cause even worse performance for smaller hash tables, where the time spent inserting into and balancing the tree is greater than the time needed to perform a linear search on all of the elements of a list. A real world example of a hash table that uses a self-balancing binary search tree for buckets is the `HashMap` class in `Java version 8`.

The variant called array hash table uses a dynamic array to store all the entries that hash to the same slot. Each newly inserted entry gets appended to the end of the dynamic array that is assigned to the slot. The dynamic array is resized in an *exact-fit* manner, meaning it is grown only by as many bytes as needed. Alternative techniques such as growing the array by block sizes or pages were found to improve insertion performance, but at a cost in space. The variation makes more efficient use of CPU caching and the translation lookaside buffer (TLB), because slot entries are stored in sequential memory positions. It also dispenses with the `next` pointers that are required by linked lists, which saves space. Despite frequent array resizing, space overheads incurred by the operating system such as memory fragmentation were found to be small.

An elaboration on this approach is so-called `dynamic perfect hashing`, where a bucket that contains k entries is organized as a perfect hash table with $k^2$ slots. While it uses more memory ($n^2$ slots for n entires, in the worst case and *n x k* slots in the average case), this variant has guaranteed constant worst-case lookup time, and low amorized time for insertion. It is also possible to use a fusion tree for each bucket, achieving constant time for all operations with high probability.

