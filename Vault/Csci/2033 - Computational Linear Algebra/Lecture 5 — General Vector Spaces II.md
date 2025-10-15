---
tags:
  - type/lecture
  - course/csci2033
  - subject/Csci
course: CSCI 2033
topic(s): Dimension, Properties of a Matrix, Null Space
---
# General Vector Spaces II

## Dimension Theory

### Dimension of a Vector Space
The **dimension** of a vector space $V$, denoted $\dim(V)$, is the number of vectors in any basis for $V$.

### Key Theorems

#### Unique Dimension Theorem
All bases for a vector space $V$ have the same number of vectors.

#### Dimension Theorems
Let $V$ be a vector space with $\dim(V) = n$. Then:

1. **Exactly $n$ vectors:** Any set of exactly $n$ linearly independent vectors in $V$ is a basis for $V$.

2. **Spanning set:** Any spanning set for $V$ contains at least $n$ vectors. If it contains exactly $n$ vectors, it is a basis.

3. **Independent set:** Any linearly independent set in $V$ contains at most $n$ vectors. If it contains exactly $n$ vectors, it is a basis.

4. **Extension theorem:** Any linearly independent set can be extended to a basis by adding vectors.

5. **Reduction theorem:** Any spanning set can be reduced to a basis by removing vectors.

### Dimension of Subspaces

**Theorem:** If $W$ is a subspace of a finite-dimensional vector space $V$, then:
$$\dim(W) \leq \dim(V)$$

Moreover, $\dim(W) = \dim(V)$ if and only if $W = V$.

### Examples of Dimensions
- $\dim(\mathbb{R}^n) = n$
- $\dim(\mathbb{P}_n) = n + 1$ (basis: $\{1, x, x^2, \ldots, x^n\}$)
- $\dim(\mathbb{M}_{m \times n}) = mn$
- $\dim(\{\mathbf{0}\}) = 0$ (the trivial space)

## Properties of Matrices

### Four Fundamental Subspaces
For an $m \times n$ matrix $A$, there are four important subspaces:

1. **Column Space** $\text{Col}(A) \subseteq \mathbb{R}^m$
2. **Row Space** $\text{Row}(A) \subseteq \mathbb{R}^n$
3. **Null Space** $\text{Null}(A) \subseteq \mathbb{R}^n$
4. **Left Null Space** $\text{Null}(A^T) \subseteq \mathbb{R}^m$

### Column Space

**Definition:** The column space of $A$ is the span of the columns of $A$:
$$\text{Col}(A) = \text{span}\{\mathbf{a}_1, \mathbf{a}_2, \ldots, \mathbf{a}_n\}$$

where $\mathbf{a}_1, \ldots, \mathbf{a}_n$ are the columns of $A$.

**Interpretation:** $\text{Col}(A)$ is the set of all vectors $\mathbf{b}$ such that $A\mathbf{x} = \mathbf{b}$ has a solution.

**Finding a basis:** Row reduce $A$ to echelon form. The columns of $A$ (original matrix) corresponding to pivot columns form a basis for $\text{Col}(A)$.

### Row Space

**Definition:** The row space of $A$ is the span of the rows of $A$.

**Equivalent definition:** $\text{Row}(A) = \text{Col}(A^T)$

**Finding a basis:** Row reduce $A$ to echelon form. The nonzero rows of the echelon form constitute a basis for $\text{Row}(A)$.

**Important property:** Row operations do not change the row space.

### Rank
The **rank** of a matrix $A$, denoted $\text{rank}(A)$, is the dimension of the column space (or row space):
$$\text{rank}(A) = \dim(\text{Col}(A)) = \dim(\text{Row}(A))$$

**Theorem (Row Rank = Column Rank):** The dimension of the column space equals the dimension of the row space.

**Computing rank:** The rank equals the number of pivot positions in any echelon form of $A$.

### Properties of Rank
1. $\text{rank}(A) \leq \min(m, n)$ for an $m \times n$ matrix
2. $\text{rank}(A) = \text{rank}(A^T)$
3. $\text{rank}(AB) \leq \min(\text{rank}(A), \text{rank}(B))$
4. If $A$ is invertible, then $\text{rank}(AB) = \text{rank}(B)$ and $\text{rank}(BA) = \text{rank}(B)$

### Full Rank
An $m \times n$ matrix $A$ has:
- **Full column rank** if $\text{rank}(A) = n$ (all columns are linearly independent)
- **Full row rank** if $\text{rank}(A) = m$ (all rows are linearly independent)

