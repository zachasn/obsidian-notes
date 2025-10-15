---
tags:
  - type/lecture
  - course/stat3021
  - subject/statistics
course: STAT 3021
topic(s): Probability of an Event, Additive Rules
---
# Probability
## Overview
Probability provides a framework for quantifying uncertainty and making informed decisions under uncertainty. This note covers fundamental probability concepts, the probability of events, and additive rules.
## Sample Spaces and Events
### Sample Space
**Definition**: The **sample space** $S$ of an experiment is the set of all possible outcomes.

**Examples**:
- Tossing a coin: $S = \{H, T\}$
- Rolling a die: $S = \{1, 2, 3, 4, 5, 6\}$
- Measuring lifetime (hours) of a component: $S = \{t \mid t \geq 0\}$
### Events
**Definition**: An **event** is a subset of the sample space $S$.
**Simple event**: An event containing exactly one outcome
**Compound event**: An event containing more than one outcome
**Null event** $\emptyset$: The empty set (impossible event)
**Certain event**: The entire sample space $S$
## Set Operations
### Basic Operations
Given events $A$ and $B$:
1. **Union** ($A \cup B$): The event that $A$ or $B$ or both occur
2. **Intersection** ($A \cap B$): The event that both $A$ and $B$ occur
3. **Complement** ($A'$ or $A^c$): The event that $A$ does not occur
### Properties
- **Mutually Exclusive (Disjoint)**: Events $A$ and $B$ are mutually exclusive if $A \cap B = \emptyset$
- **Exhaustive**: Events $A_1, A_2, \ldots, A_n$ are exhaustive if $A_1 \cup A_2 \cup \cdots \cup A_n = S$
### De Morgan's Laws
$$(A \cup B)' = A' \cap B'$$
$$(A \cap B)' = A' \cup B'$$
## Probability Axioms
For any event $A$ in sample space $S$:
1. **Non-negativity**: $0 \leq P(A) \leq 1$
2. **Certainty**: $P(S) = 1$
3. **Additivity**: If $A$ and $B$ are mutually exclusive, then:
   $$P(A \cup B) = P(A) + P(B)$$
## Classical Probability
When all outcomes are equally likely:
$$P(A) = \frac{n(A)}{n(S)} = \frac{\text{number of outcomes in } A}{\text{total number of outcomes}}$$
**Example**: Probability of rolling an even number on a fair die:
$$P(\text{even}) = \frac{3}{6} = \frac{1}{2}$$
## Additive Rules
### Rule 1: General Additive Rule
For any two events $A$ and $B$:
$$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$
> [!important] Why subtract $P(A \cap B)$?
> When we add $P(A)$ and $P(B)$, we count the intersection twice. Subtracting $P(A \cap B)$ corrects for this double-counting.
### Rule 2: Mutually Exclusive Events
If $A$ and $B$ are mutually exclusive (i.e., $A \cap B = \emptyset$):
$$P(A \cup B) = P(A) + P(B)$$
### Rule 3: Multiple Events
For mutually exclusive events $A_1, A_2, \ldots, A_n$:
$$P(A_1 \cup A_2 \cup \cdots \cup A_n) = P(A_1) + P(A_2) + \cdots + P(A_n)$$
## Complement Rule
For any event $A$:
$$P(A') = 1 - P(A)$$
**Proof**: Since $A$ and $A'$ are mutually exclusive and $A \cup A' = S$:
$$P(A \cup A') = P(A) + P(A') = P(S) = 1$$
Therefore, $P(A') = 1 - P(A)$
## Examples
### Example 1: Card Drawing
A card is drawn from a standard 52-card deck. Find the probability that it is either a king or a heart.

**Solution**:
Let $K$ = event of drawing a king, $H$ = event of drawing a heart
- $P(K) = \frac{4}{52}$
- $P(H) = \frac{13}{52}$
- $P(K \cap H) = \frac{1}{52}$ (king of hearts)
Using the additive rule:
$$P(K \cup H) = P(K) + P(H) - P(K \cap H) = \frac{4}{52} + \frac{13}{52} - \frac{1}{52} = \frac{16}{52} = \frac{4}{13}$$
### Example 2: Manufacturing Defects
In a batch of components, 5% have defect A, 3% have defect B, and 1% have both defects. What is the probability that a randomly selected component has at least one defect?

**Solution**:
Let $A$ = defect A, $B$ = defect B
Given: $P(A) = 0.05$, $P(B) = 0.03$, $P(A \cap B) = 0.01$
$$P(A \cup B) = P(A) + P(B) - P(A \cap B) = 0.05 + 0.03 - 0.01 = 0.07$$
### Example 3: Complement Rule Application
The probability that a system fails is 0.02. What is the probability it operates successfully?
**Solution**:
Let $F$ = system fails
$$P(F') = 1 - P(F) = 1 - 0.02 = 0.98$$
## Key Relationships
### Three Events
For events $A$, $B$, and $C$:
$$P(A \cup B \cup C) = P(A) + P(B) + P(C) - P(A \cap B) - P(A \cap C) - P(B \cap C) + P(A \cap B \cap C)$$
### Probability of Difference
The probability that $A$ occurs but $B$ does not:
$$P(A \cap B') = P(A) - P(A \cap B)$$
## Important Notes
> [!tip] Checking for Mutual Exclusivity
> Before applying the simpler additive rule, always verify that events are mutually exclusive. Two events cannot occur simultaneously if they are mutually exclusive.

> [!warning] Common Mistake
> Don't forget to subtract $P(A \cap B)$ when using the general additive rule. Using $P(A) + P(B)$ for non-mutually exclusive events will overcount the probability.
## Summary

| Concept               | Formula                                   |
| --------------------- | ----------------------------------------- |
| Classical Probability | $P(A) = \frac{n(A)}{n(S)}$                |
| General Additive Rule | $P(A \cup B) = P(A) + P(B) - P(A \cap B)$ |
| Mutually Exclusive    | $P(A \cup B) = P(A) + P(B)$               |
| Complement Rule       | $P(A') = 1 - P(A)$                        |
| Probability Range     | $0 \leq P(A) \leq 1$                      |

## Conditional Probability

### Definition
The **conditional probability** of event $A$ given that event $B$ has occurred is:
$$P(A \mid B) = \frac{P(A \cap B)}{P(B)}, \quad P(B) > 0$$

**Interpretation**: The probability of $A$ occurring, given that we know $B$ has occurred.

### Multiplicative Rule
Rearranging the conditional probability formula:
$$P(A \cap B) = P(B) \cdot P(A \mid B) = P(A) \cdot P(B \mid A)$$

For multiple events:
$$P(A_1 \cap A_2 \cap \cdots \cap A_n) = P(A_1) \cdot P(A_2 \mid A_1) \cdot P(A_3 \mid A_1 \cap A_2) \cdots P(A_n \mid A_1 \cap \cdots \cap A_{n-1})$$

### Independence
Events $A$ and $B$ are **independent** if and only if:
$$P(A \cap B) = P(A) \cdot P(B)$$

Equivalently, if $A$ and $B$ are independent:
$$P(A \mid B) = P(A) \quad \text{and} \quad P(B \mid A) = P(B)$$

> [!note] Independence vs. Mutual Exclusivity
> - **Independent events**: Knowledge of one does NOT affect the probability of the other
> - **Mutually exclusive events**: If one occurs, the other CANNOT occur
> - These are different concepts! Mutually exclusive events are NOT independent (except in trivial cases).
## Law of Total Probability
If $B_1, B_2, \ldots, B_k$ form a **partition** of the sample space $S$ (mutually exclusive and exhaustive), then for any event $A$:
$$P(A) = \sum_{i=1}^{k} P(A \cap B_i) = \sum_{i=1}^{k} P(B_i) \cdot P(A \mid B_i)$$
**Visual Interpretation**: The probability of $A$ is the sum of probabilities of $A$ occurring with each possible scenario $B_i$.
### Example: Law of Total Probability
A factory has three machines: Machine 1 produces 50% of items, Machine 2 produces 30%, and Machine 3 produces 20%. The defect rates are 1%, 2%, and 3% respectively. What is the probability that a randomly selected item is defective?

**Solution**:
Let $D$ = defective item, $M_1, M_2, M_3$ = machines 1, 2, 3
Given:
- $P(M_1) = 0.50$, $P(M_2) = 0.30$, $P(M_3) = 0.20$
- $P(D \mid M_1) = 0.01$, $P(D \mid M_2) = 0.02$, $P(D \mid M_3) = 0.03$

Using the law of total probability:
$$\begin{align}
P(D) &= P(M_1) \cdot P(D \mid M_1) + P(M_2) \cdot P(D \mid M_2) + P(M_3) \cdot P(D \mid M_3) \\
&= (0.50)(0.01) + (0.30)(0.02) + (0.20)(0.03) \\
&= 0.005 + 0.006 + 0.006 \\
&= 0.017
\end{align}$$
## Key Formulas Summary

| Concept                  | Formula                                            |
| ------------------------ | -------------------------------------------------- |
| Conditional Probability  | $P(A \mid B) = \frac{P(A \cap B)}{P(B)}$           |
| Multiplicative Rule      | $P(A \cap B) = P(A) \cdot P(B \mid A)$             |
| Independence             | $P(A \cap B) = P(A) \cdot P(B)$                    |
| Law of Total Probability | $P(A) = \sum_{i=1}^{k} P(B_i) \cdot P(A \mid B_i)$ |
