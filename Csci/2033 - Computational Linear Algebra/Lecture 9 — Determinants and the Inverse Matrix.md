---
tags:
  - type/lecture
  - course/csci2033
  - subject/Csci
course: CSCI 2033
topic(s): Determinant of a Matrix, Properties of Determinants, LU Factorization
---
# Determinants and the Inverse Matrix

## Determinant of a Matrix

### Definition for 2×2 Matrices
For a $2 \times 2$ matrix:
$$\det\begin{bmatrix}a & b \\ c & d\end{bmatrix} = ad - bc$$

Also written as:
$$\begin{vmatrix}a & b \\ c & d\end{vmatrix} = ad - bc$$

### Cofactor Expansion (Laplace Expansion)
For an $n \times n$ matrix $A$, the determinant can be computed by expanding along any row or column.

**Expansion along row $i$:**
$$\det(A) = \sum_{j=1}^n a_{ij}C_{ij} = a_{i1}C_{i1} + a_{i2}C_{i2} + \cdots + a_{in}C_{in}$$

**Expansion along column $j$:**
$$\det(A) = \sum_{i=1}^n a_{ij}C_{ij} = a_{1j}C_{1j} + a_{2j}C_{2j} + \cdots + a_{nj}C_{nj}$$

where $C_{ij}$ is the **cofactor** of entry $a_{ij}$.

### Minors and Cofactors
The **minor** $M_{ij}$ is the determinant of the $(n-1) \times (n-1)$ matrix obtained by deleting row $i$ and column $j$ from $A$.

The **cofactor** is:
$$C_{ij} = (-1)^{i+j}M_{ij}$$

The sign pattern follows a checkerboard:
$$\begin{bmatrix}
+ & - & + & \cdots \\
- & + & - & \cdots \\
+ & - & + & \cdots \\
\vdots & \vdots & \vdots & \ddots
\end{bmatrix}$$

### Example: 3×3 Determinant
$$\det\begin{bmatrix}a & b & c \\ d & e & f \\ g & h & i\end{bmatrix} = a\begin{vmatrix}e & f \\ h & i\end{vmatrix} - b\begin{vmatrix}d & f \\ g & i\end{vmatrix} + c\begin{vmatrix}d & e \\ g & h\end{vmatrix}$$

$$= a(ei - fh) - b(di - fg) + c(dh - eg)$$

### Determinant of Special Matrices

**Diagonal matrix:**
$$\det\begin{bmatrix}d_1 & 0 & \cdots & 0 \\ 0 & d_2 & \cdots & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \cdots & d_n\end{bmatrix} = d_1d_2 \cdots d_n$$

**Upper/Lower triangular matrix:** Product of diagonal entries

**Identity matrix:** $\det(I_n) = 1$

**Zero matrix:** $\det(0) = 0$

## Properties of Determinants

### Row Operations
1. **Row swap**: Swapping two rows multiplies the determinant by $-1$
   $$\det(B) = -\det(A)$$

2. **Row scaling**: Multiplying a row by $k$ multiplies the determinant by $k$
   $$\det(B) = k\det(A)$$

3. **Row addition**: Adding a multiple of one row to another does NOT change the determinant
   $$\det(B) = \det(A)$$

### Multiplicative Property
$$\det(AB) = \det(A)\det(B)$$

This is one of the most important properties!

### Transpose
$$\det(A^T) = \det(A)$$

### Inverse
If $A$ is invertible:
$$\det(A^{-1}) = \frac{1}{\det(A)}$$

### Scalar Multiple
$$\det(cA) = c^n\det(A)$$
for an $n \times n$ matrix $A$.

### Other Properties
1. If $A$ has a row (or column) of zeros, then $\det(A) = 0$
2. If two rows (or columns) are identical, then $\det(A) = 0$
3. If two rows (or columns) are proportional, then $\det(A) = 0$
4. $\det(A) = \det(A^T)$ (so row and column properties are the same)

## Determinant and Invertibility

### Invertible Matrix Theorem
For a square matrix $A$, the following are equivalent:
1. $A$ is invertible
2. $\det(A) \neq 0$
3. $A$ is row equivalent to $I_n$
4. $A\mathbf{x} = \mathbf{b}$ has a unique solution for every $\mathbf{b}$
5. $A\mathbf{x} = \mathbf{0}$ has only the trivial solution
6. The columns of $A$ are linearly independent
7. The columns of $A$ span $\mathbb{R}^n$
8. $\text{rank}(A) = n$
9. $\text{nullity}(A) = 0$

### Singular vs. Nonsingular
- **Nonsingular**: $\det(A) \neq 0$ (invertible)
- **Singular**: $\det(A) = 0$ (not invertible)

## Cramer's Rule

For the system $A\mathbf{x} = \mathbf{b}$ where $A$ is invertible ($\det(A) \neq 0$), the solution is:
$$x_i = \frac{\det(A_i)}{\det(A)}$$

where $A_i$ is the matrix obtained by replacing the $i$-th column of $A$ with $\mathbf{b}$.

