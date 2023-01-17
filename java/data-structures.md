# Java Data Structures

## Arrays
Use arrays always you want to create a list of elements with a defined number of elements. Arrays are fixed size, so you can't add or remove elements from it. Arrays are indexed, so you can access the elements by the index.

### Declaration
```java
// Declaration
int[] array = new int[10];

// Declaration and initialization
int[] array = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
```

## Linked Lists
Linked lists are a list of elements that are linked to each other. Each element has a reference to the next element. Linked lists are dynamic, so you can add or remove elements from it. Linked lists are not indexed, so search elements by index may not be so efficient. Use linked lists when you don't know the number of elements that you will have (fine when you need priorize memory space), and if you will not need search elements by index.

### Declaration
```java
// Declaration
LinkedList<Integer> linkedList = new LinkedList<Integer>();

// Declaration and initialization
LinkedList<Integer> linkedList = new LinkedList<Integer>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
```

## Stacks
Stacks are LIFO (Last In First Out) data structures. Use stacks when you need to store elements and get the last element added. Stacks are dynamic, so you can add or remove elements from it. Stacks are not indexed, so search elements by index may not be so efficient.

### Declaration
```java
// Declaration
Stack<Integer> stack = new Stack<Integer>();

// Pushing elements
stack.push(1);
stack.push(2);
stack.push(3);

// Popping elements
stack.pop(); // returns 3
stack.pop(); // returns 2
```

## Hash Tables
Hash tables are a data structure that stores elements in a key-value pair. Use hash tables when you need to store elements and get them by a key. Hashtables improves searchs in cases where you can store values indexed by a knowed key (a string in the most of the cases). Hash tables are dynamic, so you can add or remove elements from it.

### Declaration
```java
// Declaration
Hashtable<String, Integer> hashTable = new Hashtable<String, Integer>();

// Adding elements
hashTable.put("one", 1);
hashTable.put("two", 2);
hashTable.put("three", 3);

// Getting elements
hashTable.get("one"); // returns 1
hashTable.get("two"); // returns 2
```

## Queues
Queues are FIFO (First In First Out) data structures. Use queues when you need to store elements and get the first element added. Queues are dynamic, so you can add or remove elements from it. Queues are not indexed, so search elements by index may not be so efficient.

### Declaration
```java
// Declaration
Queue<Integer> queue = new Queue<Integer>();    

// Adding elements
queue.add(1);
queue.add(2);
queue.add(3);

// Getting elements
queue.remove(); // returns 1
queue.remove(); // returns 2
```

## Trees
Trees are a data structure that stores elements in a hierarchical way. Trees are dynamic, so you can add or remove elements from it. Tree are commonly used to represent file systems, improved searchs strategies, sorting, etc.

## Heap 
It is a based tree data structure, it is an almost complete binary tree that satisfies the heap property. The heap property is that each node is greater than or equal to each of its children (the inverse is equal to a minimun heap tree). The root of the tree is the maximum element in the tree.
Heaps are crucial in some of the graphs algorithms, like Dijkstra's algorithm.
An use of a heap is to implement the sorting algorithm heapsort.

## Graphs
Graphs are a data structure that stores elements in a way that each element is connected to other elements. In other words, graphs are a set of nodes and edges. Are commonly used to represent maps, networks, and other things that can be represented as a set of nodes and edges. A real usage of graphs is to represent locations and distances in a map, and then use the path finder algorithms to find the shortest path between two locations.