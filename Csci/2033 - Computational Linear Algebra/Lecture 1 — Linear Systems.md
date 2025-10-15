---
tags:
  - type/lecture
  - course/csci2033
  - subject/Csci
course: CSCI 2033
topic(s): Linear Equations, Gaussian elimination, Vector Arithmetic, Matrix Arithmetic
---
# Linear Systems

## Linear Equations

A **linear equation** in variables $x_1, x_2, \ldots, x_n$ is an equation that can be written in the form:
$$a_1x_1 + a_2x_2 + \cdots + a_nx_n = b$$

where $a_1, a_2, \ldots, a_n$ and $b$ are constants (coefficients).

### Properties of Linear Equations
- Each variable appears only to the first power
- No products of variables (e.g., $x_1x_2$)
- No nonlinear functions (e.g., $\sin(x)$, $e^x$, $x^2$)

### Examples
**Linear:**
- $3x_1 - 2x_2 + 5x_3 = 7$
- $x - y = 0$
- $\frac{1}{2}x_1 + \frac{3}{4}x_2 = 1$

**Not Linear:**
- $x_1x_2 + 3x_3 = 4$ (product of variables)
- $x_1^2 + x_2 = 5$ (squared term)
- $\sin(x_1) + x_2 = 0$ (nonlinear function)

## System of Linear Equations

A **system of linear equations** is a collection of one or more linear equations involving the same variables.

### General Form
$$\begin{align}
a_{11}x_1 + a_{12}x_2 + \cdots + a_{1n}x_n &= b_1 \\
a_{21}x_1 + a_{22}x_2 + \cdots + a_{2n}x_n &= b_2 \\
&\vdots \\
a_{m1}x_1 + a_{m2}x_2 + \cdots + a_{mn}x_n &= b_m
\end{align}$$

### Solutions
A **solution** of a system is an assignment of values to the variables that satisfies all equations simultaneously.

A system can have:
1. **Exactly one solution** (consistent and independent)
2. **Infinitely many solutions** (consistent and dependent)
3. **No solution** (inconsistent)

## Gaussian Elimination

**Gaussian elimination** is an algorithm for solving systems of linear equations by transforming the system into **row echelon form** (REF) using elementary row operations.

### Elementary Row Operations
1. **Row swap**: Interchange two rows
2. **Row scaling**: Multiply a row by a nonzero constant
3. **Row addition**: Add a multiple of one row to another row

These operations preserve the solution set of the system.

### Row Echelon Form (REF)
A matrix is in **row echelon form** if:
1. All rows consisting entirely of zeros are at the bottom
2. The first nonzero entry in each nonzero row (called a **pivot** or **leading entry**) is to the right of the pivot in the row above it
3. All entries below a pivot are zero

### Reduced Row Echelon Form (RREF)
A matrix is in **reduced row echelon form** if it is in REF and additionally:
1. Each pivot equals 1
2. Each pivot is the only nonzero entry in its column

### Algorithm Steps
1. Write the augmented matrix of the system
2. Use row operations to transform to REF (or RREF)
3. Use back-substitution to find solutions (or read off solutions from RREF)

### Example
Solve the system:
$$\begin{align}
x_1 + 2x_2 - x_3 &= 4 \\
2x_1 + x_2 + x_3 &= 5 \\
x_1 - x_2 + 2x_3 &= 1
\end{align}$$

**Augmented matrix:**
$$\left[\begin{array}{ccc|c}
1 & 2 & -1 & 4 \\
2 & 1 & 1 & 5 \\
1 & -1 & 2 & 1
\end{array}\right]$$

Apply row operations to reach RREF and solve.

## Vector Arithmetic

A **vector** in $\mathbb{R}^n$ is an ordered list of $n$ real numbers, typically written as a column:
$$\mathbf{v} = \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix}$$

### Vector Addition
If $\mathbf{u}, \mathbf{v} \in \mathbb{R}^n$, then:
$$\mathbf{u} + \mathbf{v} = \begin{bmatrix} u_1 \\ u_2 \\ \vdots \\ u_n \end{bmatrix} + \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix} = \begin{bmatrix} u_1 + v_1 \\ u_2 + v_2 \\ \vdots \\ u_n + v_n \end{bmatrix}$$

### Scalar Multiplication
If $\mathbf{v} \in \mathbb{R}^n$ and $c \in \mathbb{R}$, then:
$$c\mathbf{v} = c\begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix} = \begin{bmatrix} cv_1 \\ cv_2 \\ \vdots \\ cv_n \end{bmatrix}$$

### Properties
For vectors $\mathbf{u}, \mathbf{v}, \mathbf{w}$ and scalars $c, d$:
1. $\mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u}$ (commutativity)
2. $(\mathbf{u} + \mathbf{v}) + \mathbf{w} = \mathbf{u} + (\mathbf{v} + \mathbf{w})$ (associativity)
3. $\mathbf{u} + \mathbf{0} = \mathbf{u}$ (additive identity)
4. $\mathbf{u} + (-\mathbf{u}) = \mathbf{0}$ (additive inverse)
5. $c(d\mathbf{v}) = (cd)\mathbf{v}$
6. $(c + d)\mathbf{v} = c\mathbf{v} + d\mathbf{v}$
7. $c(\mathbf{u} + \mathbf{v}) = c\mathbf{u} + c\mathbf{v}$
8. $1\mathbf{v} = \mathbf{v}$

## Matrix Arithmetic

A **matrix** is a rectangular array of numbers arranged in rows and columns.

An $m \times n$ matrix $A$ has $m$ rows and $n$ columns:
$$A = \begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn}
\end{bmatrix}$$

### Matrix Addition
If $A$ and $B$ are both $m \times n$ matrices:
$$(A + B)_{ij} = a_{ij} + b_{ij}$$

Add corresponding entries.

### Scalar Multiplication
If $A$ is an $m \times n$ matrix and $c$ is a scalar:
$$(cA)_{ij} = ca_{ij}$$

Multiply each entry by $c$.

### Matrix Multiplication
If $A$ is $m \times n$ and $B$ is $n \times p$, then $AB$ is $m \times p$ with:
$$(AB)_{ij} = \sum_{k=1}^n a_{ik}b_{kj}$$

The $(i,j)$ entry of $AB$ is the dot product of the $i$-th row of $A$ with the $j$-th column of $B$.

### Properties of Matrix Multiplication
1. $(AB)C = A(BC)$ (associativity)
2. $A(B + C) = AB + AC$ (left distributivity)
3. $(A + B)C = AC + BC$ (right distributivity)
4. $c(AB) = (cA)B = A(cB)$ for scalar $c$
5. **NOT commutative**: Generally $AB \neq BA$

### Identity Matrix
The $n \times n$ **identity matrix** $I_n$ has 1's on the diagonal and 0's elsewhere:
$$I_n = \begin{bmatrix}
1 & 0 & \cdots & 0 \\
0 & 1 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1
\end{bmatrix}$$

Property: $AI_n = I_mA = A$ for any $m \times n$ matrix $A$.