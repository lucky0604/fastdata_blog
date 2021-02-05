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
  
  public void deleteFront() {
    if (head == null) {
      return;
    }
    Node<T> tmp = head;
    head = tmp.getNextRef();
    if (head == null) {
      tail = null;
    }
  }
  
  public void deleteAfter(T after) {
    Node<T> tmp = head;
    Node<T> refNode = null;
    // traverse till given element
    while (true) {
      if (tmp == null) {
        break;
      }
      if (tmp.compareTo(after) == 0) {
        // found the target node, add after the node
        refNode = tmp;
        break;
      }
      tmp = tmp.getNextRef();
    }
    if (refNode != null) {
      tmp = refNode.getNextRef();
      refNode.setNextRef(tmp.getNextRef());
      if (refNode.getNextRef() == null) {
        tail = refNode;
      }
    }
  }
  
  public void traverse() {
    Node<T> tmp = head;
    while (true) {
      if (tmp == null) {
        break;
      }
      tmp = tmp.getNextRef();
    }
  }
}
```

### Doubly Linked List[^2]

In a `doubly linked list`, each node contains, besides the next-node link, a second link field pointing to the `previous` node in the sequence. The two links may be called `forward('s')` and `backwards`, or `next` and `prev(previous)`

```java
// Node class
public class Node {
  T element;
  Node prev;
  Node next;
  public Node(T element, Node next, Node prev) {
    this.element = element;
    this.next = next;
    this.prev = prev;
  }
}

public class DoublyLinkedList {
  private int size;
  private Node head;
  private Node tail;
  
  public DoublyLinkedList() {
    size = 0;
  }
  
  public int size() {
    return size;
  }
  
  public boolean isEmpty() {
    return size == 0;
  }
  
  public void addFirst(T element) {
    Node tmp = new Node(element, head, null);
    if (head != null) {
      head.prev = tmp;
    }
    head = tmp;
    if (tail == null) {
      tail = tmp;
    }
    size ++;
  }
  
  public void addAfter(T element) {
    Node tmp = new Node(element, null, tail);
    if (tail != null) {
      tail.next = tmp
    }
    tail = tmp;
    if (head == null) {
      head = tmp;
    }
    size ++;
  }
  
  public void iterateForward() {
    Node tmp = head;
    while (tmp != null) {
      tmp = tmp.next;
    }
  }
  
  public void iterateBackward() {
    Node tmp = tail;
    while (tmp != null) {
      tmp = tmp.prev;
    }
  }
  
  public T removeFirst() {
    if (size == 0) throw new NoSuchElementException();
    Node tmp = head;
    head = head.next;
    head.prev = null;
    size --;
    return tmp.element;
  }
  
  public T removeLast() {
    if (size == 0) throw new NoSuchElementException();
    Node tmp = tail;
    tail = tail.prev;
    tail.prev = null;
    size --;
    return tmp.element;
  }
}
```

### Multiply Linked List

In a `multiply linked list`, each node contains two or more link fields, each field being used to connect the same set of data records in a different order of same set (e.g., by name, by department, by date of birth, etc.). While doubly linked lists can be seen as special cases of multiply linked list, the fact that the two and more orders are opposite to each other leads to simpler and more efficient algorithms, so they are usually treated as a separate case.

### Circular Linked List

In the last node of a list, the link field often contains a null reference, a special value is used to indicate the lack of further nodes. A less common convention is to make it point to the first node of the list; in that case, the list is said to be `circular` or `circular linked`; otherwise, it is said to be `open` or `linear` . It is a list where the last pointer points to the first node.

In the case of a circular doubly linked list, the first node also points to the last node of the list.

```java
// Node class
public class Node {
  T val;
  Node<T> next;
  
  public Node(T val) {
    this.val = val;
  }
}

public class CircularLinkedList<T> {
  private Node<T> head;
  private int size;
  
  public CircularLinkedList() {
    size = 0;
  }
  
  public void insertBeginning(T val) {
    Node<T> element = new Node<T>(val);
    if (head == null) {
      head = element;
    } else {
      Node<T> tmp = head;
      element.next = tmp;
      head = element;
    }
    size ++;
  }
  
