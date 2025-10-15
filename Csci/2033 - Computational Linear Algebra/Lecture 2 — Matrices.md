---
tags:
  - type/lecture
  - course/csci2033
  - subject/Csci
course: CSCI 2033
topic(s): Matrix Algebra, Transpose of a Matrix, Solutions, Inverse of a Matrix
book sections: 1.5, 1.6, 1.7, 1.8
---
# Matrices

## Matrix Algebra

### Matrix Definition
A **matrix** is a rectangular array of numbers. An $m \times n$ matrix has $m$ rows and $n$ columns:
$$F_0$$
$$A = \begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn}
\end{bmatrix}$$

We write $A = [a_{ij}]$ where $a_{ij}$ is the entry in row $i$, column $j$.

### Special Matrices

**Square Matrix**: A matrix with the same number of rows and columns ($m = n$)

**Zero Matrix**: All entries are zero, denoted $\mathbf{0}$ or $0_{m \times n}$

**Diagonal Matrix**: A square matrix where all off-diagonal entries are zero
$$D = \begin{bmatrix}
d_1 & 0 & \cdots & 0 \\
0 & d_2 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & d_n
\end{bmatrix}$$

**Identity Matrix**: A diagonal matrix with all diagonal entries equal to 1
$$I_n = \begin{bmatrix}
1 & 0 & \cdots & 0 \\
0 & 1 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1
\end{bmatrix}$$

**Upper Triangular Matrix**: All entries below the main diagonal are zero

**Lower Triangular Matrix**: All entries above the main diagonal are zero

### Matrix Operations

#### Addition
If $A$ and $B$ are both $m \times n$ matrices:
$$C = A + B \quad \text{where} \quad c_{ij} = a_{ij} + b_{ij}$$

**Properties:**
- Commutative: $A + B = B + A$
- Associative: $(A + B) + C = A + (B + C)$
- Identity: $A + \mathbf{0} = A$
- Inverse: $A + (-A) = \mathbf{0}$

#### Scalar Multiplication
If $c$ is a scalar and $A$ is a matrix:
$$B = cA \quad \text{where} \quad b_{ij} = ca_{ij}$$

**Properties:**
- $c(A + B) = cA + cB$
- $(c + d)A = cA + dA$
- $(cd)A = c(dA)$
- $1A = A$

#### Matrix Multiplication
If $A$ is $m \times n$ and $B$ is $n \times p$, then $C = AB$ is $m \times p$ with:
$$c_{ij} = \sum_{k=1}^n a_{ik}b_{kj}$$

The $(i,j)$ entry of $AB$ is the **dot product** of row $i$ of $A$ with column $j$ of $B$.

**Properties:**
- Associative: $(AB)C = A(BC)$
- Distributive: $A(B + C) = AB + AC$ and $(A + B)C = AC + BC$
- Identity: $AI = IA = A$ (where $I$ has appropriate dimensions)
- **NOT commutative**: Generally $AB \neq BA$
- If $AB = \mathbf{0}$, it does NOT imply $A = \mathbf{0}$ or $B = \mathbf{0}$

#### Matrix Powers
For a square matrix $A$:
$$A^n = \underbrace{A \cdot A \cdot \ldots \cdot A}_{n \text{ times}}$$

By convention: $A^0 = I$
## Transpose of a Matrix
The **transpose** of an $m \times n$ matrix $A$ is the $n \times m$ matrix $A^T$ obtained by interchanging rows and columns:
$$(A^T)_{ij} = a_{ji}$$
### Example
$$A = \begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6
\end{bmatrix} \implies A^T = \begin{bmatrix}
1 & 4 \\
2 & 5 \\
3 & 6
\end{bmatrix}$$
### Properties of Transpose
1. $(A^T)^T = A$
2. $(A + B)^T = A^T + B^T$
3. $(cA)^T = cA^T$ for scalar $c$
4. $(AB)^T = B^TA^T$ (order reverses!)
5. $(A_1A_2\cdots A_k)^T = A_k^T\cdots A_2^TA_1^T$
### Symmetric Matrices
A square matrix $A$ is **symmetric** if $A^T = A$.

This means $a_{ij} = a_{ji}$ for all $i, j$.

**Example:**
$$A = \begin{bmatrix}
1 & 2 & 3 \\
2 & 4 & 5 \\
3 & 5 & 6
\end{bmatrix}$$
### Skew-Symmetric Matrices
A square matrix $A$ is **skew-symmetric** if $A^T = -A$.

This means $a_{ij} = -a_{ji}$ for all $i, j$, and $a_{ii} = 0$ for all $i$.

