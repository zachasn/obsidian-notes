---
tags:
  - type/lecture
  - course/csci2033
  - subject/Csci
course: CSCI 2033
topic(s): Vector Spaces, Subspace of a Vector Space, Linear Independence and Basis
---
# General Vector Spaces
## Vector Spaces
### Definition
A **vector space** over the real numbers is a set $V$ together with two operations:
- **Addition**: $+: V \times V \to V$
- **Scalar multiplication**: $\cdot: \mathbb{R} \times V \to V$
that satisfy the following axioms for all $\mathbf{u}, \mathbf{v}, \mathbf{w} \in V$ and all $c, d \in \mathbb{R}$:

#### Closure Axioms
1. $\mathbf{u} + \mathbf{v} \in V$ (closed under addition)
2. $c\mathbf{u} \in V$ (closed under scalar multiplication)

#### Addition Axioms
3. $\mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u}$ (commutativity)
4. $(\mathbf{u} + \mathbf{v}) + \mathbf{w} = \mathbf{u} + (\mathbf{v} + \mathbf{w})$ (associativity)
5. There exists $\mathbf{0} \in V$ such that $\mathbf{u} + \mathbf{0} = \mathbf{u}$ (additive identity)
6. For each $\mathbf{u} \in V$, there exists $-\mathbf{u} \in V$ such that $\mathbf{u} + (-\mathbf{u}) = \mathbf{0}$ (additive inverse)
#### Scalar Multiplication Axioms
7. $c(\mathbf{u} + \mathbf{v}) = c\mathbf{u} + c\mathbf{v}$ (distributivity over vector addition)
8. $(c + d)\mathbf{u} = c\mathbf{u} + d\mathbf{u}$ (distributivity over scalar addition)
9. $c(d\mathbf{u}) = (cd)\mathbf{u}$ (associativity of scalar multiplication)
10. $1\mathbf{u} = \mathbf{u}$ (multiplicative identity)

### Examples of Vector Spaces
#### $\mathbb{R}^n$
The set of all $n$-tuples of real numbers with standard addition and scalar multiplication.
#### $\mathbb{P}_n$ (Polynomials)
The set of all polynomials of degree at most $n$:
$$\mathbb{P}_n = \{a_0 + a_1x + a_2x^2 + \cdots + a_nx^n : a_i \in \mathbb{R}\}$$
**Operations:**
- $(p + q)(x) = p(x) + q(x)$
- $(cp)(x) = c \cdot p(x)$

**Zero vector:** The zero polynomial $p(x) = 0$
#### $\mathbb{M}_{m \times n}$ (Matrices)
The set of all $m \times n$ matrices with real entries.
**Operations:**
- Matrix addition
- Scalar multiplication

**Zero vector:** The $m \times n$ zero matrix
#### $\mathbb{C}[a,b]$ (Continuous Functions)
The set of all continuous real-valued functions defined on $[a,b]$.

**Operations:**
- $(f + g)(x) = f(x) + g(x)$
- $(cf)(x) = c \cdot f(x)$

**Zero vector:** The zero function $f(x) = 0$
#### $\mathbb{F}(\mathbb{R})$ (All Functions)
The set of all real-valued functions defined on $\mathbb{R}$.
### Non-Examples
Not every set with operations is a vector space!
**Example:** The set of positive real numbers with standard addition is NOT a vector space (no zero element, no additive inverses).

## Subspaces

### Definition
A **subspace** of a vector space $V$ is a nonempty subset $W \subseteq V$ that is itself a vector space under the same operations.

### Subspace Test
A nonempty subset $W$ of vector space $V$ is a subspace iff:
1. **Closed under addition**: If $\mathbf{u}, \mathbf{v} \in W$, then $\mathbf{u} + \mathbf{v} \in W$
2. **Closed under scalar multiplication**: If $\mathbf{u} \in W$ and $c \in \mathbb{R}$, then $c\mathbf{u} \in W$

**Note:** These two conditions are sufficient because the other axioms are inherited from $V$.

**Equivalent formulation:** $W$ is a subspace iff for all $\mathbf{u}, \mathbf{v} \in W$ and all $c, d \in \mathbb{R}$:
$$c\mathbf{u} + d\mathbf{v} \in W$$
(closed under linear combinations)

### Properties
1. Every subspace must contain the zero vector: $\mathbf{0} \in W$
   (Take $c = 0$ in scalar multiplication closure)
