# Motion Planning

## Overview

Motion planning is the fundamental robotic capability of navigating from an initial configuration (point A) to a goal configuration (point B) while avoiding obstacles. This lecture explores both classical planning approaches that create complete paths before execution (offline planning) and adaptive methods that replan during motion (online planning). We examine graph-based roadmap techniques, spatial decomposition methods, and local planning approaches that form the foundation of autonomous navigation.

## Key Concepts

### What is Motion Planning?

**Motion planning** is the computational process that enables a robot to determine how to move from a starting location to a goal location. This seemingly simple task becomes complex when accounting for obstacles, robot geometry, and physical constraints.

#### Planning Paradigms

**Offline Planning** involves computing a complete path from start to goal before the robot begins moving. This approach assumes complete knowledge of the environment and produces a deterministic trajectory. The advantage is computational efficiency since planning happens once; the disadvantage is inability to handle unexpected obstacles or environmental changes during execution.

**Online Planning** computes and adjusts paths in real-time as the robot moves. The planner continuously updates the path based on new sensor information, making it robust to dynamic environments and incomplete initial knowledge. However, this requires faster computation and may produce suboptimal paths due to limited lookahead.

**Environment Types:**
- **Static environment**: Obstacles remain fixed during planning and execution
- **Dynamic environment**: Obstacles move or change, requiring replanning or reactive control

### Path Planning Assumptions

Classical path planning algorithms typically assume:

1. **Known maps**: The environment geometry is fully specified in advance
2. **Roadmaps**: The world can be represented as a graph of feasible configurations
3. **Polygonal representation**: Obstacles are modeled as polygon boundaries in 2D (or polyhedra in 3D)

These assumptions simplify the planning problem but may not hold in real-world scenarios with sensor uncertainty or unknown environments.

### Applied Graph Theory

Path planning fundamentally relies on graph representations where:

