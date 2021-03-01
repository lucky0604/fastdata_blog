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

![Hash collision by separate chaining with head records in the bucket array](/Users/lucky/Documents/Code/fastdata/Data_Structure/Hash_table_5_0_1_1_1_1_0_LL.svg.png)

#### Open addressing

In another strategy, called `open addressing`, all entry records are stored in the bucket array itself. When a new entry has to be inserted, the buckets are examined, starting with the hashed-to slot and proceeding in some probe sequence, until an unoccupied slot is found. When searching for an entry, the buckets are scanned in the same sequence, until either the target record is found, or an unused array slot is found, which indicates that there is no such key in the table. The name "open addressing" refers to the fact that the location ("address") of the item is not determined by its hash value. (This method is also called **closed hashing**; it should not be confused with "open hashing" or "closed addressing" that usually mean separate chaining).

Well-known probe sequences include:

- Linear probing, in which the interval between probes is fixed (usually 1). Because of good CPU cache utilization and high performance this algorithm is most widely used on modern computer architectures in hash table implementations.

![Hash collision resolved by open addressing with linear probing](/Users/lucky/Documents/Code/fastdata/Data_Structure/1024px-Hash_table_5_0_1_1_1_1_0_SP.svg.png)

- Quadratic probing, in which the interval between probes is increased by adding the successive outputs of a quadratic polynomial to the starting value given by the original hash computation
- Double hashing, in which the interval between probes is computed by a second hash function

A draw back of all these open addressing schemes is that the number of stored entries cannot exceed the number of slots in the bucket array. In fact, even with good hash functions, their performance dramatically degrades when the load factor grows 0.7 or so. For many applications, these restrictions mandate the use of dynamic resizing, with its attendant costs.

Open addressing schemes also put more stringent requirements on the hash function: besides distributing the keys more uniformly over the buckets, the function must also minimize the clustering of hash values that are consecutive in the probe order. Using separate chaining, the only concern is that too many objects map to the same hash value; whether they are adjacent or nearby completely irrelevant.

Open addressing only saves memory if the entries are small (less than four times the size of a pointer) and the load factor is not too small. If the load factor is close to zero (that is, there are far more buckets than stored entries), open addressing is wasteful even if each entry is just two words.

Open addressing avoids the time overhead of allocating each new entry record, and can be implemented even in the absence of a memory allocator. It also avoids the extra indirection required to access the first entry of each bucket (that is, usualy the only one). It also has better locality of reference, particularly with linear probing. With small record sizes, these factors can yield better performance than chaining, particularly for lookups. Hash tables with open addressing are also easier to serialize, because they do not use pointers.

On the other hand, normal open addressing is a poor choice for large elements, because these elements fill entire CPU cache line (negating the cache advantage), and a large amount of space is wasted on large empty table slots. If the open addressing table only stores references to elements (external storage), it uses space comparable to chaining even for large records but loses its speed advantage.

Generally speaking, open addressing is better used for hash tables with small records that can be stored within the table (internal storage) and fit in a cache line. They are particularlly suitable for elements of one word or less. If the table is expected to have a high load factor, the records are large, or the data is variable-sized, chained hash tables often perform as well or better.

##### Coalesced hashing

A hybrid of chaining and open addressing, coalesced hashing links together chains of nodes withing the table itself. Like open addressing, it achieves space usage and (somewhat diminished) cache advantages over chaining. Like chaining, it does not exhibit clustering effects; in fact, the table can be efficiently filled to a high density. Unlike chaining, it cannot have more elements than table slots.

##### Cuckoo hashing

Another alternative open-addressing solution is cuckoo hashing, which ensures constant lookup and deletion time in the worst case, and constant amortized time for insertions (with low probability that the worst-case will be encountered). It uses two or more hash functions, which means any key/value pair could be in two or more locations. For lookup, the first hash function is used; if the key/value is not found, then the second hash function is used, and so on. If a collision happens during insertion, then the key is re-hashed with the second hash function to map it to another bucket. If all hash functions are used and there is still a collision, then the key it collided with is removed to make space for the new key, and the old key is re-hashed with one of the other hash functions, which maps it to another bucket. If that location also results in a collision, then the process repeats until there is no collision or the process traverses all the buckets, at which point the table is resized. By combining multiple hash functions with multiple cells per bucket, very high space utilization can be achieved.

