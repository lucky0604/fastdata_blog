# Insertion Sort[^1]

[TOC]

## Definition

A simple sorting algorithm that builds the final sorted array(or list) one item at a time. **It is much less efficient on large lists than more advanced algorithms such as `quick sort, heap sort, or merge sort`**

## Advantages

- Simple impletation
- Efficient for `(quite)` small data sets, much like other quadratic sorting algorithms
- More efficient in practice than most other simple quadratic (i.e., $O(n^2)$) algorithms such as `selection sort or bubble sort`
- Adaptive: i.e., efficient for data sets that are already substantially sorted: the time complexity is $O(kn)$ when each element in the input is no more than k places away from its sorted position
- Stable: i.e., does not change the relative order of elements when equal keys
- In-place: i.e., only requires a constant amount $O(1)$ of additional memory space
- Online: i.e., can sort a list as it receives it

## Implementation:

### Java

```java
public void insertionSort(int[] nums) {
  // corner case
  if (nums.length == 0) return;
  // insertion number
  int number;
  // initial the insertion number from the second one 
  for (int i = 1; i < nums.length; i ++) {
    number = nums[i];
    int j = i - 1;		// already sorted nums
    while (j >= 0 && nums[j] > number) {
      // swap index
      nums[j + 1] = nums[j];
      j --;
    }
    nums[j + 1] = number;
  }
}
```

### Python

```python
def insertion_sort(arr: list[int]):
  length = len(arr)
  # the number to be insert
  for i in range(1, length):
    number = arr[i]
    j = i - 1
    while j >= 0 and arr[j] > number:
      arr[j + 1] = arr[j]
      j --
    arr[j + 1] = number
    
```

## Algorithm Principle

`Insertion Algorithm` is a `in-place algorithm`[^2]. By iterating up the array, growing the sorted list behind it. At each array-position, it checks the value there against the largest value in the sorted list(which happens to be next to it, in the previous array-position checked). If larger, it leaves the element in place and moves to the next.

Consuming one input element each repetition, and make the output list sorted. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.

It repeats until no input element remain.

The most common variant of insertion sort, which operates on arrays, can be described as follows:

- suppose there exists a function called `insert` designed to insert a value into a sorted sequence at the beginning of an array. It operates by beginning at the end of the sequence and shifting each element one place to the right until a suitable position is found for the new element. The function has the side effect of overwriting the value stored immediately after the sorted sequence in the array.
- to perform an insertion sort, begin at the left-most element of the array and invoke `Insert` to insert each element encountered into its correct position. The ordered sequence into which the element is inserted is stored at the beginning of the arary in the set of indices already examined. Each insertion overwrites a single value: the value being inserted.

## Complexity

### Time Complexity

#### Best case

The best case input is an array that is already sorted. In this case insertion sort has a linear running time, only loop outside for `n-1` times. (During each iteration, the first remaining element of the input is only compared with the right-most element of the sorted subsection of the array)

Comparition Times: $ C_{min} = n - 1$

Move Times: $M_{min} = 0$

Complexity: $O(n)$

#### Worst case

The simplest worst case input is an array sorted in reverse order. The set of all worst case inputs consists of all arrays where each element is the smallest or second-smallest of the elements before it. In these cases every iteration of the inner loop will scan and shift the entire sorted subsection of the array before inserting the next element. (If the input array sorted in reverse order, it needs to sort $n-1$ times). This gives insertion sort a quadratic running time.

#### Average case

The average case is also quadratic, which makes insertion sort impractical for sorting large arrays. However, insertion sort is one of the fastest algorithms for sorting very small arrays, even faster than `quicksort`; indeed good `quicksort` implementations use insertion sort for arrays smaller than a certain threshold, also when arising as subproblems; the exact threshold must be determined experimentally and depends on the machine, but is commonly around ten.

Comparition Time: $C_{min} = 1 + 2 + 3 + ··· + n - 1 = \frac{n(n - 1)}2 = O(n^2)$

Move Times: $M_{max} = 2 + 3 + 4 + ··· + n = \frac{(n - 1)(n + 2)} 2 = O(n^2)$

Complexity: $O(n^2)$

### Spatial Complexity

Spatial complexity is the memory of the temporary variable when exchanging elements, so it should be $O(1)$

### Stability

Duration the sorting, the same elements' relative position keep the same, so that insertion sort is a stable sort.

| Time complexity (Average) | Time complexity (Best case) | Time complexity (Worst case) | Spatial complexity | Sort order | Stability |
| ------------------------- | --------------------------- | ---------------------------- | ------------------ | ---------- | --------- |
| $O(n^2)$                  | $O(n)$                      | $O(n^2)$                     | $O(1)$             | in-place   | Stable    |



## Read More:

- [Insertion sort is $O(nlogn)$](https://www3.cs.stonybrook.edu/~bender/newpub/BenderFaMo06-librarysort.pdf)

## References

[^1]:[Insertion sort - Wikipedia](https://en.wikipedia.org/wiki/Insertion_sort)
[^2]: [In-place algorithm - Wikipedia](https://en.wikipedia.org/wiki/In-place_algorithm)