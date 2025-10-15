---
tags:
  - type/lecture
  - course/csci2033
  - subject/Csci
course: CSCI 2033
topic(s):  Inner Product Spaces, Inequalities and Orthogonality, Orthonormal Bases, Orthogonal Matrices
---
# Inner Product Spaces

## Inner Product Spaces

### Definition
An **inner product** on a vector space $V$ is a function $\langle \cdot, \cdot \rangle: V \times V \to \mathbb{R}$ that satisfies:

1. **Symmetry**: $\langle \mathbf{u}, \mathbf{v} \rangle = \langle \mathbf{v}, \mathbf{u} \rangle$
2. **Linearity in first argument**:
   - $\langle c\mathbf{u}, \mathbf{v} \rangle = c\langle \mathbf{u}, \mathbf{v} \rangle$
   - $\langle \mathbf{u} + \mathbf{w}, \mathbf{v} \rangle = \langle \mathbf{u}, \mathbf{v} \rangle + \langle \mathbf{w}, \mathbf{v} \rangle$
3. **Positive definiteness**:
   - $\langle \mathbf{v}, \mathbf{v} \rangle \geq 0$
   - $\langle \mathbf{v}, \mathbf{v} \rangle = 0$ iff $\mathbf{v} = \mathbf{0}$

A vector space with an inner product is called an **inner product space**.

### Standard Inner Product on $\mathbb{R}^n$
The **dot product** (or **Euclidean inner product**) is:
$$\langle \mathbf{u}, \mathbf{v} \rangle = \mathbf{u} \cdot \mathbf{v} = u_1v_1 + u_2v_2 + \cdots + u_nv_n$$

### Other Examples of Inner Products

#### Weighted Inner Product on $\mathbb{R}^n$
$$\langle \mathbf{u}, \mathbf{v} \rangle = w_1u_1v_1 + w_2u_2v_2 + \cdots + w_nu_nv_n$$
where $w_i > 0$ are weights.

#### Inner Product on $\mathbb{P}_n$
For polynomials $p(x)$ and $q(x)$:
$$\langle p, q \rangle = \int_a^b p(x)q(x)\,dx$$

#### Inner Product on $\mathbb{M}_{m \times n}$
For matrices $A$ and $B$:
$$\langle A, B \rangle = \text{tr}(A^TB) = \sum_{i,j} a_{ij}b_{ij}$$
(sum of products of corresponding entries)

### Norm Induced by Inner Product
The **norm** (or **length**) induced by an inner product is:
$$\|\mathbf{v}\| = \sqrt{\langle \mathbf{v}, \mathbf{v} \rangle}$$

**Properties:**
1. $\|\mathbf{v}\| \geq 0$, with equality iff $\mathbf{v} = \mathbf{0}$
2. $\|c\mathbf{v}\| = |c|\|\mathbf{v}\|$
3. $\|\mathbf{u} + \mathbf{v}\| \leq \|\mathbf{u}\| + \|\mathbf{v}\|$ (triangle inequality)

### Distance
The **distance** between vectors is:
$$d(\mathbf{u}, \mathbf{v}) = \|\mathbf{u} - \mathbf{v}\| = \sqrt{\langle \mathbf{u} - \mathbf{v}, \mathbf{u} - \mathbf{v} \rangle}$$

## Inequalities and Orthogonality

### Cauchy-Schwarz Inequality
For any vectors $\mathbf{u}, \mathbf{v}$ in an inner product space:
$$|\langle \mathbf{u}, \mathbf{v} \rangle| \leq \|\mathbf{u}\|\|\mathbf{v}\|$$

Equality holds iff $\mathbf{u}$ and $\mathbf{v}$ are linearly dependent (one is a scalar multiple of the other).

**In $\mathbb{R}^n$:**
$$|\mathbf{u} \cdot \mathbf{v}| \leq \|\mathbf{u}\|\|\mathbf{v}\|$$

### Triangle Inequality
For any vectors $\mathbf{u}, \mathbf{v}$:
$$\|\mathbf{u} + \mathbf{v}\| \leq \|\mathbf{u}\| + \|\mathbf{v}\|$$

### Orthogonality

**Definition:** Vectors $\mathbf{u}$ and $\mathbf{v}$ are **orthogonal** (denoted $\mathbf{u} \perp \mathbf{v}$) if:
$$\langle \mathbf{u}, \mathbf{v} \rangle = 0$$

**Properties:**
1. The zero vector is orthogonal to every vector
2. If $\mathbf{u} \perp \mathbf{v}$, then $\mathbf{v} \perp \mathbf{u}$ (by symmetry)

