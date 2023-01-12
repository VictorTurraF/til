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
stack.pop(); // 3
stack.pop(); // 2
```