## Solutions
A **solution** of a system is an assignment of values to the variables that satisfies all equations simultaneously.

There are three types of solution to a linear system:
1. **No solution** (inconsistent)
2. **Exactly one solution** (consistent and independent)
3. **Infinitely many solutions** (consistent and dependent)
4. **No solution** (inconsistent)
###### Example
Show the follow linear system is inconsistent
$$\begin{align}
x + y - 2z &= 3 \\
-x + 3y - 5z &= 7 \\
2x - 2y + 7z &= 1
\end{align}$$
**Augmented Matrix:**$$\left[\begin{array}{ccc|c}
1 & 1 & -1 & 3 \\
-1 & 3 & 5 & 7 \\
2 & -2 & 7 & 1
\end{array}\right]$$
**Gaussian elimination:**$$\left[\begin{array}{ccc|c}
1 & 1 & -1 & 3 \\
-1 + 1 & 3 + 1 & 5 - 1 & 7 + 3 \\
2 - (2*1) & - 2 - (2*1) & 7 - (2*-1) & 1 - (2*3)
\end{array}\right] = \left[\begin{array}{ccc|c}
1 & 1 & -1 & 3 \\
0 & 4 & -3 & 10 \\
0 & -4 & 3 & -5
\end{array}\right] $$
$$ \left[\begin{array}{ccc|c}
1 & 1 & -1 & 3 \\
0 & 4 & -3 & 10 \\
0+0 & -4+4 & 3-3 & -5+10
\end{array}\right] =  \left[\begin{array}{ccc|c}
1 & 1 & -1 & 3 \\
0 & 4 & -3 & 10 \\
0 & 0 & 0 & 5
\end{array}\right]$$
$0x + 0y + 0z = 5$ → *False!*
## Inverse of a Matrix
A square matrix $A$ is **invertible** (or **nonsingular**) if there exists a matrix $B$ such that:
$$AB = BA = I$$
The matrix $B$ is called the **inverse** of $A$, denoted $A^{-1}$.
If no such matrix exists, $A$ is called **singular** or **noninvertible**.
### Properties of Inverse
1. $(A^{-1})^{-1} = A$
2. $(AB)^{-1} = B^{-1}A^{-1}$ (order reverses!)
3. $(A^T)^{-1} = (A^{-1})^T$
4. $(cA)^{-1} = \frac{1}{c}A^{-1}$ for nonzero scalar $c$
5. If $A$ is invertible, then $A^{-1}$ is unique

### Computing the Inverse
#### For 2×2 Matrices
If $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$, then:
$$A^{-1} = \frac{1}{ad - bc}\begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$$
provided $ad - bc \neq 0$. The quantity $ad - bc$ is called the **determinant** of $A$.
#### General Method (Gauss-Jordan Elimination)
To find $A^{-1}$:
1. Form the augmented matrix $[A \mid I]$
2. Use row operations to transform it to $[I \mid B]$
3. Then $B = A^{-1}$

If the left side cannot be reduced to $I$, then $A$ is not invertible.
### Example
Find the inverse of $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$
**Method 1 (Formula):**
$$A^{-1} = \frac{1}{(1)(4) - (2)(3)}\begin{bmatrix} 4 & -2 \\ -3 & 1 \end{bmatrix} = \frac{1}{-2}\begin{bmatrix} 4 & -2 \\ -3 & 1 \end{bmatrix} = \begin{bmatrix} -2 & 1 \\ 3/2 & -1/2 \end{bmatrix}$$
**Method 2 (Gauss-Jordan):**
$$\left[\begin{array}{cc|cc} 1 & 2 & 1 & 0 \\ 3 & 4 & 0 & 1 \end{array}\right] \xrightarrow{\text{row ops}} \left[\begin{array}{cc|cc} 1 & 0 & -2 & 1 \\ 0 & 1 & 3/2 & -1/2 \end{array}\right]$$
### Applications of Inverse
**Solving $A\mathbf{x} = \mathbf{b}$:**
If $A$ is invertible, the unique solution is:
$$\mathbf{x} = A^{-1}\mathbf{b}$$
### Invertibility Conditions
For a square matrix $A$, the following are equivalent:
1. $A$ is invertible
2. $A\mathbf{x} = \mathbf{b}$ has a unique solution for every $\mathbf{b}$
3. $A\mathbf{x} = \mathbf{0}$ has only the trivial solution $\mathbf{x} = \mathbf{0}$
4. The rows (or columns) of $A$ are linearly independent
5. $\det(A) \neq 0$ (determinant is nonzero)
6. $A$ can be reduced to the identity matrix using row operations
