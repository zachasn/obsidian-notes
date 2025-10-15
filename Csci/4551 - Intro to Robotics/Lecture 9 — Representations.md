# Lecture 9 — Representations

## Overview

This lecture introduces mathematical representations for describing the position and orientation of rigid bodies in 3D space. Understanding how to represent objects relative to different coordinate frames is fundamental to robotics, as robots must reason about the positions and orientations of objects, tools, and their own components in a shared world coordinate system. We'll develop the mathematical framework using rotation matrices and homogeneous transformations that will be essential for forward kinematics, inverse kinematics, and motion planning.

This builds on [[Lecture 3 — Euclidean Space|Euclidean Space]] and [[Lecture 6 — Inner Product Spaces|Inner Product Spaces]] from linear algebra and sets the foundation for manipulator kinematics discussed in upcoming lectures.

## Key Concepts

### Coordinate Frames and Reference Systems

A **coordinate frame** is a coordinate system attached to a body or point in space. To describe the position and orientation of objects in robotics, we establish:

- **World reference frame** (or global frame): A fixed coordinate system relative to which all other objects are described
- **Body-attached frames**: Coordinate systems rigidly attached to objects or robot links

The goal is to describe the position and orientation of rigid bodies with respect to the world reference frame. This allows us to:
- Track the location of objects in the workspace
- Describe the configuration of robot manipulators
- Transform sensor measurements into a common coordinate system
- Plan collision-free paths

### Mathematical Preliminaries

Before diving into spatial representations, we review key mathematical concepts from linear algebra:

#### Vector Spaces and Norms
- **Set of real numbers**: $\mathbb{R}$
- **Vector space**: $\mathbb{R}^{N}$ represents the space of n-tuples over the real numbers
- **Vectors**: Denoted in bold as **a**, **b** ∈ $\mathbb{R}^N$
- **$L_2$ norm** (Euclidean length): The magnitude of a vector, defined as:
  $$\| \mathbf{x} \| = \left( \sum_{i=1}^n {x_i^2}\right)^{1/2}$$

  This gives the geometric length of a vector in n-dimensional space.

#### Vector Products

**Scalar (Dot) Product**: Measures projection and is used to compute angles between vectors:
$$\mathbf{x}^T\mathbf{y} = \sum_{i=1}^n {x_i y_i}$$

Properties:
- $\mathbf{x}^T\mathbf{y} = \|\mathbf{x}\| \|\mathbf{y}\| \cos(\theta)$ where $\theta$ is the angle between vectors
- Dot product equals zero when vectors are orthogonal (perpendicular)
- Used extensively in computing directional cosines for rotation matrices

**Outer Product**: Creates a matrix from two vectors:
$$\mathbf{x}\mathbf{y}^T = \begin{bmatrix}
x_{1}y_{1} & x_{1}y_{2} & \cdots & x_{1}y_{n} \\
x_{2}y_{1} & x_{2}y_{2} & \cdots & x_{2}y_{n} \\
\vdots & \vdots & \ddots & \vdots \\
x_{n}y_{1} & x_{n}y_{2} & \cdots & x_{n}y_{n}
\end{bmatrix}$$

The result is an $n \times n$ matrix. This operation is fundamental in constructing rotation matrices from coordinate frame basis vectors.

#### Matrix Trace

**Trace of a matrix**: The sum of diagonal elements:
$$\text{Tr}(A_{n \times n}) = \sum_{i=1}^n {a_{ii}}$$

The trace has important properties in rotation matrix analysis and will be useful when we study rotation parameterizations (axis-angle representations).

## Position Representation

### Position Vectors

The **position** of a point P with respect to the world coordinate system is represented as a column vector:

$${}^W\mathbf{p} = \begin{bmatrix}p_{x} \\ p_{y} \\ p_{z}\end{bmatrix}$$

The left superscript $W$ indicates that this position is expressed in the world (or reference) frame. In 2D space, we would simply omit $p_z$.

Key points:
- Position is always relative to some coordinate frame
- The same physical point has different numerical representations in different frames
- Position vectors are translation-dependent but do not capture orientation

### Representing Orientation

