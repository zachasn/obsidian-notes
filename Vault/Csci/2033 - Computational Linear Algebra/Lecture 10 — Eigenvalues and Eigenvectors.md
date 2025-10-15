---
tags:
  - type/lecture
  - course/csci2033
  - subject/Csci
course: CSCI 2033
topic(s): Eigenvalues and Eigenvectors, Properties of Eigenvalues and Eigenvectors, Diagonalization, Singular Value Decomposition
---
# Eigenvalues and Eigenvectors

## Eigenvalues and Eigenvectors

### Definition
Let $A$ be an $n \times n$ matrix. A nonzero vector $\mathbf{v}$ is an **eigenvector** of $A$ if:
$$A\mathbf{v} = \lambda\mathbf{v}$$
for some scalar $\lambda$.

The scalar $\lambda$ is called an **eigenvalue** of $A$, and $\mathbf{v}$ is an eigenvector corresponding to $\lambda$.

**Interpretation:** When $A$ acts on $\mathbf{v}$, it only scales $\mathbf{v}$ by $\lambda$ (no change in direction).

### Characteristic Equation
$\lambda$ is an eigenvalue of $A$ iff:
$$\det(A - \lambda I) = 0$$

This is called the **characteristic equation**.

The polynomial $p(\lambda) = \det(A - \lambda I)$ is called the **characteristic polynomial**.

### Finding Eigenvalues and Eigenvectors
**Step 1:** Find eigenvalues
- Solve $\det(A - \lambda I) = 0$ for $\lambda$

**Step 2:** Find eigenvectors for each eigenvalue $\lambda$
- Solve $(A - \lambda I)\mathbf{v} = \mathbf{0}$
- The nonzero solutions are eigenvectors

### Example
Find eigenvalues and eigenvectors of $A = \begin{bmatrix}3&1\\1&3\end{bmatrix}$

**Eigenvalues:**
$$\det(A - \lambda I) = \det\begin{bmatrix}3-\lambda&1\\1&3-\lambda\end{bmatrix} = (3-\lambda)^2 - 1 = \lambda^2 - 6\lambda + 8$$

$$= (\lambda - 4)(\lambda - 2) = 0$$

Eigenvalues: $\lambda_1 = 4$, $\lambda_2 = 2$

**Eigenvector for $\lambda_1 = 4$:**
$$(A - 4I)\mathbf{v} = \begin{bmatrix}-1&1\\1&-1\end{bmatrix}\mathbf{v} = \mathbf{0}$$

Solution: $\mathbf{v}_1 = \begin{bmatrix}1\\1\end{bmatrix}$ (and any scalar multiple)

**Eigenvector for $\lambda_2 = 2$:**
$$(A - 2I)\mathbf{v} = \begin{bmatrix}1&1\\1&1\end{bmatrix}\mathbf{v} = \mathbf{0}$$

Solution: $\mathbf{v}_2 = \begin{bmatrix}1\\-1\end{bmatrix}$ (and any scalar multiple)

### Eigenspace
For an eigenvalue $\lambda$, the **eigenspace** $E_\lambda$ is:
$$E_\lambda = \text{Null}(A - \lambda I) = \{\mathbf{v} : A\mathbf{v} = \lambda\mathbf{v}\}$$

This is a subspace (contains all eigenvectors for $\lambda$ plus the zero vector).

## Properties of Eigenvalues and Eigenvectors

### Basic Properties
1. **Triangular matrices**: The eigenvalues are the diagonal entries
2. **Zero is an eigenvalue** iff $A$ is singular (not invertible)
3. If $\mathbf{v}$ is an eigenvector, so is any nonzero scalar multiple $c\mathbf{v}$
4. Eigenvectors corresponding to different eigenvalues are linearly independent

### Eigenvalues of Special Matrices

**Inverse:** If $A$ is invertible with eigenvalue $\lambda$ and eigenvector $\mathbf{v}$, then $A^{-1}$ has eigenvalue $\frac{1}{\lambda}$ with the same eigenvector $\mathbf{v}$.

**Transpose:** $A$ and $A^T$ have the same eigenvalues (but possibly different eigenvectors)

**Powers:** If $\lambda$ is an eigenvalue of $A$ with eigenvector $\mathbf{v}$, then $\lambda^k$ is an eigenvalue of $A^k$ with the same eigenvector $\mathbf{v}$.

**Scalar multiple:** If $\lambda$ is an eigenvalue of $A$, then $c\lambda$ is an eigenvalue of $cA$