##### Hopscotch hashing

Another alternative open-addressing solution is hopscotch hashing, which combines the approaches of cuckoo hashing and linear probing, yet seems in general to avoid their limitations. In particular it works well even when the load factor grows beyond 0.9. The algorithm is well suited for implementation a resizable concurrent hash table.

The hoscotch hashing algorithm works by defining a neighborhood of buckets near the original hashed bucket, where a given entry is always found. Thus, search is limited to the number of entries in this neighborhood, which is logarithmic in the worst case, constant on average, and with proper alignment of the neighborhood typically requires one cache miss. When inserting an entry, one first attempts to add it to a bucket in the neighborhood. However, if all buckets in this neighborhood are occupied, the algorithm traverses buckets in sequence until an open slot (an unoccupied bucket) is found (as in linear probing). At that point, since the empty bucket is outside the neighborhood, items are repeatedly displaced in a sequence of hops. (This is similar to cuckoo hashing, but with the differece that in this case the empty slot is being moved into the neighborhood, instead of items being moved out with the hope of eventually finding an empty slot.) Each hop brings the open slot closer to the original neighborhood, without invalidating the neighborhood property of any of the buckets along the way. In the end, the open slot has been moved into the neighborhood, and the entry being inserted can be added to it.

##### Robin Hood hashing

A variation on double-hasing collision resolution is Robin Hood hashing. The idea is that a new key may displace a key already inserted, if its probe count is larger than that of the key at the current position. The net effect of this is that it reduces worst case search times in the table. This is similar to ordered hash tables except that the criterion for bumping a key does not depend on a direct relationship between the keys. Since both the worst case and the variation in the number of probes is reduced dramatically, and interesting variation is to probe the table starting at the expected successful probe value and then expand from that position in both directions. External Robin Hood hashing is an extension of this algorithm where the table is stored in an external file and each table position corresponds to a fixed-sized page or bucket with B records.

##### 2-choice hashing

2-choice hashing employes two different hash functions, $h_1(x)$ and $h_2(x)$, for the hash table. Both hash functions are used to compute two table locations. When an object is inserted in the table, it is placed in the table location that contains fewer objects (with the default being the $h_1(x)$ table location if there is equality in bucket size). 2-choice hashing employs the principle of the power of two choices.

### Dynamic resizing

When an insert is made such that the number of entries in a hash table exceeds the product of the load factor and the current capacity then the hash table will need to be rehashed. Rehashing includes increasing the size of the underlying data structure and mapping existing items to new bucket locations. In some implementations, if the initial capacity is greater than the maximum number of entries divided  by the load factor, no rehash operations will ever occur.

#### Resizing by copying all entries

A common approach is to automatically trigger a complete resizing when the load factor exceeds some threshold $r_{max}$. Then a new larger table is allocated, each entry is removed from the old table, and inserted into the new table. When all entries have been removed from the old table then the old table is returned to the free storage pool. Likewise, when the load factor falls below a second threshold $r_{min}$, all entries are moved to a new smaller table.

For hash tables that shrink and grow frequently, the resizing downward can be skipped entirely. In this case, the table size is proportional to the maximum numebr of entries that ever were in the hash table at one time, rather than the current number. The disadvantage is that memory usage will be higher, and thus cache behavior may be worse. For best control, a "shrink-to-fit" operation can be provided that does this only on request.

If the table size increases or decreases by a fixed percentage at each expansion, the total cost of these resizing, amortized over all insert and delete operations, is still a constant, independent of the number of entries *n* and of the number *m* of operations performed.

For example, consider a table that was created with the minimum possible size and is doubled each time the load ratio exceeds some threshold. If *m* elements are inserted into that table, the total number of extra re-insertions that occur in all dynamic resizings of the table is at most *m - 1*. In other words, dynamic resizing roughly doubles the cost of each insert or delete operation.

#### Alternatives to all-at-once rehashing

Some hash table implementations, notably in real-time systems, cannot pay the price of enlarging the hash table all at once, because it may inerrupt time-critical operations. If one cannot avoid dynamic resizing, a solution is to perform the resizing gradually. Disk-based hash tables almost always use some alternative to all-at-once rehashing, since the cost of rebuilding the entire table on disk would be too high.

##### Incremental resizing