Position alone is insufficient to fully describe a rigid body—we also need its **orientation** (how it's rotated). To represent orientation:

1. **Attach a coordinate frame to the body**: Place a coordinate system $\{B\}$ rigidly on the object
2. **Describe this frame relative to the reference frame**: Specify how the body frame's axes relate to the world frame's axes

The orientation is captured by specifying the directions of the body frame's axes ($\mathbf{i}_B$, $\mathbf{j}_B$, $\mathbf{k}_B$) as vectors expressed in the reference frame.

![[Csci4551-Lec9-Orientation.excalidraw]]

### Frame Notation

A complete **frame** $\{B\}$ relative to frame $\{A\}$ is specified by:

$$\{B\} = \left\{{}^A_B\mathbf{R}, {}^A\mathbf{P}_{B\text{-origin}}\right\}$$

also written as:
$$\{B\} = \{\mathbf{R}_{AB}, \mathbf{P}_{AB}\}$$

Where:
- $\mathbf{R}_{AB}$: **Rotation matrix** describing the orientation of frame $\{B\}$ relative to frame $\{A\}$
- $\mathbf{P}_{AB}$: **Position vector** of the origin of frame $\{B\}$ expressed in frame $\{A\}$

This pair $(R, P)$ completely specifies the pose (position + orientation) of frame $\{B\}$.

## Coordinate Frame Transformations

### Example: Point on the Whiteboard

Consider a point $\bar{\mathbf{p}}$ that can be described in two different coordinate frames:

![[position_ex.excalidraw]]

- **Frame 0** (Global frame): $F_0 = \{\mathbf{i}_0, \mathbf{j}_0, \mathbf{k}_0\}$
- **Frame 1** (Whiteboard frame): $F_1 = \{\mathbf{i}_1, \mathbf{j}_1, \mathbf{k}_1\}$

The same physical point $\bar{\mathbf{p}}$ has different coordinate representations:

**In Frame 1 (Whiteboard)**:
$$\bar{\mathbf{p}} = x_{p}^{1} \mathbf{i}_1 + y_{p}^{1} \mathbf{j}_1 + z_{p}^{1} \mathbf{k}_1$$

**In Frame 0 (Global)**:
$$\bar{\mathbf{p}} = x_{p}^{0} \mathbf{i}_0 + y_{p}^{0} \mathbf{j}_0 + z_{p}^{0} \mathbf{k}_0$$

### Mathematical Representation

We can write the frame basis vectors as columns of a matrix:
$$F_0 = \begin{bmatrix} \mathbf{i}_0 & \mathbf{j}_0 & \mathbf{k}_0 \end{bmatrix}$$

Then the physical vector $\bar{\mathbf{p}}$ can be written as:
$$\bar{\mathbf{p}} = F_0^T \mathbf{p}^0$$

where $\mathbf{p}^0 = \begin{bmatrix} x_p^0 \\ y_p^0 \\ z_p^0 \end{bmatrix}$ is the coordinate representation in frame 0.

#### Orthonormality Property

Since coordinate frame axes are orthonormal (perpendicular unit vectors), we have:
$$F_{0}F_{0}^T = \begin{bmatrix} \mathbf{i}_0 \\ \mathbf{j}_0 \\ \mathbf{k}_0 \end{bmatrix} \begin{bmatrix} \mathbf{i}_0 & \mathbf{j}_0 & \mathbf{k}_0 \end{bmatrix} =  \begin{bmatrix} 1 & 0 &  0 \\ 0 & 1 &  0 \\ 0 & 0 &  1 \end{bmatrix} = I$$

This is because:
- $\mathbf{i}_0 \cdot \mathbf{i}_0 = 1$ (unit vector)
- $\mathbf{i}_0 \cdot \mathbf{j}_0 = 0$ (orthogonal)
- And so on for all pairs

### Deriving the Transformation

Starting from the physical vector representation:
$$\bar{\mathbf{p}} = F_0^T \mathbf{p}^0$$

Pre-multiply both sides by $F_0$:
$$\mathbf{p}^{0} = F_{0} \cdot \bar{\mathbf{p}} = F_{0} \cdot F_{0}^{T}\mathbf{p}^{0} = I\mathbf{p}^{0}$$

Similarly for frame 1:
$$\bar{\mathbf{p}} = F_{1}^{T}\mathbf{p}^{1}$$
$$\mathbf{p}^{1} = F_{1} \cdot \bar{\mathbf{p}}$$

#### Finding the Transformation Between Frames

To transform from frame 1 coordinates to frame 0 coordinates, substitute:
$$\mathbf{p}^{0} = F_{0} \cdot \bar{\mathbf{p}} = F_{0} \cdot F_{1}^{T}\mathbf{p}^{1}$$

The product $F_{0}F_{1}^{T}$ is the **rotation matrix** $R_{01}$ that transforms coordinates from frame 1 to frame 0:
$$\mathbf{p}^{0} = R_{01} \mathbf{p}^{1}$$

## The Rotation Matrix

### Definition and Structure

The **rotation matrix** $R_{01}$ transforms vector representations from frame 1 to frame 0:

$$R_{01} = F_{0}F_{1}^{T} = \begin{bmatrix} \mathbf{i}_{0} \cdot \mathbf{i}_{1} & \mathbf{i}_{0} \cdot \mathbf{j}_{1} & \mathbf{i}_{0} \cdot \mathbf{k}_{1} \\ \mathbf{j}_{0} \cdot \mathbf{i}_{1} & \mathbf{j}_{0} \cdot \mathbf{j}_{1} & \mathbf{j}_{0} \cdot \mathbf{k}_{1} \\ \mathbf{k}_{0} \cdot \mathbf{i}_{1} & \mathbf{k}_{0} \cdot \mathbf{j}_{1} & \mathbf{k}_{0} \cdot \mathbf{k}_{1} \end{bmatrix}$$

### Interpretation

The rotation matrix is a **matrix of directional cosines**:
- Each element $r_{ij} = \mathbf{e}_i^0 \cdot \mathbf{e}_j^1$ represents the cosine of the angle between axis $i$ of frame 0 and axis $j$ of frame 1
- **Column $j$**: Contains the components of frame 1's $j$-th axis vector expressed in frame 0 coordinates
- **Row $i$**: Contains the projections of frame 1's axes onto frame 0's $i$-th axis

**Geometric Interpretation Example** (2D for simplicity):

If frame 1 is rotated by angle $\theta$ relative to frame 0 about the z-axis:
$$R_{01} = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}$$

