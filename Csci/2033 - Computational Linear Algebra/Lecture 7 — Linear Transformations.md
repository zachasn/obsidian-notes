---
tags:
  - type/lecture
  - course/csci2033
  - subject/Csci
course: CSCI 2033
topic(s): Linear Transformations, Kernel and Range of a Linear Transformation, Rank and Nullity
---
# Linear Transformations

## Linear Transformations

### Definition
A **linear transformation** (or **linear map**) $T: V \to W$ between vector spaces $V$ and $W$ is a function that preserves vector addition and scalar multiplication:

1. **Additivity**: $T(\mathbf{u} + \mathbf{v}) = T(\mathbf{u}) + T(\mathbf{v})$ for all $\mathbf{u}, \mathbf{v} \in V$
2. **Homogeneity**: $T(c\mathbf{v}) = cT(\mathbf{v})$ for all $\mathbf{v} \in V$ and scalars $c$

**Equivalent condition:** $T$ is linear iff:
$$T(c_1\mathbf{v}_1 + c_2\mathbf{v}_2) = c_1T(\mathbf{v}_1) + c_2T(\mathbf{v}_2)$$

for all vectors and scalars.

### Properties of Linear Transformations
If $T: V \to W$ is linear, then:
1. $T(\mathbf{0}) = \mathbf{0}$ (maps zero to zero)
2. $T(-\mathbf{v}) = -T(\mathbf{v})$
3. $T(c_1\mathbf{v}_1 + \cdots + c_n\mathbf{v}_n) = c_1T(\mathbf{v}_1) + \cdots + c_nT(\mathbf{v}_n)$

### Examples of Linear Transformations

#### Zero Transformation
$T: V \to W$ defined by $T(\mathbf{v}) = \mathbf{0}$ for all $\mathbf{v} \in V$

#### Identity Transformation
$I: V \to V$ defined by $I(\mathbf{v}) = \mathbf{v}$

#### Matrix Transformation
$T: \mathbb{R}^n \to \mathbb{R}^m$ defined by $T(\mathbf{x}) = A\mathbf{x}$ for a fixed $m \times n$ matrix $A$

**Fundamental Theorem:** Every linear transformation from $\mathbb{R}^n$ to $\mathbb{R}^m$ is a matrix transformation.

#### Rotation in $\mathbb{R}^2$
$T: \mathbb{R}^2 \to \mathbb{R}^2$ that rotates vectors by angle $\theta$:
$$T(\mathbf{x}) = \begin{bmatrix}\cos\theta & -\sin\theta \\ \sin\theta & \cos\theta\end{bmatrix}\mathbf{x}$$

#### Projection
Orthogonal projection onto a subspace $W$: $T(\mathbf{v}) = \text{proj}_W\mathbf{v}$

#### Differentiation
$D: \mathbb{P}_n \to \mathbb{P}_{n-1}$ defined by $D(p) = p'$ (derivative)

#### Integration
$I: \mathbb{C}[a,b] \to \mathbb{R}$ defined by $I(f) = \int_a^b f(x)\,dx$

### Non-Examples
Not every transformation is linear!

**Non-linear:**
- $T(\mathbf{x}) = \mathbf{x} + \mathbf{b}$ where $\mathbf{b} \neq \mathbf{0}$ (translation)
- $T(x) = x^2$
- $T(x) = |x|$
- $T(x) = \sin(x)$

## Kernel and Range

### Kernel (Null Space)
The **kernel** (or **null space**) of $T: V \to W$ is:
$$\ker(T) = \{\mathbf{v} \in V : T(\mathbf{v}) = \mathbf{0}\}$$

The set of all vectors that map to zero.

**Theorem:** $\ker(T)$ is a subspace of $V$.

**For matrix transformations:** If $T(\mathbf{x}) = A\mathbf{x}$, then $\ker(T) = \text{Null}(A)$.

### Range (Image)
The **range** (or **image**) of $T: V \to W$ is:
$$\text{range}(T) = \{T(\mathbf{v}) : \mathbf{v} \in V\} = \{\mathbf{w} \in W : \mathbf{w} = T(\mathbf{v}) \text{ for some } \mathbf{v} \in V\}$$

The set of all outputs of $T$.

**Theorem:** $\text{range}(T)$ is a subspace of $W$.

**For matrix transformations:** If $T(\mathbf{x}) = A\mathbf{x}$, then $\text{range}(T) = \text{Col}(A)$.

### Properties

#### Injectivity (One-to-One)
$T$ is **injective** (or **one-to-one**) if different inputs give different outputs:
$$T(\mathbf{u}) = T(\mathbf{v}) \implies \mathbf{u} = \mathbf{v}$$

**Theorem:** $T$ is injective iff $\ker(T) = \{\mathbf{0}\}$.

#### Surjectivity (Onto)
$T$ is **surjective** (or **onto**) if every element of $W$ is in the range:
$$\text{range}(T) = W$$

For every $\mathbf{w} \in W$, there exists $\mathbf{v} \in V$ such that $T(\mathbf{v}) = \mathbf{w}$.

#### Bijectivity
$T$ is **bijective** (or an **isomorphism**) if it is both injective and surjective.

Bijective linear transformations are invertible.

### Examples

#### Example 1: Matrix Transformation
Let $T: \mathbb{R}^3 \to \mathbb{R}^2$ be defined by:
$$T(\mathbf{x}) = \begin{bmatrix}1 & 2 & 3 \\ 0 & 1 & 2\end{bmatrix}\mathbf{x}$$

