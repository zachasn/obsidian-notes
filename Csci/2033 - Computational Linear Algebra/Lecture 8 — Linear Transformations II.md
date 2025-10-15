---
tags:
  - type/lecture
  - course/csci2033
  - subject/Csci
course: CSCI 2033
topic(s): Inverse Linear Transformations, Matrix of a Linear Transformation, Composition and Inverse Linear Transformations
---
# Linear Transformations II

## Matrix of a Linear Transformation

### Representation Theorem
Every linear transformation $T: \mathbb{R}^n \to \mathbb{R}^m$ can be represented as:
$$T(\mathbf{x}) = A\mathbf{x}$$
for a unique $m \times n$ matrix $A$.

### Finding the Standard Matrix
To find the matrix $A$ representing $T: \mathbb{R}^n \to \mathbb{R}^m$:

The columns of $A$ are the images of the standard basis vectors:
$$A = [T(\mathbf{e}_1) \mid T(\mathbf{e}_2) \mid \cdots \mid T(\mathbf{e}_n)]$$

where $\mathbf{e}_1, \ldots, \mathbf{e}_n$ are the standard basis vectors for $\mathbb{R}^n$.

### Example
Find the matrix of $T: \mathbb{R}^2 \to \mathbb{R}^3$ where:
$$T\left(\begin{bmatrix}x_1\\x_2\end{bmatrix}\right) = \begin{bmatrix}2x_1 + x_2\\x_1 - x_2\\3x_2\end{bmatrix}$$

**Solution:**
$$T(\mathbf{e}_1) = T\left(\begin{bmatrix}1\\0\end{bmatrix}\right) = \begin{bmatrix}2\\1\\0\end{bmatrix}$$
$$T(\mathbf{e}_2) = T\left(\begin{bmatrix}0\\1\end{bmatrix}\right) = \begin{bmatrix}1\\-1\\3\end{bmatrix}$$

Therefore:
$$A = \begin{bmatrix}2 & 1 \\ 1 & -1 \\ 0 & 3\end{bmatrix}$$

### Matrix Relative to General Bases
For $T: V \to W$ where $V$ and $W$ have bases $\mathcal{B} = \{\mathbf{v}_1, \ldots, \mathbf{v}_n\}$ and $\mathcal{C} = \{\mathbf{w}_1, \ldots, \mathbf{w}_m\}$:

The **matrix of $T$ relative to $\mathcal{B}$ and $\mathcal{C}$**, denoted $[T]_{\mathcal{B}}^{\mathcal{C}}$, has columns:
$$[T]_{\mathcal{B}}^{\mathcal{C}} = [[T(\mathbf{v}_1)]_{\mathcal{C}} \mid [T(\mathbf{v}_2)]_{\mathcal{C}} \mid \cdots \mid [T(\mathbf{v}_n)]_{\mathcal{C}}]$$

where $[T(\mathbf{v}_i)]_{\mathcal{C}}$ is the coordinate vector of $T(\mathbf{v}_i)$ relative to basis $\mathcal{C}$.

### Coordinate Transformation
If $[\mathbf{x}]_{\mathcal{B}}$ is the coordinate vector of $\mathbf{x}$ relative to $\mathcal{B}$, then:
$$[T(\mathbf{x})]_{\mathcal{C}} = [T]_{\mathcal{B}}^{\mathcal{C}} [\mathbf{x}]_{\mathcal{B}}$$

## Inverse Linear Transformations

### Definition
A linear transformation $T: V \to W$ is **invertible** if there exists a linear transformation $S: W \to V$ such that:
$$S \circ T = I_V \quad \text{and} \quad T \circ S = I_W$$

The transformation $S$ is called the **inverse** of $T$, denoted $T^{-1}$.

### Invertibility Conditions
A linear transformation $T: V \to W$ is invertible iff:
1. $T$ is **bijective** (both injective and surjective)
2. $T$ is **one-to-one and onto**
3. $\ker(T) = \{\mathbf{0}\}$ and $\text{range}(T) = W$

