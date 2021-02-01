# Stack

[TOC]

## Definition

A stack is an abstract data type that serves as a collection of elements, with two main principal operations:

- **Push** , which adds an element to the collection
- **Pop** , which removes the most recently added element that was not yet removed

The order in which elements come off a stack gives rise to its alternative name: **LIFO (Last In, First Out)**

Additionally, a peek operation may give access to the top without modifying the stack.

Considered as a linear data structure, or more abstractly a sequential collection, the push and pop operations occur only at one end of the structure, referred to  as the top of the stack. This data structure makes it possible to implement a stack as a singly linked list and a pointer to the top element.

A stack may be implemented to have a bounded capacity. If the stack is full and does not contain enough space to accept an entity to be pushed, the stack is then considered to be in an overflow state. The pop operation removes an item from the top of the stack.

> A stack is needed to implement `depth-first search`

## Implementation

```java
// implement a stack by using Array
public class ArrayStack {
  private int arr[];
  private int size;
  private int index = 0;
  
  public ArrayStack(int size) {
    this.size = size;
    arr = new int[size];
  }
  
  public void add(int element) {
    arr[index] = element;
    index ++;
  }
  
  public int pop() {
    return arr[--index];
  }
  
  public boolean isEmpty() {
    if (index == 0) {
      return true;
    }
    return false;
  }
  
  public int size() {
    return index;
  }
}
```

## Applications

### Expression evaluation and syntax parsing

Calculators employing reverse Polish notation use a stack structure to hold values. Expressions can be represented in prefix, postfix or infix notations and conversion from one from to another may be accomplished using a stack. Many compilers use a stack for parsing the syntax of expressions, program blocks etc. before translating into low level code. Most programming languages are context-free languages, allowing them to be parsed with stack based machines.

### Backtracking

Another important application of stacks is backtracking. Consider a simple example of finding the correct path in a maze. There are a series of points, from the starting point to the destination. We start from one point. To reach the final destination, there are several paths. Suppose we choose a random path. After following a certain path, we realise that the path we have chosen is wrong. So we need to find a way by which we can return to the beginning of that path. This can be done with the use of stacks. With the help of stacks, we remember the point where we have reached. This is done by pushing that point into the stack. In case we end up on the wrong path, we can pop the last point from the stack and thus return to the last point and continue our quest to find the right path. This is called backtracking.

The prototypical example of a backtracking algorithm is `depth-first search`, which finds all vertices of a graph that can be reached from a specified starting vertex. Other applications of backtracking involve searching through spaces that represent potential solutions to an optimization problem. `Branch and bound` is a technique for performing such backtracking searches without exhaustively searching all of the potential solutions in such a space.

### Compile time memory management

> Main articles: Stack-based `memory allocation`[^2] and `Stack machine`[^3]

A number of programming languages are `stack-oriented`, meaning they define most basic operations (adding two numbers, printing a character) as taking their arguments from the stack, and placing any return values back on the stack.

## Security

Some computing environments use stacks in ways that may make them vulnerable to security breaches and attacks. Programmers working in such environments must take special care to avoid the pitfalls of these implementations.

For example, some programming languages use a common stack to store both data local to a called procedure and the linking information that allows the procedure to return to its caller. This means that the program moves data into and out of the same stack that contains critical return addresses for the procedure calls. If data is moved to the wrong location on the stack, or an oversized data item is moved to a stack location that is not large enough to contain it, return information for procedure calls may be corrupted, causing the program to fail.

Malicious parties may attempt a stack smashing attack that takes advantage of this type of implementation by providing oversized data input to a program that does not check the length of input. Such a program may copy the data in its entirety to a location on the stack, and in so doing it may change the return addresses for procedures taht have called it. An attacker can experiment to find a specific type of data that can be provided to such a program such that the return address of the current procedure is reset to point to an area within the stack itself (and within the data provided by the attacker), which in turn contains instructions that carry out unauthorized operations.

This type of attack is a variation on the buffer overflow attack and is an extremely frequent source of security breaches in software, mainly because some of the most popular compilers use a shared stack for both data and procedure calls, and do not verify the length of data items. Frequently programmers do not write code to verify the size of data items, either, and when an oversized or undersized data item is copied to the stack, a security breach may occur.



## References:

[^1]:https://en.wikipedia.org/wiki/Stack_(abstract_data_type)
[^2]:https://en.wikipedia.org/wiki/Stack-based_memory_allocation
[^3]:https://en.wikipedia.org/wiki/Stack_machine