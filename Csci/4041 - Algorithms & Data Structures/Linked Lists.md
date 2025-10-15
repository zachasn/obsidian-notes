## Overview
- A linked list is a linear data structure where elements(called nodes) are stored in sequence, but unlike arrays, the elements are not stored in continuous memory location. Instead, each node contains a pointer/reference to the next node in the sequence.

## Terminologies
- **Node Structure:** A node typically consists of two components: 
```cpp
#include <iostream>
struct Node {
	int data; // The actual value or data being stored within the node
	Node* next; // The memory address(reference) of the next node
	Node(int val) : data(val), next(nullptr) {}
};
``` 
- **Head & Tail:** 
  ```cpp
class LinkedList {
	private:
		Node* head; // Points to the first node in the list
		Node* tail; // POints to the last node in the list,
};
``` 

## Types 
- *Singly Linked List:* Each node points the next node, last node points to null
  [A]→[ B ]→[C]→[D]→ NULL
- *Doubly Linked List:* Each node has a pointer to **both** the next and previous nodes
   NULL←[A]→←[B]→←[C]→←[D]→ NULL
- *Circular Linked List:* The last node points back to the first node
  [A]→[B]→[C]→[D] → [A]
- *Doubly Circular Linked List:* The first and last nodes connect to each other
  [D] ← [A]→←[B]→← [C]→←[D] → [A]

## Complexity

| Operation              | Time Complexity | Auxiliary Space | Explanation                                                            |
| ---------------------- | --------------- | --------------- | ---------------------------------------------------------------------- |
| Insertion at Beginning | O(1)            | O(1)            | Constant-time pointer updates                                          |
| Insertion at End       | O(N)            | O(1)            | Traversal required to find the last node                               |
| Insertion at Position  | O(N)            | O(1)            | Traversal to the desired position, then constant-time pointer updates  |
| Deletion at Beginning  | O(1)            | O(1)            | Constant-time pointer update                                           |
| Deletion at End        | O(N)            | O(1)            | Traversal required to find the second last node                        |
| Deletion at Position   | O(N)            | O(1)            | Traversal to the desired position, then constant-time pointer updates. |
| Search                 | O(N)            | O(1)            | Traversal through the list to find the desired value                   |

>[!tip] Key Point
>Linked Lists excel at insertion and deletion at beginning.
>They're also space efficient as a node is only created when needed.

## When to Use:
- Frequent insertion and deletion 
- Can be used to implement [[Stacks]], [[Queue]], and other [[abstract Data Structures]] 
- Might be more space efficient compared to arrays in cases where we cannot guess the number of elements in advance

