---
tags:
  - type/lecture
  - course/stat3021
  - subject/statistics
course: STAT 3021
topic(s): Sample Space, Events
---
# Sample Space & Events
## Terms
- **Statistics:** A procedure of *collecting*, *organizing*, *interpreting* data
- **Population:** Collection of individual units (person, things, etc)
- **Parameter:** A number that describes a population
- **Sample space:** The set of all possible outcomes of a statistical experiment (denoted $S$)
- **Sample size:** The number of elements in the sample
- **Statistic:** A number that describes a sample
- **Descriptive statistics:** To summarize and describe a sample
- **Inferential statistics:** Use data to infer a conclusion from a population
- **Mean:** Numerical average
- **Median:** The middle value
- **Event:** A subset of the sample space
- **Simple event:** An event containing exactly one outcome
- **Compound event:** An event containing more than one outcome
- **Null event:** The empty set $\emptyset$ (an event with no outcomes)
## Examples of Sample Spaces
### Discrete Sample Spaces
**Example 1:** Tossing a coin
- $S = \{\text{H}, \text{T}\}$

**Example 2:** Rolling a die
- $S = \{1, 2, 3, 4, 5, 6\}$
- Event $A$ (rolling an even number): $A = \{2, 4, 6\}$

**Example 3:** Tossing three coins
- $S = \{\text{HHH}, \text{HHT}, \text{HTH}, \text{HTT}, \text{THH}, \text{THT}, \text{TTH}, \text{TTT}\}$
- Event $B$ (exactly 2 heads): $B = \{\text{HHT}, \text{HTH}, \text{THH}\}$
### Continuous Sample Spaces
**Example 4:** Measuring temperature
- $S = \{t \mid 0 \leq t \leq 100\}$ (all real numbers between 0 and 100)

## Set Operations
#### Complement
- **Complement of $A$:** Denoted $A'$ or $\bar{A}$
- Contains all outcomes in $S$ that are not in $A$
- $A' = \{x \in S \mid x \notin A\}$
#### Union
- **Union of $A$ and $B$:** Denoted $A \cup B$
- Contains all outcomes in $A$ or $B$ (or both)
- $A \cup B = \{x \mid x \in A \text{ or } x \in B\}$
#### Intersection
- **Intersection of $A$ and $B$:** Denoted $A \cap B$
- Contains all outcomes in both $A$ and $B$
- $A \cap B = \{x \mid x \in A \text{ and } x \in B\}$
#### Mutually Exclusive Events
- Events $A$ and $B$ are **mutually exclusive** (or **disjoint**) if they have no outcomes in common
- $A \cap B = \emptyset$
- Cannot occur simultaneously
#### Venn Diagrams
- Visual representation of sample spaces and events
- Rectangle represents sample space $S$
- Circles/regions represent events
- Useful for visualizing set operations

## Laws of Set Theory
#### Commutative Laws
- $A \cup B = B \cup A$
- $A \cap B = B \cap A$
#### Associative Laws
- $(A \cup B) \cup C = A \cup (B \cup C)$
- $(A \cap B) \cap C = A \cap (B \cap C)$
#### Distributive Laws
- $A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$
- $A \cup (B \cap C) = (A \cup B) \cap (A \cup C)$
#### De Morgan's Laws
- $(A \cup B)' = A' \cap B'$
- $(A \cap B)' = A' \cup B'$
#### Identity Laws
- $A \cup \emptyset = A$
- $A \cap S = A$
- $A \cup S = S$
- $A \cap \emptyset = \emptyset$
#### Complement Laws
- $A \cup A' = S$
- $A \cap A' = \emptyset$
- $(A')' = A$
- $S' = \emptyset$
- $\emptyset' = S$
## Counting Sample Points
#### Multiplication Rule
If an operation can be performed in $n_1$ ways, and for each of these a second operation can be performed in $n_2$ ways, then the two operations can be performed together in $n_1 \times n_2$ ways.

**Extended:** For $k$ operations with $n_1, n_2, \ldots, n_k$ ways respectively:
$$\text{Total ways} = n_1 \times n_2 \times \cdots \times n_k$$
#### Permutations
- **Permutation:** An arrangement of objects in a specific order
- Number of permutations of $n$ distinct objects taken $r$ at a time:
$$P(n,r) = \frac{n!}{(n-r)!} = n(n-1)(n-2)\cdots(n-r+1)$$

- Special case: Permutations of all $n$ objects:
$$P(n,n) = n!$$
#### Combinations
- **Combination:** A selection of objects without regard to order
- Number of combinations of $n$ distinct objects taken $r$ at a time:
$$C(n,r) = \binom{n}{r} = \frac{n!}{r!(n-r)!}$$
#### Permutations with Repetition
- Number of permutations of $n$ objects where $n_1$ are of one type, $n_2$ are of another type, etc.:
$$\frac{n!}{n_1! \cdot n_2! \cdot \ldots \cdot n_k!}$$
where $n_1 + n_2 + \cdots + n_k = n$

## Key Formulas Summary

| Concept                      | Formula                                              |
| ---------------------------- | ---------------------------------------------------- |
| Permutations                 | $P(n,r) = \frac{n!}{(n-r)!}$                         |
| Combinations                 | $C(n,r) = \binom{n}{r} = \frac{n!}{r!(n-r)!}$        |
| Permutations with repetition | $\frac{n!}{n_1! \cdot n_2! \cdot \ldots \cdot n_k!}$ |
| Multiplication rule          | $n_1 \times n_2 \times \cdots \times n_k$            |
## Practice
1. An experiment involves tossing a pair of dice, one green and one red, and recording the numbers that come up. If $x$ equals the outcome on the green die and $y$ the outcome on the red die, describe the sample space $S$ by (a) by listing the elements (x, y), and (b) by using the rule method.
	- 
2. An experiment consists of tossing a die and then flipping a coin once if the number on the die is even. If the number on the die is odd, the coin is flipped twice. Using the notation $4H$, for example, to denote the outcome that the die comes up 4 and then the coin comes up heads, and $3HT$ to denote the outcome that the die comes up 3 followed by a head and then a tail on the coin, construct a tree diagram to show the 18 elements of the sample space $S$.
	- 
3. If $S = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}$ and $A = {0, 2, 4, 6, 8}, B = {1, 3, 5, 7, 9}$, $C = {2, 3, 4, 5}$, and $D = {1, 6, 7}$, list the elements of the sets corresponding to the following events:
	- $A ∪ C$
	- $A ∩ B$
	- $C'$
	- $(C' ∩ D) ∪ B$
	- $(S ∩ C)'$
	- $A ∩ C ∩ D'$
4. Let $A$, $B$, and $C$ be events relative to the sample space $S$. Using Venn diagrams, shade the areas representing the following events:
	- $(A ∩ B)'$
	- $(A ∪ B)'$
	- $(A ∩ C) ∪ B$