### Pythagorean Theorem
If $\mathbf{u} \perp \mathbf{v}$, then:
$$\|\mathbf{u} + \mathbf{v}\|^2 = \|\mathbf{u}\|^2 + \|\mathbf{v}\|^2$$

### Orthogonal Complement
For a subspace $W$ of $V$, the **orthogonal complement** is:
$$W^\perp = \{\mathbf{v} \in V : \langle \mathbf{v}, \mathbf{w} \rangle = 0 \text{ for all } \mathbf{w} \in W\}$$

**Properties:**
1. $W^\perp$ is a subspace
2. $W \cap W^\perp = \{\mathbf{0}\}$
3. $(W^\perp)^\perp = W$

### Orthogonal Projection
The **orthogonal projection** of $\mathbf{v}$ onto a nonzero vector $\mathbf{u}$ is:
$$\text{proj}_{\mathbf{u}}\mathbf{v} = \frac{\langle \mathbf{v}, \mathbf{u} \rangle}{\langle \mathbf{u}, \mathbf{u} \rangle}\mathbf{u} = \frac{\mathbf{v} \cdot \mathbf{u}}{\mathbf{u} \cdot \mathbf{u}}\mathbf{u}$$

The **component orthogonal** to $\mathbf{u}$ is:
$$\mathbf{v} - \text{proj}_{\mathbf{u}}\mathbf{v}$$

This satisfies: $(\mathbf{v} - \text{proj}_{\mathbf{u}}\mathbf{v}) \perp \mathbf{u}$

## Orthogonal and Orthonormal Sets

### Orthogonal Set
A set of vectors $\{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k\}$ is **orthogonal** if:
$$\langle \mathbf{v}_i, \mathbf{v}_j \rangle = 0 \quad \text{for all } i \neq j$$

**Theorem:** Any orthogonal set of nonzero vectors is linearly independent.

### Orthonormal Set
A set of vectors is **orthonormal** if it is orthogonal and each vector has norm 1:
$$\langle \mathbf{v}_i, \mathbf{v}_j \rangle = \begin{cases} 1 & \text{if } i = j \\ 0 & \text{if } i \neq j \end{cases}$$

This can be written compactly as: $\langle \mathbf{v}_i, \mathbf{v}_j \rangle = \delta_{ij}$ (Kronecker delta).

### Orthonormal Basis
An **orthonormal basis** for a vector space $V$ is a basis that is orthonormal.

**Standard basis for $\mathbb{R}^n$:** $\{\mathbf{e}_1, \mathbf{e}_2, \ldots, \mathbf{e}_n\}$ is orthonormal.

### Coordinates Relative to Orthonormal Basis
If $\mathcal{B} = \{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_n\}$ is an orthonormal basis for $V$, then any $\mathbf{x} \in V$ can be written as:
$$\mathbf{x} = \langle \mathbf{x}, \mathbf{v}_1 \rangle \mathbf{v}_1 + \langle \mathbf{x}, \mathbf{v}_2 \rangle \mathbf{v}_2 + \cdots + \langle \mathbf{x}, \mathbf{v}_n \rangle \mathbf{v}_n$$

The coefficients are simply the inner products! (This is much easier than solving a system of equations.)

## Gram-Schmidt Process

The **Gram-Schmidt process** converts a basis into an orthogonal (or orthonormal) basis.

### Algorithm
Given a basis $\{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_n\}$, construct an orthogonal basis $\{\mathbf{u}_1, \mathbf{u}_2, \ldots, \mathbf{u}_n\}$:

1. $\mathbf{u}_1 = \mathbf{v}_1$

2. $\mathbf{u}_2 = \mathbf{v}_2 - \frac{\langle \mathbf{v}_2, \mathbf{u}_1 \rangle}{\langle \mathbf{u}_1, \mathbf{u}_1 \rangle}\mathbf{u}_1$

3. $\mathbf{u}_3 = \mathbf{v}_3 - \frac{\langle \mathbf{v}_3, \mathbf{u}_1 \rangle}{\langle \mathbf{u}_1, \mathbf{u}_1 \rangle}\mathbf{u}_1 - \frac{\langle \mathbf{v}_3, \mathbf{u}_2 \rangle}{\langle \mathbf{u}_2, \mathbf{u}_2 \rangle}\mathbf{u}_2$

In general:
$$\mathbf{u}_k = \mathbf{v}_k - \sum_{i=1}^{k-1} \frac{\langle \mathbf{v}_k, \mathbf{u}_i \rangle}{\langle \mathbf{u}_i, \mathbf{u}_i \rangle}\mathbf{u}_i$$

To obtain an **orthonormal** basis, normalize each vector:
$$\mathbf{w}_i = \frac{\mathbf{u}_i}{\|\mathbf{u}_i\|}$$