- **The world is a graph**: Physical space is discretized into a network structure
- **Vertices (nodes)** represent discrete robot configurations or locations
- **Edges** represent feasible motions between configurations
- **Graph representation** enables use of classical search algorithms (Dijkstra's, A*, etc.)

This abstraction transforms continuous motion planning into discrete graph search, which is computationally tractable.

## Roadmap Methods

Roadmap approaches create a network of collision-free paths that capture the connectivity of free space. The robot's start and goal are connected to this roadmap, and graph search finds a path along the network.

### Visibility/Tangent Graph

The **visibility graph** (also called **tangent graph**) connects configurations that can "see" each other without intersecting obstacles.

#### Construction Algorithm

1. Connect the initial location to all visible obstacle vertices
2. Connect the goal location to all visible obstacle vertices
3. Connect each obstacle vertex to every other visible obstacle vertex
4. Remove any edges that intersect obstacle interiors
5. Plan using graph search (e.g., A*) on the resulting network

The visibility graph has an elegant geometric property: optimal shortest paths consist of straight-line segments that touch obstacle corners. This is sometimes called the **"rubber band algorithm"** because the optimal path behaves like a taut rubber band stretched from start to goal around obstacles.

#### Major Fault: Point Robot Assumption

Visibility graphs assume a **point robot** (zero size). For real robots with non-zero dimensions, following visibility graph paths would cause collisions because the paths graze obstacle boundaries. This requires either:
- Growing obstacles by the robot's radius (configuration space approach)
- Post-processing paths to maintain clearance from obstacles

### Voronoi Diagrams

![[Csci4551-Lec5-Voronoi.excalidraw]]

A **Voronoi diagram** partitions space based on proximity to a set of points. For any point set $\{p_1, p_2, \ldots, p_n\}$, the Voronoi region of point $p_i$ contains all locations closer to $p_i$ than to any other point.

The edges of a Voronoi diagram are equidistant from the two nearest points. This elegant structure solves the "**Post Office Problem**": given post office locations, which post office serves each address?

#### Generalized Voronoi Diagrams

![[Csci4551-lec5-SD.excalidraw]]

For robot path planning, we generalize from point sets to obstacle boundaries. Let:

- $B$ be the boundary of free space $C_{\text{free}}$
- $\mathbf{q}$ be a point in $C_{\text{free}}$
- $\color{blue}\text{clearance}\color{white}(\mathbf{q}) = \min \{ \|\mathbf{q} - \mathbf{p}\| : \mathbf{p} \in B \}$ is the distance to the nearest obstacle
- $\color{green}\text{near}\color{white}(\mathbf{q}) = \{ \mathbf{p} \in B \mid \|\mathbf{q} - \mathbf{p}\| = \color{blue}\text{clearance}\color{white}(\mathbf{q})\}$ is the set of nearest obstacle points

A point $\mathbf{q}$ lies on the **Voronoi diagram** of $C_{\text{free}}$ if $|\color{green}\text{near}\color{white}(\mathbf{q})| > 1$ (i.e., it is equidistant from two or more obstacle boundaries).

#### Voronoi Diagrams - Evaluation

**Advantages:**
- Maximizes distance from obstacles (safer paths with clearance)
- Reduces continuous planning to discrete graph search
- Generalizes to higher dimensions

**Disadvantages:**
- Produces non-optimal (longer) paths compared to visibility graphs
- Real sensor data creates noisy diagrams with spurious branches
- Sensitive to small boundary perturbations

#### Voronoi Applications Beyond Robotics

- **3D medial surface**: The Voronoi diagram of a 3D object's surface is called the medial surface, used in shape analysis
- **2D medial axis**: In 2D, this becomes the medial axis (skeleton)
- **Skeletonization**: Results from constant-speed curve evolution inward from boundaries
- **Shape representation**: Captures topological structure of objects

#### Problems with Voronoi Skeletons

The skeleton is highly **sensitive to small boundary changes**. A tiny bump on an object's boundary creates a large branch in the skeleton, changing its topology. This instability makes Voronoi diagrams challenging for practical applications.

Additionally, many graph problems on the resulting structure (like **graph isomorphism**) are **NP-complete**, limiting computational efficiency for complex environments.

### Roadmap Recomputation Issues

A fundamental challenge: if an obstacle moves or the map changes, can we update the roadmap faster than recomputing from scratch (which takes $O(n^2)$ time for $n$ vertices)?

Incremental algorithms exist but remain computationally expensive. This motivates local planning methods that avoid global roadmap computation.

## Spatial Decomposition Methods

Rather than building roadmaps through free space, decomposition methods partition the environment into cells and use cell adjacency as the graph structure.

![[Csci4551-lec5-SD.excalidraw]]

### Exact Cell Decomposition

**Exact decomposition** divides free space into non-overlapping convex cells whose union exactly covers $C_{\text{free}}$.

#### Properties

- Each cell is **convex**, so any two points within a cell can be connected by a straight line
- Cells tile the free space with no gaps or overlaps
- An **adjacency graph** represents which cells share boundaries
- Path planning: determine which cells contain start and goal, find path through adjacency graph, connect waypoints within cells

#### Optimality Problem

Finding the decomposition with the **minimum number of convex cells** is **NP-complete**. Practical algorithms use heuristics or accept suboptimal decompositions.

### Trapezoidal Decomposition

A specific exact decomposition method:

1. Sweep a vertical line across the environment
2. At each obstacle vertex, extend vertical lines upward and downward until hitting an obstacle or boundary
3. This creates trapezoidal (or triangular) cells

**Trapezoidal decomposition** is:
- **Exact**: perfectly covers free space
- **Complete**: guaranteed to find a path if one exists
- **Not optimal**: uses more cells than the minimum convex decomposition

The simplicity of construction ($O(n \log n)$ with a sweep line algorithm) makes it practical despite suboptimality.

### Approximate Cell Decomposition

When exact decomposition is too complex, we can use **approximate methods** that sacrifice completeness for computational efficiency.

#### Quadtree Decomposition

The **quadtree** recursively subdivides space into four equal quadrants:

1. Start with a bounding box containing the environment
2. If a cell is entirely free, mark it FREE
3. If entirely occupied by obstacles, mark it OCCUPIED
4. If **mixed** (partially free, partially occupied), subdivide into four equal sub-cells
5. Recursively subdivide mixed cells to a resolution threshold

**Path planning with quadtrees:**
- Build adjacency graph of FREE cells
- Search from cell containing start to cell containing goal
- Is this **complete**? Not for arbitrary resolution—paths through narrow passages may be missed if resolution is too coarse. However, it becomes probabilistically complete as resolution increases.

**Advantages:**
- Simple to implement
- Efficient for environments with large open regions
- Natural multi-resolution representation

**Disadvantages:**
- Resolution vs. completeness tradeoff
- Many cells may be needed for complex environments
- Axis-aligned decomposition may be inefficient for diagonal obstacles

## Local Planning Techniques

Global methods (roadmaps, decomposition) require complete environment knowledge. Local techniques make planning decisions based on immediate sensor observations.

### Potential Field Methods

**Potential fields** treat the robot as a particle influenced by virtual forces:

1. **Attractive force** pulls the robot toward the goal: $F_{\text{att}}(\mathbf{q}) = -k_{\text{att}}(\mathbf{q} - \mathbf{q}_{\text{goal}})$
2. **Repulsive force** pushes away from obstacles: $F_{\text{rep}}(\mathbf{q}) = k_{\text{rep}} \sum_{\text{obstacles}} \frac{1}{d(\mathbf{q}, \text{obstacle})^2}$
3. **Control law**: Move in direction of net force $F = F_{\text{att}} + F_{\text{rep}}$

**Advantages:**
- Computable from sensor readings (local information)
- Reactive to dynamic obstacles
- Smooth, continuous control

**Disadvantages:**
- **Local minima**: Robot can get stuck in configurations where net force is zero but goal is not reached
- No completeness guarantees
- Parameter tuning ($k_{\text{att}}$, $k_{\text{rep}}$) affects behavior

### Combining Random Search with Potential Fields

To escape local minima, potential field methods are often combined with **random search**:

- When stuck in a local minimum (velocity near zero, not at goal), execute random walk
- After randomization, resume potential field control
- This hybrid approach improves practical performance but remains **suboptimal**

Random walks sacrifice efficiency for robustness, producing longer paths than necessary.

## Summary

Motion planning transforms high-level navigation goals into executable robot trajectories. Key takeaways:

1. **Graph abstraction** is fundamental—roadmaps and cell decompositions both reduce planning to graph search
2. **Visibility graphs** produce optimal shortest paths but require point robot assumption and obstacle corner contact
3. **Voronoi diagrams** maximize obstacle clearance but produce longer paths and are sensitive to boundary noise
4. **Spatial decomposition** partitions space into cells; exact methods guarantee completeness while approximate methods trade completeness for efficiency
5. **Local techniques** like potential fields work with sensor data but suffer from local minima and lack optimality guarantees

The choice of planning method depends on environment characteristics, computational resources, and whether completeness, optimality, or speed is prioritized.

## Examples and Applications

### Example 1: Visibility Graph Construction

Consider a 2D environment with two rectangular obstacles. To construct the visibility graph:

1. Extract obstacle vertices: 8 corners total
2. Add start and goal configurations
3. For each vertex pair, check line-of-sight (ray does not intersect obstacle interiors)
4. Build graph with visible pairs connected
5. Run A* search with Euclidean distance heuristic

The resulting path consists of straight segments connecting start → obstacle corners → goal, following the shortest collision-free route.

### Example 2: Quadtree Resolution

A narrow corridor 1 meter wide requires:
- Robot radius: 0.2m → minimum clearance: 0.4m
- Free corridor width: 0.6m
- Quadtree cell size must be ≤ 0.6m to represent corridor as FREE
- If initial bounding box is 64m × 64m, need at least 7 levels of subdivision (64 / 2^7 = 0.5m < 0.6m)

Insufficient resolution marks corridor as MIXED, blocking paths through it.

## Review Questions

1. **Conceptual**: Explain the tradeoff between visibility graphs and Voronoi diagrams in terms of path length and obstacle clearance. When would you prefer each method?

2. **Computational**: Why is finding the minimum convex cell decomposition NP-complete, while trapezoidal decomposition runs in $O(n \log n)$ time?

3. **Application**: A robot with 0.3m radius must navigate using a visibility graph. How should you modify the algorithm to prevent collisions?

4. **Analysis**: Why do potential field methods suffer from local minima? Sketch a simple environment where the robot would get stuck using potential fields but could reach the goal with a roadmap approach.

5. **Comparative**: Compare exact vs. approximate cell decomposition. Give a scenario where you would accept the incompleteness of quadtrees in exchange for computational efficiency.