One alternative to enlarging the table all at once is to perform the rehashing gradually:

- During the resize, allocate the new hash table, but keep the old table unchanged.
- In each lookup or delete operation, check both tables.
- Perform insertion operations only in the new table.
- At each insertion also move *r* elements from the old table to the new table.
- When all elements are removed from the old table, deallocate it.

To ensure that the old table is completely copied over before the new table itself needs to be enlarged, it is necessary to increase the size of the table by a factor of at least *(r + 1) / r* during resizing.

##### Monotonic keys

If it is known that keys will be stored in monotonically increasing (or decreasing) order, then a variation of consistent hashing can be achieved.

Given some initial key $k_1$, a subsequent key $k_i$ partitions the key domain $[k_1, \infty)$ into the set {${[k_1, k_i), [k_i, \infty)}$}. In general, repeating this process gives a finer partition{$[k_1, k_{i_0}), [k_{i_0}, k_{i_1}), ..., [k_{i_{n-1}}, k_{i_n}), [k_{i_n}, \infty)$} for some sequence of monotonically increasing keys ($k_{i_0}$, ..., $k_{i_n}$), where *n* is the number of refinements. The same process applies, mutatis mutandis, to monotonically decreasing keys. By assigning to each subinterval of this partition a different hash function or hash table (or both), and by refining the partition whenever the hash table is resized, this approach guarantees that any key's hash, once issued, will never change, even when the hash table is grown.

Since it is common to grow the overall number of entries by doubling, there will only be $O(log(N))$ subintervals to check, and binary search time for the redirection will be $O(log(log(N)))$.

##### Linear hashing

Linear hashing is a hash table algorithm that permits incremental hash table expansion. It is implemented using a single hash table, but with two possible lookup functions.

##### Hashing for distributed hash tables

Another way to decrease the cost of table resizing is to choose a hash function in such a way that the hashes of most values do not change when the table is resized. Such hash functions are prevalent in disk-based and distributed hash tables, where rehashing is prohibitively costly. The problem of designing a hash such that most values do not change when the table is resized is known as the distributed hash table problem. The four most popular approaches are rendezvous hashing, consistent hashing, the content addressable network algorithm and Kademlia distance.

### Performance

#### Speed analysis

In the simplest model, the hash function is completely unspecified and the table does not resize. With an ideal hash function, a table of size **k** with open addressing has no collisions and holds up to **k** elements with a single comparison for successful lookup, while a table of size **k** with chaining and *n* keys has the minimum $max(0, n-k)$ collisions and $\Theta(1 + \frac{n}{k})$ comparisons for lookup. With the worst possible hash function, every insertion causes a collision, and hash tables degenerate to linear search, with $\Theta(n)$ amorized comparisons per insertion and up to *n* comparisons for a successful lookup.

Adding rehashing to this model is straightforward. As in a dynamic array, geometric resizing by a factor of *b* implies that only $\frac{n}{b^i}$ keys are inserted *i* or more times, so that the total number of insertions is bounded above by $\frac{bn}{b - 1}$, which is $\Theta(n)$. By using rehashing to maintain *n < k*, tables using both chaining and open addressing can have unlimited elements and perform successful lookup in a single comparison for the best choice of hash function.

In more realistic models, the hash function is a random variable over a probability distribution of hash functions, and performance is computed on average over the choice of hash function. When this distribution is uniform, the assumption is called "simple uniform hashing" and it can be shown that hashing with chaining requires $\Theta(1 + \frac{n}{k})$ comparisons on average for an unsuccessful lookup, and hashing with open addressing requires $\Theta(\frac{1}{1 - \frac{n}{k}})$. Both these bounds are constant, if we maintain $\frac{n}{k} < c$ using table resizing, where *c* is fixed constant than 1.

Two factors affect significantly the latency of operations on a hash table:

- Cache missing. With the increasing of load factor, the search and insertion performance of hash tables can be degraded a lot due to the rise of  average cache missing.
- Cost of resizing. Resizing becomes an extreme time-consuming task when hash tables grow massive.

In latency-sensitive programs, the time consumption of operations on both the average and the worst cases are required to be small, stable, and even predictable. The **K** hash table is designed for a general scenario of low-latency applications, aiming to achieve cost-stable operations on a growing huge-sized table.

#### Memory utilization