For a square $n \times n$ matrix, $A$ has **full rank** if $\text{rank}(A) = n$.

## Null Space

### Definition
The **null space** (or **kernel**) of an $m \times n$ matrix $A$ is:
$$\text{Null}(A) = \{\mathbf{x} \in \mathbb{R}^n : A\mathbf{x} = \mathbf{0}\}$$

This is the solution set of the homogeneous system $A\mathbf{x} = \mathbf{0}$.

### Properties
1. $\text{Null}(A)$ is a subspace of $\mathbb{R}^n$
2. $\text{Null}(A)$ always contains the zero vector
3. $\text{Null}(A) = \{\mathbf{0}\}$ iff the columns of $A$ are linearly independent

### Finding a Basis for Null Space
To find a basis for $\text{Null}(A)$:
1. Row reduce the augmented matrix $[A \mid \mathbf{0}]$ to RREF
2. Identify free variables
3. Express basic variables in terms of free variables
4. Write the general solution as a linear combination
5. The coefficient vectors form a basis for $\text{Null}(A)$

### Example
Find a basis for $\text{Null}(A)$ where $A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \end{bmatrix}$

After row reduction:
$$\begin{bmatrix} 1 & 0 & -1 \\ 0 & 1 & 2 \\ 0 & 0 & 0 \end{bmatrix}$$

General solution: $\mathbf{x} = \begin{bmatrix} x_3 \\ -2x_3 \\ x_3 \end{bmatrix} = x_3\begin{bmatrix} 1 \\ -2 \\ 1 \end{bmatrix}$

Basis: $\left\{\begin{bmatrix} 1 \\ -2 \\ 1 \end{bmatrix}\right\}$

### Nullity
The **nullity** of a matrix $A$, denoted $\text{nullity}(A)$, is the dimension of the null space:
$$\text{nullity}(A) = \dim(\text{Null}(A))$$

**Interpretation:** The nullity equals the number of free variables in the system $A\mathbf{x} = \mathbf{0}$.

## Rank-Nullity Theorem

### Statement
For an $m \times n$ matrix $A$:
$$\text{rank}(A) + \text{nullity}(A) = n$$

Equivalently:
$$\dim(\text{Col}(A)) + \dim(\text{Null}(A)) = n$$

### Interpretation
- $n$ = number of columns = dimension of the domain
- $\text{rank}(A)$ = number of pivot columns = number of basic variables
- $\text{nullity}(A)$ = number of free variables

### Applications

#### Invertibility
For a square $n \times n$ matrix $A$, the following are equivalent:
1. $A$ is invertible
2. $\text{rank}(A) = n$
3. $\text{nullity}(A) = 0$
4. $\text{Null}(A) = \{\mathbf{0}\}$
5. Columns of $A$ are linearly independent
6. Columns of $A$ span $\mathbb{R}^n$
7. $A\mathbf{x} = \mathbf{b}$ has a unique solution for every $\mathbf{b}$

#### Existence and Uniqueness
For the system $A\mathbf{x} = \mathbf{b}$:
- **Solution exists** iff $\mathbf{b} \in \text{Col}(A)$
- **Solution is unique** iff $\text{Null}(A) = \{\mathbf{0}\}$ (i.e., $\text{nullity}(A) = 0$)

### Example
If $A$ is a $5 \times 8$ matrix with $\text{rank}(A) = 3$, then:
- $\text{nullity}(A) = 8 - 3 = 5$
- $\dim(\text{Col}(A)) = 3$
- $\dim(\text{Null}(A)) = 5$
- The system $A\mathbf{x} = \mathbf{0}$ has 5 free variables

## Summary of Matrix Dimensions

For an $m \times n$ matrix $A$:

| Subspace | Subset of | Dimension |
|----------|-----------|-----------|
| $\text{Col}(A)$ | $\mathbb{R}^m$ | $\text{rank}(A)$ |
| $\text{Row}(A)$ | $\mathbb{R}^n$ | $\text{rank}(A)$ |
| $\text{Null}(A)$ | $\mathbb{R}^n$ | $\text{nullity}(A)$ |
| $\text{Null}(A^T)$ | $\mathbb{R}^m$ | $m - \text{rank}(A)$ |

**Key relationships:**
- $\text{rank}(A) + \text{nullity}(A) = n$
- $\dim(\text{Row}(A)) = \dim(\text{Col}(A))$
