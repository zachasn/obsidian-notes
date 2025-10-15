## Overview 
- A Linear Data structure that follows **LIFO** (Last-In-First-Out) Principle
- **LIFO Principle:
	1. New elements are always pushed on top
	2. Removal(pop) also happens only from the top
	3. This ensures a strict order — last in → first out

## Implementation
1. **Array-based:** Excellent cache locality and memory overhead. It's ideal for scenarios where the max stack size is known
2. **Linked List-based:** Excels in scenarios where the stack size is varies and efficient memory wise

## Operations

##### `Push()` 