- First column $[\cos\theta, \sin\theta]^T$ = frame 1's x-axis ($\mathbf{i}_1$) in frame 0 coordinates
- Second column $[-\sin\theta, \cos\theta]^T$ = frame 1's y-axis ($\mathbf{j}_1$) in frame 0 coordinates

### Key Properties

Rotation matrices belong to the special orthogonal group $SO(3)$ and have these properties:

1. **Orthonormality**: $R^T R = I$ (columns are orthonormal)
2. **Determinant**: $\det(R) = 1$ (preserves orientation and volume)
3. **Inverse equals transpose**: $R^{-1} = R^T$
4. **Composition**: Product of rotation matrices is a rotation matrix
5. **Preserves lengths and angles**: $\|\mathbf{Rx}\| = \|\mathbf{x}\|$

### Why This Matters

Rotation matrices are fundamental to robotics because they:
- Transform sensor measurements between coordinate frames
- Describe the orientation of robot links in forward kinematics
- Are used in the Denavit-Hartenberg convention for robot modeling
- Enable coordinate transformations for trajectory planning
- Preserve the geometric properties of vectors (lengths and angles)

The ability to transform between coordinate frames is essential whenever multiple reference frames are involved—which is almost always the case in robotics!

## Applications in Robotics

### Robot Manipulator Kinematics

In robot arms, each joint has an associated coordinate frame. Rotation matrices relate these frames to:
- Compute end-effector position and orientation (forward kinematics)
- Transform desired end-effector poses into joint coordinates (inverse kinematics)
- Calculate velocities and forces at different points on the manipulator

### Sensor Data Processing

Sensors (cameras, LiDAR, IMUs) have their own reference frames. Rotation matrices allow us to (relevant for [[Lecture 7 — State Estimation]] and [[Lecture 8 — SLAM]]):
- Transform point cloud data from camera frame to world frame
- Fuse measurements from multiple sensors with different orientations
- Track object poses relative to a moving robot base

### Motion Planning and Control

Path planning algorithms (see [[Lecture 5 — Motion Planning]] and [[Lecture 6 — Motion Planning II]]) need consistent representations:
- Obstacles and free space described in world coordinates
- Robot configuration expressed relative to world frame
- Trajectory waypoints transformed between frames as needed

## Summary

- **Coordinate frames** provide a systematic way to describe positions and orientations of rigid bodies in 3D space
- **Position** is represented as a vector relative to a reference frame
- **Orientation** is represented by attaching a coordinate frame to the body and describing its axes relative to a reference frame
- **Rotation matrices** transform vector representations between coordinate frames and are composed of directional cosines (dot products of frame axes)
- Rotation matrices are orthogonal matrices with determinant +1 (members of $SO(3)$) and have the property $R^{-1} = R^T$
- The combination of position and orientation (rotation) fully describes the **pose** of a rigid body
- These mathematical tools form the foundation for forward kinematics, inverse kinematics, and all spatial reasoning in robotics

## Exercises

### Guided Practice Problems

#### Problem 1: Constructing a Rotation Matrix

**Problem**: Frame $\{B\}$ is oriented relative to frame $\{A\}$ such that:
- The x-axis of frame B ($\mathbf{i}_B$) points in the direction $(0.8, 0.6, 0)$ when expressed in frame A
- The y-axis of frame B ($\mathbf{j}_B$) points in the direction $(-0.6, 0.8, 0)$ when expressed in frame A
- The z-axis of frame B ($\mathbf{k}_B$) points in the direction $(0, 0, 1)$ when expressed in frame A