**Shift:** If $\lambda$ is an eigenvalue of $A$, then $\lambda + c$ is an eigenvalue of $A + cI$

### Trace and Determinant
For an $n \times n$ matrix $A$ with eigenvalues $\lambda_1, \lambda_2, \ldots, \lambda_n$ (counting multiplicities):
$$\text{tr}(A) = \sum_{i=1}^n \lambda_i$$
$$\det(A) = \prod_{i=1}^n \lambda_i$$

### Algebraic and Geometric Multiplicity
For an eigenvalue $\lambda$:

**Algebraic multiplicity:** The multiplicity of $\lambda$ as a root of the characteristic polynomial

**Geometric multiplicity:** $\dim(E_\lambda)$ = number of linearly independent eigenvectors for $\lambda$

**Theorem:** $1 \leq \text{geometric multiplicity} \leq \text{algebraic multiplicity}$

### Linear Independence
**Theorem:** Eigenvectors corresponding to distinct eigenvalues are linearly independent.

More generally, if we take one eigenvector from each distinct eigenspace, the collection is linearly independent.

## Diagonalization

### Definition
A matrix $A$ is **diagonalizable** if there exists an invertible matrix $P$ and a diagonal matrix $D$ such that:
$$A = PDP^{-1}$$

Equivalently: $P^{-1}AP = D$

**Interpretation:** $A$ is similar to a diagonal matrix.

### Diagonalization Theorem
An $n \times n$ matrix $A$ is diagonalizable iff $A$ has $n$ linearly independent eigenvectors.

**Construction:** If $A$ has eigenvalues $\lambda_1, \ldots, \lambda_n$ with corresponding eigenvectors $\mathbf{v}_1, \ldots, \mathbf{v}_n$, then:
$$P = [\mathbf{v}_1 \mid \mathbf{v}_2 \mid \cdots \mid \mathbf{v}_n]$$
$$D = \begin{bmatrix}\lambda_1 & 0 & \cdots & 0 \\ 0 & \lambda_2 & \cdots & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \cdots & \lambda_n\end{bmatrix}$$

### Conditions for Diagonalizability
An $n \times n$ matrix is diagonalizable if:
1. It has $n$ distinct eigenvalues, OR
2. For each eigenvalue, the geometric multiplicity equals the algebraic multiplicity

### Steps to Diagonalize
1. Find eigenvalues of $A$
2. Find eigenvectors for each eigenvalue
3. Form $P$ from eigenvectors as columns
4. Form $D$ with eigenvalues on diagonal (in corresponding order)
5. Verify: $A = PDP^{-1}$ or $P^{-1}AP = D$

### Example
Diagonalize $A = \begin{bmatrix}3&1\\1&3\end{bmatrix}$

From earlier: $\lambda_1 = 4$, $\mathbf{v}_1 = \begin{bmatrix}1\\1\end{bmatrix}$; $\lambda_2 = 2$, $\mathbf{v}_2 = \begin{bmatrix}1\\-1\end{bmatrix}$

$$P = \begin{bmatrix}1&1\\1&-1\end{bmatrix}, \quad D = \begin{bmatrix}4&0\\0&2\end{bmatrix}$$

Verify: $A = PDP^{-1}$

### Applications of Diagonalization

#### Computing Powers
If $A = PDP^{-1}$, then:
$$A^k = PD^kP^{-1}$$

where $D^k$ is easy to compute (just raise diagonal entries to the $k$-th power).

#### Solving Difference Equations
Systems like $\mathbf{x}_{n+1} = A\mathbf{x}_n$ have solution:
$$\mathbf{x}_n = A^n\mathbf{x}_0 = PD^nP^{-1}\mathbf{x}_0$$

## Symmetric Matrices

### Spectral Theorem
If $A$ is a **symmetric matrix** ($A^T = A$), then:
1. All eigenvalues of $A$ are **real**
2. Eigenvectors corresponding to distinct eigenvalues are **orthogonal**
3. $A$ is **orthogonally diagonalizable**: $A = QDQ^T$ where $Q$ is orthogonal

**Orthogonal diagonalization:** $A = QDQ^{-1} = QDQ^T$ where $Q^TQ = I$

### Spectral Decomposition
For symmetric $A$ with eigenvalues $\lambda_1, \ldots, \lambda_n$ and orthonormal eigenvectors $\mathbf{u}_1, \ldots, \mathbf{u}_n$:
$$A = \lambda_1\mathbf{u}_1\mathbf{u}_1^T + \lambda_2\mathbf{u}_2\mathbf{u}_2^T + \cdots + \lambda_n\mathbf{u}_n\mathbf{u}_n^T$$