  public void insertAfter(T val) {
    Node<T> element = new Node<T>(val);
    if (head == null) {
      head = element;
    } else {
      Node<T> tmp = head;
      while (tmp != head) {
        tmp = tmp.next;
      }
      tmp.next = element;
    }
    size ++;
  }
  
  public void insertAtPosition(T val, int position) {
    if (position < 0 || position > size) {
      throw new IllegalArgumentException("Invalid position");
    }
    Node<T> element = new Node<T>(val);
    Node<T> tmp = head;
    Node<T> prev = null;
    for (int i = 0; i < position; i ++) {
      if (tmp.next == head) {
        break;
      }
      prev = tmp;
      tmp = tmp.next;
    }
    prev.next = element;
    element.next = tmp;
    size ++;
  }
  
  public void deleteFromBeginning() {
    Node<T> tmp = head;
    while (tmp.next != head) {
      tmp = tmp.next;
    }
    tmp.next = head.next;
    head = head.next;
    size --;
  }
  
  public void deleteFromPosition(int position) {
    if (position < 0 || position >= size) {
      throw new IllegalArgumentExcetpion("Invalid position");
    }
    Node<T> curr = head;
    Node<T> prev = head;
    for (int i = 0; i < position; i ++) {
      if (curr.next == head) break;
      prev = curr;
      curr = curr.next;
    }
    if (potition == 0) {
      deleteFromBeginning();
    } else {
      prev.next = curr.next;
    }
    size --;
  }
  
  public Node<T> searchByPosition(int position) {
    if (position < 0 || position > size) {
      throw new IllegalArgumentExcetipion("Invalid position");
    }
    Node<T> tmp = head;
   	for (int i = 0; i < position; i ++) {
      tmp = tmp.next;
    }
    return tmp;
  }
  
  public Node<T> searchByValue(T val) {
    Node<T> tmp = head;
    while (tmp != null && tmp.val != val) {
      tmp = tmp.next;
    }
    if (tmp.val == val) {
      return tmp;
    }
    return null;
  }
  
  public int size() {
    return size;
  }
  
  public boolean isEmpty() {
    return size == 0;
  }
}
```

## Tradeoffs

### Linked Lists vs. Dynamic arrays

|                            | Linked List                                                  | Array       | Dynamic array         | Balanced tree  | Random access list | Hashed array tree     |
| -------------------------- | ------------------------------------------------------------ | ----------- | --------------------- | -------------- | ------------------ | --------------------- |
| indexing                   | $\theta(n)$                                                  | $\theta(1)$ | $\theta(1)$           | $\theta(logn)$ | $\theta(logn)$     | $\theta(1)$           |
| insert/delete at beginning | $\theta(1)$                                                  | N/A         | $\theta(n)$           | $\theta(logn)$ | $\theta(1)$        | $\theta(n)$           |
| Insert/delete at end       | $\theta(1)$ when last element is known;  $\theta(n)$ when last element is unknown; | N/A         | $\theta(1)$ amortized | $\theta(logn)$ | N/A                | $\theta(1)$ amortized |
| insert/delete in middle    | search time + $\theta(1)$                                    | N/A         | $\theta(n)$           | $\theta(logn)$ | N/A                | $\theta(n)$           |
| Wasted space (average)     | $\theta(n)$                                                  | 0           | $\theta(n)$           | $\theta(n)$    | $\theta(n)$        | $\theta(\sqrt{n})$    |



>  A dynamic array is a data structure that allocates all element contiguously in memory, and keeps a count of the current number of elements. If the space reserved for the dynamic array is exceeded, it is reallocated and (possibly) copied, which is an expensive operation.

Linked lists have several advantages over dynamic arrays. Insertion or deletion of an element at a specific point of a list, assuming that we have indexed a pointer to the node (before the one to be removed, or before the insertion point) already, is a **constant-time** operation (otherwise without this reference it is $$O(n)$$), whereas insertion in a dynamic array at random locations will require moving half of the elements on average, and all the elements in the worst case. While one can "delete" an element from an array in constant time by somehow marking its slot as "vacant", this causes fragmentation that impedes the performance of iteration.



## References

[^1]:https://en.wikipedia.org/wiki/Linked_list
[^2]: https://en.wikipedia.org/wiki/Doubly_linked_list