Sometimes the memory requirement for a table needs to be minimized. One way to reduce memory usage in chaining methods is to eliminate some of the chianing pointers or to replace them with some form of abbreviated pointers.

Another technique was introduced by Donald Knuth and is called quotienting.. For this discussion assume that the key, or a reversibly-hashed version of that key, is an integer *m* from {0, 1, 2, ..., M - 1} and the number of buckets is *N*. *m* is divided by *N* to produce a quotient *q* and a remainder *r*. The remainder *r* is used to select the bucket; in the bucket only the quotient *q* need be stored. This saves $log_2(N)$ bits per element, which can be very significant in some applications.

*Quotienting* works readily with chaining hash tables, or with simple cuckoo hash tables. To apply the technique with ordinary open-addressing hash tables, John G.Cleary introduced a method where two bits (a virgin bit and a change bit) are included in each bucket to allow the original bucket index (*r*)to be reconstructed.

In the scheme just described, $log_2(\frac{M}{N}) + 2$ bits are used to store each key. It is interesting to note that the theoretical minimum storage would be $log_2(\frac{M}{N}) + 1.4427$ bits where $1.4427 = log_2(e)$.

### Features

#### Advantages

- The main advantage of hash tables over other table data structures is speed. This advantage is more apparent when the number of entries is large. Hash tables are particularly efficient when the maximum number of entries can be predicted, so that the bucket array can be allocated once with the optimum size and never resized.
- If the set of key-value pairs is fixed and known ahead of time (so insertions and deletions are not allowed), one may reduce the average lookup cost by a careful choice of the hash function, bucket table size, and internal data structures. In particular, one may be able to devise a hash function that is collision-free, or even perfect. In this case the keys need not be stored in the table.

#### Drawbacks

- Although operations on a hash table take constant time on average, the cost of a good hash function can be significantly higher than the inner loop of the lookup algorithm for a sequential list or search tree. Thus hash tables are not effective when the number of entries is very small. (However, in some cases the high cost of computing the hash function can be mitigated by saving the hash value together with the key).
- For certain string processing applications, such as spell-checking, hash tables may be less efficient than tries, finite automata, or Judy arrays. Also if there are not too many possible keys to store-that is, if each key can be represented by a small enough number of bits-then, instead of a hash table, one may use the key directly as the index into an array of values. Note that there are no collisions in this case.
- The entries stored in a hash table can be enumerated efficiently (at constant cost per entry), but only in some pseudo-random order. Therefore, there is no efficient way to locate an entry whose key is *nearest* to a given key. Listing all *n* entries in some specific order generally requires a separate sorting step, whose cost is proportional to *log(n)* per entry. In comparison, ordered search trees have lookup and insertion cost proportional to *log(n)*, but allow finding the nearest key at about the same cost, and ordered enumeration of all entries at constant cost per entry. However, a LinkingHashMap can be made to create a hash table with a non-random sequence.
- If the keys are not stored (because the hash function is collision-free), there may be no easy way to enumerate the keys that are present in the table at any given moment.
- Although the average cost per operation is constant and fairly small, the cost of a single operation may be quite high. In particular, if the hash table uses dynamic resizing, an insertion or deletion operation may occasionally take time proportional to the number of entries. This may be a serious drawback in real-time or interactive applications.
- Hash tables in general exhibit pool locality of reference - that is, the data to be accessed is distributed seemingly at random in memory. Because hash tables cause access patterns that jump around, this can trigger microprocessor cache misses that cause long delays. Compact data structrues such as arrays searched with linear search may be faster, if the table is relatively small and keys are compact. The optimal performance point varies from system to system.
- Hash tables become quite inefficient when there are many collisions. While extremely uneven hash distributions are extremely unlikely to arise by chance, a malicious adversary with knowledge of the hash function may be able to supply informationto a hash that creates worst-case behavior by causing excessive collisions, resulting in very poor performance, e.g., a denial of service attack. In critical applications, a data structure with better worst-case guarantees can be used; however, universal hashing - a randomized algorithm that prevents the attacker from predicting which inputs cause worst-case behavior - may be preferable. The hash function used by the hash table in the Linux routing table cache was changed with Linux version 2.4.2 as a countermeasure against such attacks.

### Uses

- Associative arrays
- Database indexing
- Caches
- Sets
- Object representation
- Unique data representation
- Transposition table