### Quadratic Forms
A **quadratic form** is a function $Q(\mathbf{x}) = \mathbf{x}^TA\mathbf{x}$ where $A$ is symmetric.

**Classification:**
- **Positive definite**: All eigenvalues $> 0$ (then $Q(\mathbf{x}) > 0$ for all $\mathbf{x} \neq \mathbf{0}$)
- **Positive semidefinite**: All eigenvalues $\geq 0$
- **Negative definite**: All eigenvalues $< 0$
- **Negative semidefinite**: All eigenvalues $\leq 0$
- **Indefinite**: Both positive and negative eigenvalues

## Singular Value Decomposition (SVD)

### Definition
For any $m \times n$ matrix $A$, there exist:
- Orthogonal matrix $U$ ($m \times m$)
- Orthogonal matrix $V$ ($n \times n$)
- Diagonal matrix $\Sigma$ ($m \times n$)

such that:
$$A = U\Sigma V^T$$

The diagonal entries $\sigma_1 \geq \sigma_2 \geq \cdots \geq \sigma_r > 0$ of $\Sigma$ are the **singular values** of $A$.

### Relationship to Eigenvalues
- Singular values of $A$ are the square roots of eigenvalues of $A^TA$ (or $AA^T$)
- Columns of $V$ are eigenvectors of $A^TA$
- Columns of $U$ are eigenvectors of $AA^T$

### Properties
1. $\text{rank}(A) = r$ = number of nonzero singular values
2. Largest singular value = $\|\mathbf{A}\|_2$ (operator norm)
3. Works for any matrix (not just square or symmetric)

### Finding SVD
**Steps:**
1. Compute $A^TA$ (which is $n \times n$ and symmetric)
2. Find eigenvalues $\lambda_1 \geq \lambda_2 \geq \cdots \geq \lambda_n$ of $A^TA$
3. Singular values: $\sigma_i = \sqrt{\lambda_i}$
4. Find orthonormal eigenvectors $\mathbf{v}_1, \ldots, \mathbf{v}_n$ of $A^TA$ (columns of $V$)
5. Compute $\mathbf{u}_i = \frac{1}{\sigma_i}A\mathbf{v}_i$ for $i = 1, \ldots, r$ (columns of $U$)
6. Extend $\{\mathbf{u}_1, \ldots, \mathbf{u}_r\}$ to orthonormal basis of $\mathbb{R}^m$

### Applications of SVD

#### Low-Rank Approximation
The best rank-$k$ approximation to $A$ (in terms of Frobenius norm or operator norm) is:
$$A_k = \sigma_1\mathbf{u}_1\mathbf{v}_1^T + \sigma_2\mathbf{u}_2\mathbf{v}_2^T + \cdots + \sigma_k\mathbf{u}_k\mathbf{v}_k^T$$

#### Image Compression
Represent an image matrix $A$ using only top $k$ singular values/vectors, achieving compression.

#### Principal Component Analysis (PCA)
SVD is the computational tool for PCA, finding directions of maximum variance in data.
#### Pseudoinverse
The **Moore-Penrose pseudoinverse** $A^+$ is defined using SVD:
$$A^+ = V\Sigma^+ U^T$$
where $\Sigma^+$ is formed by taking reciprocals of nonzero singular values and transposing.

**Use:** Solve $A\mathbf{x} = \mathbf{b}$ when $A$ is not invertible. The solution $\mathbf{x} = A^+\mathbf{b}$ minimizes $\|A\mathbf{x} - \mathbf{b}\|$.

### Example
For $A = \begin{bmatrix}3&0\\4&5\end{bmatrix}$:

Compute $A^TA = \begin{bmatrix}25&20\\20&25\end{bmatrix}$

Eigenvalues: $\lambda_1 = 45$, $\lambda_2 = 5$

Singular values: $\sigma_1 = \sqrt{45} = 3\sqrt{5}$, $\sigma_2 = \sqrt{5}$

(Continue to find $U$ and $V$...)

## Summary
**Key Concepts:**
- **Eigenvalues and eigenvectors**: Special scalars and directions for a matrix
- **Diagonalization**: Representing a matrix in a simple diagonal form
- **Symmetric matrices**: Real eigenvalues, orthogonal eigenvectors
- **SVD**: Decomposition that works for any matrix, with many applications

**Main Theorems:**
1. $A$ is diagonalizable iff it has $n$ linearly independent eigenvectors
2. Symmetric matrices are orthogonally diagonalizable
3. Every matrix has a singular value decomposition