2. The trivial subspace $\{\mathbf{0}\}$ is a subspace of every vector space
3. The entire space $V$ is a subspace of itself

### Examples of Subspaces

#### In $\mathbb{R}^3$:
1. The origin: $\{\mathbf{0}\}$
2. Lines through the origin: $\{t\mathbf{v} : t \in \mathbb{R}\}$ for any $\mathbf{v} \neq \mathbf{0}$
3. Planes through the origin
4. All of $\mathbb{R}^3$

**Non-example:** A line NOT through the origin is not a subspace (doesn't contain $\mathbf{0}$).

#### In $\mathbb{P}_3$:
The set of all polynomials of degree at most 2 (i.e., $\mathbb{P}_2$) is a subspace.

#### In $\mathbb{M}_{n \times n}$:
1. The set of all diagonal matrices
2. The set of all symmetric matrices
3. The set of all upper triangular matrices

### Column Space and Row Space
For an $m \times n$ matrix $A$:

**Column space** $\text{Col}(A)$: The subspace of $\mathbb{R}^m$ spanned by the columns of $A$
$$\text{Col}(A) = \text{span}\{\mathbf{a}_1, \mathbf{a}_2, \ldots, \mathbf{a}_n\}$$

**Row space** $\text{Row}(A)$: The subspace of $\mathbb{R}^n$ spanned by the rows of $A$

### Null Space (Kernel)
The **null space** of an $m \times n$ matrix $A$ is:
$$\text{Null}(A) = \{\mathbf{x} \in \mathbb{R}^n : A\mathbf{x} = \mathbf{0}\}$$

This is the set of all solutions to the homogeneous system $A\mathbf{x} = \mathbf{0}$.

**Theorem:** $\text{Null}(A)$ is a subspace of $\mathbb{R}^n$.

## Linear Independence and Basis in General Vector Spaces

### Linear Combination
A **linear combination** of vectors $\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k$ in a vector space $V$ is:
$$c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_k\mathbf{v}_k$$
where $c_i \in \mathbb{R}$.

### Span
The **span** of $\{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k\}$ is the set of all linear combinations:
$$\text{span}\{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k\}$$

**Theorem:** $\text{span}\{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k\}$ is a subspace of $V$.

### Linear Independence
Vectors $\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k$ are **linearly independent** if:
$$c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_k\mathbf{v}_k = \mathbf{0}$$
implies $c_1 = c_2 = \cdots = c_k = 0$.

Otherwise, they are **linearly dependent**.

### Examples

#### Polynomials
The polynomials $1, x, x^2, x^3$ are linearly independent in $\mathbb{P}_3$.

To verify: If $c_0 + c_1x + c_2x^2 + c_3x^3 = 0$ for all $x$, then $c_0 = c_1 = c_2 = c_3 = 0$.

#### Matrices
The matrices $\begin{bmatrix}1&0\\0&0\end{bmatrix}, \begin{bmatrix}0&1\\0&0\end{bmatrix}, \begin{bmatrix}0&0\\1&0\end{bmatrix}, \begin{bmatrix}0&0\\0&1\end{bmatrix}$ are linearly independent in $\mathbb{M}_{2 \times 2}$.

### Basis
A **basis** for a vector space $V$ is a linearly independent set that spans $V$.

### Standard Bases

#### For $\mathbb{R}^n$:
$$\left\{\begin{bmatrix}1\\0\\\vdots\\0\end{bmatrix}, \begin{bmatrix}0\\1\\\vdots\\0\end{bmatrix}, \ldots, \begin{bmatrix}0\\0\\\vdots\\1\end{bmatrix}\right\}$$

#### For $\mathbb{P}_n$:
$$\{1, x, x^2, \ldots, x^n\}$$

#### For $\mathbb{M}_{m \times n}$:
The set of matrices with a single 1 and all other entries 0.

### Dimension
The **dimension** of a vector space $V$ is the number of vectors in any basis for $V$.

**Examples:**
- $\dim(\mathbb{R}^n) = n$
- $\dim(\mathbb{P}_n) = n + 1$
- $\dim(\mathbb{M}_{m \times n}) = mn$

### Theorems
1. All bases for a given vector space have the same number of vectors
2. If $\dim(V) = n$, then:
   - Any linearly independent set of $n$ vectors is a basis
   - Any spanning set of $n$ vectors is a basis
   - Any linearly independent set can be extended to a basis
   - Any spanning set can be reduced to a basis
3. If $W$ is a subspace of $V$, then $\dim(W) \leq \dim(V)$
