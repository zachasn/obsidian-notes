---
tags:
  - type/lecture
  - course/csci2033
  - subject/Csci
course: CSCI 2033
topic(s): Properties of Vectors, Linear Independence, Basis and Spanning Sets
---
# Euclidean Space

## Properties of Vectors in $\mathbb{R}^n$

**Euclidean space** $\mathbb{R}^n$ is the set of all ordered $n$-tuples of real numbers:
$$\mathbb{R}^n = \left\{\begin{bmatrix} x_1 \\ x_2 \\ \vdots \\ x_n \end{bmatrix} : x_i \in \mathbb{R}\right\}$$

### Vector Operations

**Addition:** $\mathbf{u} + \mathbf{v} = \begin{bmatrix} u_1 + v_1 \\ u_2 + v_2 \\ \vdots \\ u_n + v_n \end{bmatrix}$

**Scalar Multiplication:** $c\mathbf{v} = \begin{bmatrix} cv_1 \\ cv_2 \\ \vdots \\ cv_n \end{bmatrix}$

### Algebraic Properties
For vectors $\mathbf{u}, \mathbf{v}, \mathbf{w} \in \mathbb{R}^n$ and scalars $c, d \in \mathbb{R}$:

1. $\mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u}$ (commutativity)
2. $(\mathbf{u} + \mathbf{v}) + \mathbf{w} = \mathbf{u} + (\mathbf{v} + \mathbf{w})$ (associativity)
3. $\mathbf{u} + \mathbf{0} = \mathbf{u}$ (additive identity)
4. $\mathbf{u} + (-\mathbf{u}) = \mathbf{0}$ (additive inverse)
5. $c(d\mathbf{v}) = (cd)\mathbf{v}$
6. $(c + d)\mathbf{v} = c\mathbf{v} + d\mathbf{v}$
7. $c(\mathbf{u} + \mathbf{v}) = c\mathbf{u} + c\mathbf{v}$
8. $1\mathbf{v} = \mathbf{v}$

### Dot Product (Inner Product)
The **dot product** of vectors $\mathbf{u}, \mathbf{v} \in \mathbb{R}^n$ is:
$$\mathbf{u} \cdot \mathbf{v} = u_1v_1 + u_2v_2 + \cdots + u_nv_n = \sum_{i=1}^n u_iv_i$$

**Properties:**
1. $\mathbf{u} \cdot \mathbf{v} = \mathbf{v} \cdot \mathbf{u}$ (commutative)
2. $\mathbf{u} \cdot (\mathbf{v} + \mathbf{w}) = \mathbf{u} \cdot \mathbf{v} + \mathbf{u} \cdot \mathbf{w}$ (distributive)
3. $(c\mathbf{u}) \cdot \mathbf{v} = c(\mathbf{u} \cdot \mathbf{v})$
4. $\mathbf{u} \cdot \mathbf{u} \geq 0$, with equality iff $\mathbf{u} = \mathbf{0}$

### Vector Norm (Length)
The **norm** (or **length**) of a vector $\mathbf{v}$ is:
$$\|\mathbf{v}\| = \sqrt{\mathbf{v} \cdot \mathbf{v}} = \sqrt{v_1^2 + v_2^2 + \cdots + v_n^2}$$

**Properties:**
1. $\|\mathbf{v}\| \geq 0$, with equality iff $\mathbf{v} = \mathbf{0}$
2. $\|c\mathbf{v}\| = |c|\|\mathbf{v}\|$
3. **Triangle inequality:** $\|\mathbf{u} + \mathbf{v}\| \leq \|\mathbf{u}\| + \|\mathbf{v}\|$

**Unit vector:** A vector with norm 1. Any nonzero vector can be normalized:
$$\mathbf{u} = \frac{\mathbf{v}}{\|\mathbf{v}\|}$$
### Distance
The **distance** between vectors $\mathbf{u}$ and $\mathbf{v}$ is:
$$d(\mathbf{u}, \mathbf{v}) = \|\mathbf{u} - \mathbf{v}\|$$
### Angle Between Vectors
For nonzero vectors $\mathbf{u}, \mathbf{v} \in \mathbb{R}^n$, the angle $\theta$ between them satisfies:
$$\cos\theta = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\|\|\mathbf{v}\|}$$
**Orthogonal vectors:** $\mathbf{u}$ and $\mathbf{v}$ are **orthogonal** (perpendicular) if $\mathbf{u} \cdot \mathbf{v} = 0$.