### Example
Solve:
$$\begin{cases}
2x_1 + x_2 = 5 \\
x_1 + 3x_2 = 8
\end{cases}$$

$$A = \begin{bmatrix}2&1\\1&3\end{bmatrix}, \quad \mathbf{b} = \begin{bmatrix}5\\8\end{bmatrix}$$

$$\det(A) = 6 - 1 = 5$$

$$A_1 = \begin{bmatrix}5&1\\8&3\end{bmatrix}, \quad \det(A_1) = 15 - 8 = 7$$

$$A_2 = \begin{bmatrix}2&5\\1&8\end{bmatrix}, \quad \det(A_2) = 16 - 5 = 11$$

$$x_1 = \frac{7}{5}, \quad x_2 = \frac{11}{5}$$

**Note:** Cramer's rule is computationally expensive for large systems; Gaussian elimination is more efficient.

## Adjugate Matrix and Matrix Inverse

### Adjugate Matrix
The **adjugate** (or **classical adjoint**) of $A$ is the transpose of the cofactor matrix:
$$\text{adj}(A) = [C_{ij}]^T$$

where $C_{ij}$ are the cofactors.

### Inverse Formula
If $\det(A) \neq 0$, then:
$$A^{-1} = \frac{1}{\det(A)}\text{adj}(A)$$

### Example: 2×2 Inverse
For $A = \begin{bmatrix}a&b\\c&d\end{bmatrix}$:
$$A^{-1} = \frac{1}{ad-bc}\begin{bmatrix}d&-b\\-c&a\end{bmatrix}$$

**Note:** For matrices larger than $3 \times 3$, Gauss-Jordan elimination is more efficient than using the adjugate formula.

## Geometric Interpretation

### Area and Volume
For a $2 \times 2$ matrix $A$:
$$|\det(A)| = \text{area of parallelogram formed by columns of } A$$

For a $3 \times 3$ matrix $A$:
$$|\det(A)| = \text{volume of parallelepiped formed by columns of } A$$

### Linear Transformation
If $T: \mathbb{R}^n \to \mathbb{R}^n$ is represented by matrix $A$, then:
$$|\det(A)| = \text{scaling factor for volumes under } T$$

- If $\det(A) = 0$, $T$ collapses dimension (not invertible)
- If $|\det(A)| > 1$, $T$ expands volumes
- If $|\det(A)| < 1$, $T$ contracts volumes
- If $\det(A) < 0$, $T$ reverses orientation

## LU Factorization

### Definition
An **LU factorization** (or **LU decomposition**) of a matrix $A$ is:
$$A = LU$$
where:
- $L$ is a **lower triangular** matrix
- $U$ is an **upper triangular** matrix

### When Does LU Factorization Exist?
Not every matrix has an LU factorization. It exists if $A$ can be row reduced to echelon form **without row swaps**.

If row swaps are needed, we use **PLU factorization**: $PA = LU$ where $P$ is a permutation matrix.

### Finding LU Factorization
**Method:** Use Gaussian elimination to reduce $A$ to $U$, keeping track of the row operations. The multipliers form $L$.

**Steps:**
1. Reduce $A$ to echelon form $U$ using only row addition operations (no row swaps)
2. Record the multipliers used in each step
3. Form $L$ from these multipliers (with 1's on the diagonal)

### Example
Find the LU factorization of $A = \begin{bmatrix}2&4&-2\\1&-1&3\\3&2&1\end{bmatrix}$

**Step 1:** Eliminate below first pivot
- $R_2 - \frac{1}{2}R_1 \to R_2$
- $R_3 - \frac{3}{2}R_1 \to R_3$

**Step 2:** Eliminate below second pivot
- $R_3 - \frac{2}{3}R_2 \to R_3$

Result:
$$U = \begin{bmatrix}2&4&-2\\0&-3&4\\0&0&3\end{bmatrix}$$

$$L = \begin{bmatrix}1&0&0\\\frac{1}{2}&1&0\\\frac{3}{2}&\frac{2}{3}&1\end{bmatrix}$$

Verify: $LU = A$

### Applications of LU Factorization

#### Solving $A\mathbf{x} = \mathbf{b}$
1. Factor $A = LU$
2. Solve $L\mathbf{y} = \mathbf{b}$ by forward substitution
3. Solve $U\mathbf{x} = \mathbf{y}$ by back substitution

**Advantage:** Once $A$ is factored, solving for different $\mathbf{b}$ vectors is fast.

#### Computing Determinant
$$\det(A) = \det(L)\det(U) = (1)(u_{11}u_{22}\cdots u_{nn})$$

Since $L$ has 1's on diagonal, $\det(L) = 1$, and $\det(U)$ is the product of diagonal entries.

#### Efficiency
- LU factorization: $O(n^3)$ operations
- Solving with LU: $O(n^2)$ operations per right-hand side
- Better than Gaussian elimination when solving multiple systems with the same $A$

### PLU Factorization (with Pivoting)
For numerical stability and to handle cases where row swaps are needed:
$$PA = LU$$
where $P$ is a **permutation matrix**.

**Process:** Use partial pivoting during elimination, keep track of row swaps in $P$.