Construct the rotation matrix $R_{AB}$ and verify it satisfies the orthonormality property.

**Solution**:

The rotation matrix is formed by placing the basis vectors of frame B (expressed in frame A) as columns:

$$R_{AB} = \begin{bmatrix} 0.8 & -0.6 & 0 \\ 0.6 & 0.8 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

**Verification of orthonormality**:

Check $R_{AB}^T R_{AB} = I$:

$$R_{AB}^T R_{AB} = \begin{bmatrix} 0.8 & 0.6 & 0 \\ -0.6 & 0.8 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 0.8 & -0.6 & 0 \\ 0.6 & 0.8 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

$$= \begin{bmatrix} 0.64 + 0.36 & -0.48 + 0.48 & 0 \\ -0.48 + 0.48 & 0.36 + 0.64 & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} = I \,\checkmark$$

Note: This represents a rotation of approximately 36.87° about the z-axis.

#### Problem 2: Coordinate Transformation

**Problem**: A point P has coordinates $\mathbf{p}^B = \begin{bmatrix} 2 \\ 3 \\ 1 \end{bmatrix}$ in frame $\{B\}$. Using the rotation matrix from Problem 1, find the coordinates of this point in frame $\{A\}$.

**Solution**:

Use the transformation equation $\mathbf{p}^A = R_{AB} \mathbf{p}^B$:

$$\mathbf{p}^A = \begin{bmatrix} 0.8 & -0.6 & 0 \\ 0.6 & 0.8 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 2 \\ 3 \\ 1 \end{bmatrix}$$

$$= \begin{bmatrix} 0.8(2) + (-0.6)(3) + 0(1) \\ 0.6(2) + 0.8(3) + 0(1) \\ 0(2) + 0(3) + 1(1) \end{bmatrix} = \begin{bmatrix} 1.6 - 1.8 \\ 1.2 + 2.4 \\ 1 \end{bmatrix} = \begin{bmatrix} -0.2 \\ 3.6 \\ 1 \end{bmatrix}$$

Therefore, $\mathbf{p}^A = \begin{bmatrix} -0.2 \\ 3.6 \\ 1 \end{bmatrix}$.

### Additional Practice Problems

1. **Rotation About X-Axis**: Derive the general rotation matrix $R_x(\theta)$ for a rotation of angle $\theta$ about the x-axis. Start by considering where the basis vectors $\mathbf{i}$, $\mathbf{j}$, and $\mathbf{k}$ point after rotation.

2. **Inverse Transformation**: Given the point from Problem 2, verify that transforming $\mathbf{p}^A$ back to frame $\{B\}$ using $R_{AB}^T$ returns the original coordinates $\mathbf{p}^B$.

3. **Composition of Rotations**: Frame $\{C\}$ is related to frame $\{B\}$ by a rotation of 90° about the z-axis. Using the rotation matrix from Problem 1 and your knowledge of basic rotations, find $R_{AC}$ (the rotation from frame C to frame A).

4. **Verification Properties**: For the rotation matrix $R = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}$, verify that:
   - $\det(R) = 1$
   - $\text{Tr}(R) = 1 + 2\cos\theta$
   - The columns form an orthonormal set of vectors

5. **Robot Gripper Application**: A robot gripper frame $\{G\}$ is rotated 30° about the z-axis and then 45° about the resulting y-axis, relative to the robot base frame $\{B\}$. If the gripper is holding an object at position $(5, 0, 0)$ cm in the gripper frame, what are the coordinates of this object in the base frame? (Hint: You'll need to compose two rotation matrices.)

## Review Questions

1. **Conceptual**: Why is specifying only the position of a rigid body insufficient for robotics applications? Give an example where orientation matters.

2. **Computational**: Given two coordinate frames where frame 1 is rotated 45° about the z-axis relative to frame 0, write out the rotation matrix $R_{01}$. Verify that $R_{01}^T R_{01} = I$.

3. **Application**: Explain in your own words what each column of a rotation matrix $R_{01}$ represents geometrically. How would you construct $R_{01}$ if you know the basis vectors of frame 1 expressed in frame 0?

4. **Analysis**: If $R_{01}$ transforms from frame 1 to frame 0, what mathematical operation gives you the transformation from frame 0 to frame 1? Prove your answer using the orthonormality property.

5. **Synthesis**: A robot's camera is mounted at position $(1, 0, 0.5)$ relative to the robot base, with its optical axis pointing in the $+x$ direction of the base frame. Describe what information you would need to transform a point detected by the camera into base frame coordinates.