## Linear Combinations and Spanning
### Linear Combination
A **linear combination** of vectors $\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k$ is a vector of the form:
$$c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_k\mathbf{v}_k$$
where $c_1, c_2, \ldots, c_k$ are scalars.
### Span
The **span** of a set of vectors $\{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k\}$ is the set of all linear combinations:
$$\text{span}\{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k\} = \{c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_k\mathbf{v}_k : c_i \in \mathbb{R}\}$$
**Interpretation:** The span is the set of all vectors that can be "reached" using the given vectors.
### Examples
1. $\text{span}\left\{\begin{bmatrix}1\\0\end{bmatrix}\right\}$ is the $x$-axis in $\mathbb{R}^2$
2. $\text{span}\left\{\begin{bmatrix}1\\0\end{bmatrix}, \begin{bmatrix}0\\1\end{bmatrix}\right\} = \mathbb{R}^2$ (all of the plane)
3. $\text{span}\left\{\begin{bmatrix}1\\0\\0\end{bmatrix}, \begin{bmatrix}0\\1\\0\end{bmatrix}\right\}$ is the $xy$-plane in $\mathbb{R}^3$

## Linear Independence
### Definition
Vectors $\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k$ are **linearly independent** if the only solution to:
$$c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_k\mathbf{v}_k = \mathbf{0}$$
is $c_1 = c_2 = \cdots = c_k = 0$ (the **trivial solution**).
If there exists a nontrivial solution (at least one $c_i \neq 0$), the vectors are **linearly dependent**.
### Geometric Interpretation
- Two vectors are linearly dependent iff one is a scalar multiple of the other (they lie on the same line)
- Three vectors in $\mathbb{R}^3$ are linearly dependent iff they lie in the same plane

### Testing for Linear Independence
To determine if vectors are linearly independent:
1. Form a matrix with the vectors as columns: $A = [\mathbf{v}_1 \mid \mathbf{v}_2 \mid \cdots \mid \mathbf{v}_k]$
2. Row reduce to echelon form
3. The vectors are linearly independent iff each column contains a pivot

### Properties
1. Any set containing the zero vector is linearly dependent
2. A set of one vector $\{\mathbf{v}\}$ is linearly independent iff $\mathbf{v} \neq \mathbf{0}$
3. If a set is linearly independent, any subset is also linearly independent
4. If a set is linearly dependent, any superset is also linearly dependent
5. More than $n$ vectors in $\mathbb{R}^n$ must be linearly dependent

## Basis and Dimension
### Basis
A **basis** for a subspace $S$ of $\mathbb{R}^n$ is a set of vectors $\{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k\}$ such that:
1. The vectors are **linearly independent**
2. The vectors **span** $S$: $\text{span}\{\mathbf{v}_1, \ldots, \mathbf{v}_k\} = S$

### Standard Basis for $\mathbb{R}^n$
The **standard basis** for $\mathbb{R}^n$ consists of the vectors:
$$\mathbf{e}_1 = \begin{bmatrix}1\\0\\\vdots\\0\end{bmatrix}, \mathbf{e}_2 = \begin{bmatrix}0\\1\\\vdots\\0\end{bmatrix}, \ldots, \mathbf{e}_n = \begin{bmatrix}0\\0\\\vdots\\1\end{bmatrix}$$
### Dimension
The **dimension** of a subspace $S$ is the number of vectors in any basis for $S$.
**Theorem:** All bases for a given subspace have the same number of vectors.

**Notation:** $\dim(S)$
### Examples
- $\dim(\mathbb{R}^n) = n$
- The dimension of a line through the origin in $\mathbb{R}^3$ is 1
- The dimension of a plane through the origin in $\mathbb{R}^3$ is 2
- $\dim(\{\mathbf{0}\}) = 0$

### Finding a Basis
To find a basis for the span of vectors $\{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k\}$:
1. Form the matrix $A = [\mathbf{v}_1 \mid \mathbf{v}_2 \mid \cdots \mid \mathbf{v}_k]$
2. Row reduce to echelon form
3. The columns of $A$ corresponding to pivot columns form a basis

### Coordinate Vector
If $\mathcal{B} = \{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_n\}$ is a basis for $\mathbb{R}^n$, then every vector $\mathbf{x} \in \mathbb{R}^n$ can be uniquely written as:
$$\mathbf{x} = c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_n\mathbf{v}_n$$

The **coordinate vector** of $\mathbf{x}$ relative to $\mathcal{B}$ is:
$$[\mathbf{x}]_{\mathcal{B}} = \begin{bmatrix}c_1\\c_2\\\vdots\\c_n\end{bmatrix}$$