# LinkedList[^1]

[TOC]

## Definition

A Linked List is a linear collection of data elements whose order is not given by their physical placement in memory. Instead, each element `points` to the next. 

It is a data structure consisting of a collection of nodes which together represent a sequence. In its most basic form, each node contains: `data and a reference (in other words, a link) to the next node in the sequence.` This structure allows for efficient insertion or removal of elements from any position in the sequence during iteration. More complex variants add additional links, allowing more efficient insertion or removal of nodes at arbitrary positions. A drawback of linked lists is that access time is linear (and difficult to pipeline). 

Faster access, such as random access, is not feasible. Arrays have better cache locality compared to linked lists.

### Implementation in Java

```java
public class Node<T> implements Comparable<T> {
  
  private T value;
  private Node<T> nextRef;
  
  public T getValue() {
    return value;
  }
  public void setValue(T value) {
    this.value = value
  }
  public Node<T> getNextRef() {
    return nextRef;
  }
  public void setNextRef(Node<T> ref) {
    this.nextRef = ref;
  }
  
  @Override
  public int compareTo(T arg) {
    if (arg == this.value) {
      return 0;
    } else {
      return 1;
    }
  }
}
```



## Disadvantages

- They use more memory than arrays because of the storage used by their pointers.
- Nodes in a linked list must be read in order from the beginning as linked lists are inherently sequential access
- Nodes are stored noncontiguously, greatly increasing the time periods required to access individual elements within the list, especially with a `CPU cache`
- Difficulties arise in linked lists when it comes to reverse traversing. For instance, singly-linked lists are cumbersome to navigate backward and while doubly linked lists are somewhat easier to read, memory is consumed in allocating space for a back-pointer

## Basic concepts and nomenclature

### Singly Linked List

Singly linked lists contain nodes which have a data filed as well as 'next' field, which points to the next node in line of nodes. Operations that can be performed on singly linked lists include insertion, deletion and traversal.

```java
// Singly Linked List operation
public class SinglyLinkedList<T> {
  private Node<T> head;
  private Node<T> tail;
  
  public void add(T element) {
    Node<T> node = new Node<T>();
    node.setValue(element);
    if (head == null) {
      // since there is only one element, both head and tail points to the same object
      head = node;
      tail = node;
    } else {
      // set current tail next link to new node
      tail.setNextRef(node);
      // set tail as newly created node
      tail = node;
    }
  }
  
  public void addAfter(T element, T after) {
    Node<T> tmp = head;
    Node<T> refNode = null;
    // Traverse till given element
    while (true) {
      if (tmp == null) {
        break;
      }
      if (tmp.compareTo(after) == 0) {
        // found the target node, add after this node
        refNode = tmp;
        break;
      }
      tmp = tmp.getNextRef();
    }
    if (refNode != null) {
      // add element after the target node
      Node<T> node = new Node<T>();
      node.setValue(element);
      node.setNextRef(tmp.getNextRef());
      if (tmp == tail) {
        tail = node;
      }
      tmp.setNextRef(node);
    } else {
      throw new Exception("Unable to find the given element")
    }
  }
}
```









## References

[^1]:https://en.wikipedia.org/wiki/Linked_list