---
tags:
  - type/lecture
  - course/stat3021
  - subject/statistics
course: STAT 3021
topic(s): Counting Sample Points
---
# Counting
## Overview
Counting techniques are fundamental to probability theory. Before we can calculate probabilities, we need to determine the number of outcomes in sample spaces and events.
## Fundamental Counting Principles
### Multiplication Principle
> [!important] Multiplication Principle
> If an operation can be performed in $n_1$ ways, and for each of these ways a second operation can be performed in $n_2$ ways, then the two operations can be performed together in $n_1 \times n_2$ ways.

**Generalization**: For $k$ operations that can be performed in $n_1, n_2, \ldots, n_k$ ways respectively, the total number of ways to perform all operations is:
$$n_1 \times n_2 \times \cdots \times n_k$$
**Example**: If a license plate has 3 letters followed by 4 digits:
- Letters: $26 \times 26 \times 26 = 17,576$ ways
- Digits: $10 \times 10 \times 10 \times 10 = 10,000$ ways
- Total: $17,576 \times 10,000 = 175,760,000$ possible plates
### Addition Principle
> [!important] Addition Principle
> If operation $A$ can be performed in $n$ ways and operation $B$ can be performed in $m$ ways, and the two operations cannot be performed simultaneously, then performing either $A$ or $B$ can be done in $n + m$ ways.

**Example**: If a test has 5 true/false questions OR 3 multiple choice questions (not both), there are $5 + 3 = 8$ total questions to choose from.
## Permutations
### Permutations of $n$ Distinct Objects
> [!note] Definition: Permutation
> A **permutation** is an arrangement of objects in a specific order.

The number of permutations of $n$ distinct objects taken all at a time is:
$$P(n,n) = n! = n \times (n-1) \times (n-2) \times \cdots \times 2 \times 1$$
where $0! = 1$ by definition.

**Example**: The number of ways to arrange 5 books on a shelf is $5! = 120$.
### Permutations of $r$ Objects from $n$ Distinct Objects
The number of permutations of $n$ distinct objects taken $r$ at a time (where $r \leq n$) is:
$$P(n,r) = \frac{n!}{(n-r)!} = n(n-1)(n-2)\cdots(n-r+1)$$
This represents selecting $r$ objects from $n$ objects where **order matters**.

**Example**: How many 3-digit numbers can be formed from {1,2,3,4,5,6,7} without repetition?
$$P(7,3) = \frac{7!}{4!} = 7 \times 6 \times 5 = 210$$
### Permutations with Repetitions
When we have $n$ objects where $n_1$ are of one type, $n_2$ are of another type, ..., and $n_k$ are of a $k$-th type (where $n_1 + n_2 + \cdots + n_k = n$), the number of distinguishable permutations is:
$$\frac{n!}{n_1! \, n_2! \, \cdots \, n_k!}$$
**Example**: How many distinguishable permutations of the letters in MISSISSIPPI?
- Total letters: $n = 11$
- M: 1, I: 4, S: 4, P: 2
$$\frac{11!}{1! \, 4! \, 4! \, 2!} = \frac{39,916,800}{1,152} = 34,650$$
## Combinations
### Combinations of $r$ Objects from $n$ Distinct Objects
> [!note] Definition: Combination
> A **combination** is a selection of objects where **order does not matter**.

The number of combinations of $n$ distinct objects taken $r$ at a time is:
$$C(n,r) = \binom{n}{r} = \frac{n!}{r!(n-r)!}$$
Also written as $_nC_r$ or $\binom{n}{r}$ (read as "$n$ choose $r$").

**Relationship to permutations**:
$$C(n,r) = \frac{P(n,r)}{r!}$$
This makes sense because each combination of $r$ objects can be arranged in $r!$ ways.

**Example**: A committee of 3 people is to be selected from 10 candidates. How many committees are possible?
$$\binom{10}{3} = \frac{10!}{3! \, 7!} = \frac{10 \times 9 \times 8}{3 \times 2 \times 1} = 120$$
### Properties of Combinations
1. **Symmetry**: $\binom{n}{r} = \binom{n}{n-r}$
2. **Pascal's Identity**: $\binom{n}{r} = \binom{n-1}{r-1} + \binom{n-1}{r}$
3. **Sum**: $\sum_{r=0}^{n} \binom{n}{r} = 2^n$
4. **Special cases**:
   - $\binom{n}{0} = 1$
   - $\binom{n}{1} = n$
   - $\binom{n}{n} = 1$
## Key Distinctions

| Scenario                              | Order Matters? | Repetition? | Formula                              |
| ------------------------------------- | -------------- | ----------- | ------------------------------------ |
| Arrange $r$ from $n$ distinct objects | Yes            | No          | $P(n,r) = \frac{n!}{(n-r)!}$         |
| Select $r$ from $n$ distinct objects  | No             | No          | $\binom{n}{r} = \frac{n!}{r!(n-r)!}$ |
| Arrange $n$ with repetitions          | Yes            | Yes         | $\frac{n!}{n_1! n_2! \cdots n_k!}$   |

> [!tip] When to Use Which?
> - **Permutation**: "In how many ways can we arrange/order...?"
> - **Combination**: "In how many ways can we select/choose...?"
> - If the problem mentions "committee", "group", "sample" → usually **combination**
> - If the problem mentions "ranking", "seating", "arrangement" → usually **permutation**
## Applications to Probability
In probability problems, counting techniques help us determine:
- $|S|$ = size of sample space
- $|A|$ = size of event $A$

For equally likely outcomes:
$$P(A) = \frac{|A|}{|S|} = \frac{\text{number of favorable outcomes}}{\text{total number of outcomes}}$$
**Example**: What is the probability of being dealt a 5-card poker hand with exactly 3 aces?
- Total 5-card hands: $\binom{52}{5}$
- Hands with exactly 3 aces: $\binom{4}{3} \times \binom{48}{2}$ (choose 3 of 4 aces, then 2 from remaining 48 cards)
$$P(\text{3 aces}) = \frac{\binom{4}{3} \times \binom{48}{2}}{\binom{52}{5}} = \frac{4 \times 1,128}{2,598,960} = \frac{4,512}{2,598,960} \approx 0.00174$$
## Practice Problems
1. How many 4-letter "words" can be formed from the alphabet if:
   - a) Repetition is allowed?
   - b) Repetition is not allowed?

2. A box contains 15 defective and 35 non-defective items. If 5 items are selected at random, in how many ways can exactly 2 be defective?

3. How many permutations of the letters in STATISTICS are there?

4. From a group of 8 men and 6 women, how many committees of 5 can be formed if:
   - a) There are no restrictions?
   - b) The committee must have exactly 3 men?
   - c) The committee must have at least 4 women?
## Summary
- **Multiplication Principle**: Used when performing multiple operations in sequence
- **Addition Principle**: Used when choosing between mutually exclusive operations
- **Permutations**: Count ordered arrangements ($P(n,r)$ or $n!/(n-r)!$)
- **Combinations**: Count unordered selections ($\binom{n}{r}$ or $n!/(r!(n-r)!)$)
- These techniques are essential for calculating probabilities in discrete sample spaces