- $\ker(T) = \text{Null}(A)$ (solve $A\mathbf{x} = \mathbf{0}$)
- $\text{range}(T) = \text{Col}(A)$ (span of columns of $A$)

#### Example 2: Differentiation
$D: \mathbb{P}_3 \to \mathbb{P}_2$ defined by $D(p) = p'$

- $\ker(D) = \mathbb{P}_0$ (constant polynomials)
- $\text{range}(D) = \mathbb{P}_2$ (all polynomials of degree $\leq 2$)
- $D$ is surjective but not injective

## Rank and Nullity

### Rank
The **rank** of a linear transformation $T: V \to W$ is:
$$\text{rank}(T) = \dim(\text{range}(T))$$

**For matrix transformations:** $\text{rank}(T) = \text{rank}(A)$

### Nullity
The **nullity** of a linear transformation $T: V \to W$ is:
$$\text{nullity}(T) = \dim(\ker(T))$$

**For matrix transformations:** $\text{nullity}(T) = \text{nullity}(A)$

## Rank-Nullity Theorem

### Statement
Let $T: V \to W$ be a linear transformation where $V$ is finite-dimensional. Then:
$$\text{rank}(T) + \text{nullity}(T) = \dim(V)$$

Equivalently:
$$\dim(\text{range}(T)) + \dim(\ker(T)) = \dim(V)$$

This is also called the **Dimension Theorem** or **Fundamental Theorem of Linear Transformations**.

### Interpretation
- $\dim(V)$ = dimension of domain
- $\text{rank}(T)$ = "effective dimension" of the output
- $\text{nullity}(T)$ = "dimension lost" in the transformation

### Applications

#### Determining Injectivity and Surjectivity
For $T: V \to W$:
- $T$ is injective iff $\text{nullity}(T) = 0$ iff $\text{rank}(T) = \dim(V)$
- $T$ is surjective iff $\text{rank}(T) = \dim(W)$
- If $\dim(V) = \dim(W)$, then $T$ is injective iff $T$ is surjective iff $T$ is bijective

#### Example
Let $T: \mathbb{R}^5 \to \mathbb{R}^3$ be linear with $\text{nullity}(T) = 2$.

Then:
- $\text{rank}(T) = 5 - 2 = 3$
- Since $\text{rank}(T) = \dim(\mathbb{R}^3)$, $T$ is surjective
- Since $\text{nullity}(T) \neq 0$, $T$ is not injective

### Finding Kernel and Range

#### Finding a Basis for Kernel
To find a basis for $\ker(T)$:
1. Set up the equation $T(\mathbf{v}) = \mathbf{0}$
2. If $T$ is a matrix transformation $T(\mathbf{x}) = A\mathbf{x}$, solve $A\mathbf{x} = \mathbf{0}$
3. Express the solution in parametric form
4. The parameter vectors form a basis for $\ker(T)$

#### Finding a Basis for Range
To find a basis for $\text{range}(T)$:
1. **Method 1:** Find $T$ evaluated on a basis for $V$. The range is spanned by these images. Remove dependent vectors.
2. **Method 2:** If $T(\mathbf{x}) = A\mathbf{x}$, find a basis for $\text{Col}(A)$ using row reduction.

### Example Problem
Let $T: \mathbb{R}^3 \to \mathbb{R}^3$ be defined by:
$$T\left(\begin{bmatrix}x_1\\x_2\\x_3\end{bmatrix}\right) = \begin{bmatrix}x_1 + x_2\\x_2 + x_3\\x_1 + x_3\end{bmatrix}$$

**Find kernel:**
Solve $T(\mathbf{x}) = \mathbf{0}$:
$$\begin{bmatrix}x_1 + x_2\\x_2 + x_3\\x_1 + x_3\end{bmatrix} = \begin{bmatrix}0\\0\\0\end{bmatrix}$$

This gives $x_1 = -x_2$, $x_3 = -x_2$, so:
$$\mathbf{x} = x_2\begin{bmatrix}-1\\1\\-1\end{bmatrix}$$

Basis for $\ker(T)$: $\left\{\begin{bmatrix}-1\\1\\-1\end{bmatrix}\right\}$

**Find range:**
Matrix representation:
$$A = \begin{bmatrix}1&1&0\\0&1&1\\1&0&1\end{bmatrix}$$

Row reduce to find $\text{rank}(A) = 2$, so $\dim(\text{range}(T)) = 2$.

**Verify Rank-Nullity:**
$$\text{rank}(T) + \text{nullity}(T) = 2 + 1 = 3 = \dim(\mathbb{R}^3)$$ ✓

## Composition of Linear Transformations

### Definition
If $T: U \to V$ and $S: V \to W$ are linear transformations, their **composition** is:
$$(S \circ T): U \to W \quad \text{defined by} \quad (S \circ T)(\mathbf{u}) = S(T(\mathbf{u}))$$

### Properties
1. The composition of linear transformations is linear
2. Composition is associative: $(R \circ S) \circ T = R \circ (S \circ T)$
3. Composition is NOT commutative in general: $S \circ T \neq T \circ S$

### Matrix Representation
If $T(\mathbf{x}) = A\mathbf{x}$ and $S(\mathbf{x}) = B\mathbf{x}$, then:
$$(S \circ T)(\mathbf{x}) = B(A\mathbf{x}) = (BA)\mathbf{x}$$

The matrix of $S \circ T$ is the product $BA$ (note the order!).