### Theorem: Invertibility of Square Matrices
For a linear transformation $T: \mathbb{R}^n \to \mathbb{R}^n$ with matrix $A$, the following are equivalent:
1. $T$ is invertible
2. $A$ is invertible
3. $\det(A) \neq 0$
4. $\text{rank}(A) = n$
5. $\text{nullity}(A) = 0$
6. The columns of $A$ are linearly independent
7. The columns of $A$ span $\mathbb{R}^n$
8. $A$ can be row reduced to $I_n$

### Properties of Inverse Transformations
1. If $T$ is invertible, then $T^{-1}$ is unique
2. $(T^{-1})^{-1} = T$
3. If $T: V \to W$ is invertible with matrix $A$, then $T^{-1}$ has matrix $A^{-1}$
4. If $T$ and $S$ are invertible, then $(S \circ T)^{-1} = T^{-1} \circ S^{-1}$

### Finding the Inverse Transformation
To find $T^{-1}$ when $T(\mathbf{x}) = A\mathbf{x}$:
1. Compute $A^{-1}$ using Gauss-Jordan elimination
2. Then $T^{-1}(\mathbf{y}) = A^{-1}\mathbf{y}$

### Example
Let $T: \mathbb{R}^2 \to \mathbb{R}^2$ be defined by:
$$T\left(\begin{bmatrix}x_1\\x_2\end{bmatrix}\right) = \begin{bmatrix}2x_1 + x_2\\x_1 + x_2\end{bmatrix}$$

**Find $T^{-1}$:**

Matrix: $A = \begin{bmatrix}2&1\\1&1\end{bmatrix}$

Inverse: $A^{-1} = \begin{bmatrix}1&-1\\-1&2\end{bmatrix}$

Therefore:
$$T^{-1}\left(\begin{bmatrix}y_1\\y_2\end{bmatrix}\right) = \begin{bmatrix}1&-1\\-1&2\end{bmatrix}\begin{bmatrix}y_1\\y_2\end{bmatrix} = \begin{bmatrix}y_1 - y_2\\-y_1 + 2y_2\end{bmatrix}$$

## Composition and Inverse Linear Transformations

### Composition
If $T: U \to V$ and $S: V \to W$ are linear transformations, their composition is:
$$(S \circ T): U \to W$$

**Matrix representation:** If $T$ has matrix $A$ and $S$ has matrix $B$, then $S \circ T$ has matrix $BA$.

Note the order: composition $S \circ T$ corresponds to matrix product $BA$ (reversed order).

### Properties of Composition
1. **Associativity**: $(R \circ S) \circ T = R \circ (S \circ T)$
2. **Not commutative**: Generally $S \circ T \neq T \circ S$
3. **Identity**: $T \circ I = I \circ T = T$
4. If $T$ and $S$ are linear, then $S \circ T$ is linear
5. $\text{range}(S \circ T) \subseteq \text{range}(S)$
6. $\ker(T) \subseteq \ker(S \circ T)$

### Composition and Invertibility
**Theorem:** If $T: U \to V$ and $S: V \to W$ are both invertible, then $S \circ T: U \to W$ is invertible with:
$$(S \circ T)^{-1} = T^{-1} \circ S^{-1}$$

**Matrix form:** $(BA)^{-1} = A^{-1}B^{-1}$

### Example: Rotation and Reflection
Let $R: \mathbb{R}^2 \to \mathbb{R}^2$ be rotation by $90°$:
$$R(\mathbf{x}) = \begin{bmatrix}0&-1\\1&0\end{bmatrix}\mathbf{x}$$

Let $F: \mathbb{R}^2 \to \mathbb{R}^2$ be reflection about $x$-axis:
$$F(\mathbf{x}) = \begin{bmatrix}1&0\\0&-1\end{bmatrix}\mathbf{x}$$

**Composition $F \circ R$:**
$$(F \circ R)(\mathbf{x}) = \begin{bmatrix}1&0\\0&-1\end{bmatrix}\begin{bmatrix}0&-1\\1&0\end{bmatrix}\mathbf{x} = \begin{bmatrix}0&-1\\-1&0\end{bmatrix}\mathbf{x}$$

This represents reflection about the line $y = -x$.

## Isomorphisms

### Definition
A linear transformation $T: V \to W$ is an **isomorphism** if it is bijective (both injective and surjective).