### Example
Orthogonalize $\left\{\begin{bmatrix}1\\1\\0\end{bmatrix}, \begin{bmatrix}1\\0\\1\end{bmatrix}\right\}$

**Step 1:** $\mathbf{u}_1 = \begin{bmatrix}1\\1\\0\end{bmatrix}$

**Step 2:**
$$\mathbf{u}_2 = \begin{bmatrix}1\\0\\1\end{bmatrix} - \frac{1}{2}\begin{bmatrix}1\\1\\0\end{bmatrix} = \begin{bmatrix}1/2\\-1/2\\1\end{bmatrix}$$

For orthonormal basis, normalize:
$$\mathbf{w}_1 = \frac{1}{\sqrt{2}}\begin{bmatrix}1\\1\\0\end{bmatrix}, \quad \mathbf{w}_2 = \frac{1}{\sqrt{3/2}}\begin{bmatrix}1/2\\-1/2\\1\end{bmatrix}$$

## Orthogonal Matrices

### Definition
A square matrix $Q$ is **orthogonal** if:
$$Q^TQ = QQ^T = I$$

Equivalently, $Q^T = Q^{-1}$.

### Properties of Orthogonal Matrices
1. The columns of $Q$ form an orthonormal basis for $\mathbb{R}^n$
2. The rows of $Q$ form an orthonormal basis for $\mathbb{R}^n$
3. $\det(Q) = \pm 1$
4. $Q$ preserves lengths: $\|Q\mathbf{x}\| = \|\mathbf{x}\|$ for all $\mathbf{x}$
5. $Q$ preserves inner products: $\langle Q\mathbf{x}, Q\mathbf{y} \rangle = \langle \mathbf{x}, \mathbf{y} \rangle$
6. $Q$ preserves angles between vectors
7. If $Q_1$ and $Q_2$ are orthogonal, then $Q_1Q_2$ is orthogonal

### Examples of Orthogonal Matrices

**Identity matrix:**
$$I = \begin{bmatrix}1&0\\0&1\end{bmatrix}$$

**Rotation matrix:**
$$R(\theta) = \begin{bmatrix}\cos\theta & -\sin\theta \\ \sin\theta & \cos\theta\end{bmatrix}$$

**Reflection matrix:**
$$F = \begin{bmatrix}1&0\\0&-1\end{bmatrix}$$

**Permutation matrices** (reorder rows/columns)

### QR Factorization
Any $m \times n$ matrix $A$ with linearly independent columns can be factored as:
$$A = QR$$
where:
- $Q$ is an $m \times n$ matrix with orthonormal columns
- $R$ is an $n \times n$ upper triangular matrix with positive diagonal entries

**Construction:** Apply Gram-Schmidt to the columns of $A$ to get $Q$, then $R = Q^TA$.

**Application:** Solving $A\mathbf{x} = \mathbf{b}$ becomes $QR\mathbf{x} = \mathbf{b}$, so $R\mathbf{x} = Q^T\mathbf{b}$ (easier to solve).

## Best Approximation Theorem

### Orthogonal Projection onto a Subspace
Let $W$ be a subspace of $V$ with orthogonal basis $\{\mathbf{u}_1, \mathbf{u}_2, \ldots, \mathbf{u}_k\}$.

The **orthogonal projection** of $\mathbf{v}$ onto $W$ is:
$$\text{proj}_W\mathbf{v} = \sum_{i=1}^k \frac{\langle \mathbf{v}, \mathbf{u}_i \rangle}{\langle \mathbf{u}_i, \mathbf{u}_i \rangle}\mathbf{u}_i$$

If the basis is orthonormal:
$$\text{proj}_W\mathbf{v} = \sum_{i=1}^k \langle \mathbf{v}, \mathbf{u}_i \rangle \mathbf{u}_i$$

### Best Approximation Theorem
The vector in $W$ closest to $\mathbf{v}$ is $\text{proj}_W\mathbf{v}$.

That is, $\text{proj}_W\mathbf{v}$ minimizes $\|\mathbf{v} - \mathbf{w}\|$ over all $\mathbf{w} \in W$.

### Least Squares Solution
For an inconsistent system $A\mathbf{x} = \mathbf{b}$ (no solution), the **least squares solution** is the $\mathbf{x}$ that minimizes $\|A\mathbf{x} - \mathbf{b}\|$.

**Solution:** $\mathbf{x} = (A^TA)^{-1}A^T\mathbf{b}$ (if $A$ has linearly independent columns)

Equivalently, solve the **normal equations**: $A^TA\mathbf{x} = A^T\mathbf{b}$
