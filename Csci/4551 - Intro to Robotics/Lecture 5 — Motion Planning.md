# Motion Planning
**Motion planning** is the ability to go from point **A** to **B**, robots need explicit algorithms to: *Represent* the world as they see it, and *Plan* a collision-free path
## Offline V. Online Planning
- **Offline Planning:** Create a complete path before starting to move
- **Online Planning:** Plan and re-plan in real-time as you move
- Static environment → Offline planning
- Dynamic Environment → Online planning
## Visibility/Tangent Graph
Also called **Rubber band algorithm**, produces a minimum-length path from start to goal by solving a graph traversal algorithm. 
- **Assumption:** Map is known
- Connect initial and goal locations with all the visible vertices![[vgraph1.png]]
- Connect each obstacle vertex to every visible obstacle vertex![[vgraph2.png]]
- Remove edges that intersect the interior of an obstacle![[vgraph4.png]]
- Plan on the resulting Tangent graph (A* algorithm, Shortest path, etc)![[vgraph3.png]]
- **Major Faults:**
	- Robot rubs against obstacle edges
	- Doesn't take into account the size of the robot
	- In some cases, it may not find a path even if one exists → not **Complete** 
## Generalized Voronoi Diagrams
A Voronoi diagram partitions space into regions based on distance to obstacles. Each point on a Voronoi edge is equidistant from at least two obstacles.
- **Pros:**
	- Maximizes distance from obstacles
	- Reduces a 2D problem to graph search
	- Can be used in higher-dimensions
	- Provides safe paths with clearance from obstacles
- **Cons:**
	- Non-optimal path length
	- Real diagrams tend to be noisy
	- Computationally expensive for complex environments
- **Applications:**
	- Safe navigation in cluttered environments
	- Multi-robot coordination and coverage
	- Terrain analysis and medial axis extraction
	- Facility location and resource allocation problems
- **Problems:**
	- The skeleton is sensitive to small changes in the object's boundary
	- Graph isomorphism (and lots of other graph questions) → NP-complete
	- Difficult to compute incrementally for dynamic obstacles

![[Voro-diagram.png]]

> [!note] Mathematical Definition
> Let $B$ be the boundary of $C_{\text{free}}$
>
> Let $\mathbf{q}$ be a point in $C_{\text{free}}$
>
> $\text{clearance}(\mathbf{q}) = \min \{ \|\mathbf{q} - \mathbf{p}\| : \mathbf{p} \in B \}$
>
> $\text{near}(\mathbf{q}) = \{ \mathbf{p} \in B \mid \|\mathbf{q} - \mathbf{p}\| = \text{clearance}(\mathbf{q}) \}$
>
> $\mathbf{q}$ is in the **<mark style="background: #FFB86CA6;">Voronoi diagram</mark>** of $C_{\text{free}}$ if $|\text{near}(\mathbf{q})| > 1$
> **Main Idea:** if you're on the orange line, you have at least two objects that you are equally var away from.
#### Drawing a Generalized Voronoi Diagram
Assume a point robot
![[Screenshot 2025-10-11 at 4.37.18 PM.png]]
1. 
## Concerns with Planning methods
**Completeness:** Are we assured to find a path if one exists?
- **Resolution-complete:** If and only if the assurance depends on sufficient sampling
- **Probabilistically-complete:** If and only if the assurance depends on sufficient computation
###### Computational Complexity
As environments become more complex and robots have more degrees of freedom, planning becomes harder

**Examples:**
- *2D Point Robot* → Easy
- *3D Robot with Orientation* → Manageable
- *Humanoid Robot* → Challenging
###### Uncertainty 
- **Sensor Uncertainty:**
	- Robot doesn't know exact position
	- Obstacle locations are approximate
	- Goals might change/move

- **Actuator Uncertainty:**
	- Motors don't move exactly as commanded
	- Wheels slip, joints have backlash
	- External forces (wind, friction variations)
###### Dynamic Environments
By the time a plan is computed, the environment may have changed
###### Safety V Efficiency Trade-offs
- Safe paths are often longer and slower
- Efficient paths may be riskier
- Real-time constraints force compromise