Two vector spaces $V$ and $W$ are **isomorphic** (denoted $V \cong W$) if there exists an isomorphism between them.

### Properties of Isomorphisms
1. Isomorphisms preserve all vector space structure
2. If $T: V \to W$ is an isomorphism, then $\dim(V) = \dim(W)$
3. Isomorphic spaces are "the same" from a linear algebra perspective
4. If $T$ is an isomorphism, then $T^{-1}$ is also an isomorphism

### Fundamental Theorem
**Theorem:** Any $n$-dimensional vector space $V$ is isomorphic to $\mathbb{R}^n$.

Specifically, if $\mathcal{B} = \{\mathbf{v}_1, \ldots, \mathbf{v}_n\}$ is a basis for $V$, the coordinate mapping:
$$T: V \to \mathbb{R}^n, \quad T(\mathbf{v}) = [\mathbf{v}]_{\mathcal{B}}$$
is an isomorphism.

### Examples

#### $\mathbb{P}_n \cong \mathbb{R}^{n+1}$
The map $T: \mathbb{P}_n \to \mathbb{R}^{n+1}$ defined by:
$$T(a_0 + a_1x + \cdots + a_nx^n) = \begin{bmatrix}a_0\\a_1\\\vdots\\a_n\end{bmatrix}$$
is an isomorphism.

#### $\mathbb{M}_{m \times n} \cong \mathbb{R}^{mn}$
Any $m \times n$ matrix space is isomorphic to $\mathbb{R}^{mn}$.

### Classification Theorem
**Theorem:** Two finite-dimensional vector spaces are isomorphic iff they have the same dimension.

## Change of Basis

### Motivation
The same vector $\mathbf{v}$ has different coordinate representations in different bases.

### Change of Basis Matrix
Let $\mathcal{B} = \{\mathbf{v}_1, \ldots, \mathbf{v}_n\}$ and $\mathcal{C} = \{\mathbf{w}_1, \ldots, \mathbf{w}_n\}$ be bases for $V$.

The **change of basis matrix** from $\mathcal{B}$ to $\mathcal{C}$ is:
$$P_{\mathcal{B} \to \mathcal{C}} = [[\mathbf{v}_1]_{\mathcal{C}} \mid [\mathbf{v}_2]_{\mathcal{C}} \mid \cdots \mid [\mathbf{v}_n]_{\mathcal{C}}]$$

This matrix satisfies:
$$[\mathbf{x}]_{\mathcal{C}} = P_{\mathcal{B} \to \mathcal{C}} [\mathbf{x}]_{\mathcal{B}}$$

### Properties
1. $P_{\mathcal{B} \to \mathcal{C}}$ is invertible
2. $P_{\mathcal{C} \to \mathcal{B}} = (P_{\mathcal{B} \to \mathcal{C}})^{-1}$
3. $P_{\mathcal{B} \to \mathcal{B}} = I$
4. $P_{\mathcal{A} \to \mathcal{C}} = P_{\mathcal{B} \to \mathcal{C}} P_{\mathcal{A} \to \mathcal{B}}$

### Change of Basis for Standard Basis
If $\mathcal{E}$ is the standard basis and $\mathcal{B} = \{\mathbf{v}_1, \ldots, \mathbf{v}_n\}$:
$$P_{\mathcal{B} \to \mathcal{E}} = [\mathbf{v}_1 \mid \mathbf{v}_2 \mid \cdots \mid \mathbf{v}_n]$$

And:
$$P_{\mathcal{E} \to \mathcal{B}} = [\mathbf{v}_1 \mid \mathbf{v}_2 \mid \cdots \mid \mathbf{v}_n]^{-1}$$

### Similarity of Matrices
Two matrices $A$ and $B$ are **similar** if there exists an invertible matrix $P$ such that:
$$B = P^{-1}AP$$

**Interpretation:** $A$ and $B$ represent the same linear transformation in different bases.

**Properties of similar matrices:**
1. $\det(A) = \det(B)$
2. $\text{rank}(A) = \text{rank}(B)$
3. $\text{tr}(A) = \text{tr}(B)$ (same trace)
4. $A$ and $B$ have the same eigenvalues
