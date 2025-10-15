# Simultaneous Localization and Mapping
The Simultaneous Localization and Mapping (SLAM) problem in robotics is the challenge a mobile robot faces when:
- It needs to build a map of an unknown environment
- While simultaneously keeping track of its own location within that developing map

**Challenge of SLAM:**
- Need an accurate map to know where the robot is → *Localization*
- Need an accurate location to build the map correctly → *Mapping*
- Odometry drift (errors in wheel encoders) ruin estimates
## Problems with SLAM
**Error Accumulation:** Robot's sensors & motion are inherently noisy which lead to:
1. Localization Errors
2. Mapping Errors 
###### 1. Localization Errors
The robot uses odometry or Inertial Measurements Units (IMU) to estimate movement
**these measurements**:
- contain small errors due to wheel slippage, bumpy floors, or sensor noise
- Accumulate over time, leading to a large discrepancy between the robot's estimated position & its true position
###### 2. Mapping Errors
The robot uses sensors like LiDAR, sonar, or cameras to measure distances to objects:
- Sensor readings are also noisy and imperfect
- If the robot's estimated position is wrong, the map it builds will be blurry, distorted, or incorrectly scaled
## Corrective Measures
To solve the SLAM problem the robot must fuse data from two main sources:
1. **The Motion Model**:
	- Models how the robot's [[Lecture 7 — State Estimation#^10aaa0|state]] changes over time based on its control input
	- *Input (u)*: Control signals (e.g., motor commands, velocity estimates) and odometry readings
	- *Goal*: Predict the next pose
2. **The Observation Model**:
	- Models the sensor measurements & relates them to the features in the environment
	- *Input (z)*: Sensor readings (e.g., laser scan points, visual features)
	- *Goal*: Correct the predicted pose and update the map
### Probabilistic Solution
SLAM algorithms rely on probabilistic estimation (e.g., [[Lecture 7 — State Estimation#^4dd8f1|Kalman Filters]], [[Lecture 7 — State Estimation#^50f4c6|Particle Filters]], or Graph-Based methods) to address *joint uncertainty*

**Key concepts in these solutions include**:
- *Data Association*: Determining which new sensor measurement corresponds to which previously mapped feature
- *Loop Closure*: Recognizing a previously visited location to constrain the overall accumulated error and correct the map globally
## SLAM with Gmapping
- **Gmapping**: Efficient SLAM for mobile robots
- **Method**: Based on the **Rao-Blackwellized** Particle Filter (RBPF)
- **Output**: An **Occupancy Grid Map** and the robot's estimated pose
- **Advantage**: Highly efficient and robust, especially for 2D indoor environment using laser rangefinders
#### Rao-Blackwellized Particle Filter
Breaks complex joint probability into two simpler parts:
$$P(x,m| \cdots) = P(m|x \cdots) \times P(x| \cdots) $$
Where:
- $P(m|x)$ = Map $m$ given path $x$, per particle map update
- $P(x| \cdots)$ = Robot path $x$ probability estimated using a particle filter

###### The Particle Hypotheses
A **Particle** in Gmapping is a hypothesis about both the robot's trajectory and the map of the environment. The system mains a set of N particles and each particle represents one possible solution to the SLAM problem. 

**Path Hypothesis ($x_i$):**
- A sequence of robot poses over time (trajectory)
- Each particle maintains its own belief about where the robot has been
- Updated based on motion commands and sensor observations

**Map Hypothesis ($m_i$):**
- An occupancy grid map associated with each particle
- Built incrementally as the robot explores
- Each particle's map reflects what the environment looks like given that particle's trajectory

**Differences between Hypotheses:**
- Particle filter handles the difficult, nonlinear Localization problem
- The simpler, nearly independent Mapping problem is solved deterministically for each particle

###### Predict, Update, Resample

| Step           | Action                                                                                          | Data Used            | Goal                                                            |
| -------------- | ----------------------------------------------------------------------------------------------- | -------------------- | --------------------------------------------------------------- |
| Prediction     | Move all particle according to motion model (odometry inputs)                                   | Control inputs ($u$) | Propagate the pose, increase uncertainty                        |
| Observation    | Get new sensor data (laser scan)                                                                | Sensor Reading ($z$) | Use the data to evaluate how well each particle matches reality |
| Weighting      | Calculate each particle's weight based on how well its pose aligns the new scan with its map    | $z$ and $m_i$        | Identify the most likely poses/maps                             |
| Map Refinement | Particles with high weights update and refine their own $m_i$ (filling in newly observed cells) | $z$ and $x_i$        | Build the map incrementally                                     |
| Resampling     | Duplicate high-weight particles, discard low-weight ones                                        | Weights              | Focus computational resources on the most probable hypotheses   |
## SLAM in ROS2
The standard for highly effective particle filter-based SLAM in ROS2.

**SLAM Toolbox:**
- Similar approach to Gmapping but designed for ROS2
- "sync" mode is functionally equivalent to Gmapping
- Supports both online and offline mapping
- Includes loop closure detection and map optimization
- More flexible and actively maintained than Gmapping
#### Simulate in Gazebo
Gazebo is a 3D simulator that doesn't need ROS, but has strong integration with it. The combination of ROS-Gazebo are specifically defined for ROS Jazzy; use Gazebo Harmonic.

**Need to have**:
- *Gazebo* - 3D simulation environment
- *Nav2* - Navigation framework for ROS2
- *SLAM Toolbox* - SLAM implementation for ROS2
- *Turtlebot3 packages* - Robot models and launch files


