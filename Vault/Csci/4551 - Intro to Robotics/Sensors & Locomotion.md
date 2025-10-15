## Overview
This lecture covers fundamental concepts in robotic sensing and locomotion systems.

## Sensors

### Types of Sensors

#### Proprioceptive Sensors
- **Definition**: Sensors that measure internal state of the robot
- **Examples**:
  - Encoders (position, velocity)
  - Accelerometers
  - Gyroscopes
  - Compass/magnetometer
- **Applications**: Self-localization, motion control

#### Exteroceptive Sensors
- **Definition**: Sensors that measure external environment
- **Examples**:
  - Cameras (visual)
  - Lidar (laser ranging)
  - Sonar/ultrasonic
  - Infrared
  - Touch/tactile sensors
- **Applications**: Environment mapping, obstacle detection, navigation

### Sensor Characteristics

#### Key Properties
- **Range**: Minimum and maximum measurable values
- **Resolution**: Smallest detectable change
- **Accuracy**: How close measurement is to true value
- **Precision**: Repeatability of measurements
- **Bandwidth**: Rate at which sensor can provide updates
- **Power consumption**
- **Size and weight**

#### Sensor Noise and Uncertainty
- **Sources of noise**:
  - Electronic noise
  - Environmental interference
  - Calibration errors
- **Dealing with uncertainty**:
  - Sensor fusion
  - Filtering techniques
  - Redundancy

## Locomotion

### Types of Locomotion
#### Wheeled Locomotion
- **Advantages**:
  - Simple control
  - Energy efficient on smooth surfaces
  - High speed capability
- **Disadvantages**:
  - Limited to smooth terrain
  - Cannot climb stairs/obstacles
- **Configurations**:
  - Differential drive
  - Ackermann steering
  - Omnidirectional wheels
#### Legged Locomotion
- **Advantages**:
  - Versatile terrain handling
  - Can climb obstacles
  - Natural for biological inspiration
- **Disadvantages**:
  - Complex control
  - Higher energy consumption
  - Stability challenges
- **Types**:
  - Bipedal (2 legs)
  - Quadrupedal (4 legs)
  - Hexapedal (6 legs)

#### Other Locomotion Methods
- **Tracked**: Tank-like treads for rough terrain
- **Flying**: Rotorcraft, fixed-wing
- **Swimming**: Underwater vehicles
- **Crawling**: Snake-like movement

### Kinematics and Dynamics

#### Forward Kinematics
- Given joint angles/wheel speeds, calculate robot position/velocity
- Mathematical relationship between actuator inputs and robot motion

#### Inverse Kinematics
- Given desired robot motion, calculate required joint angles/wheel speeds
- More complex, may have multiple solutions or no solution

#### Dynamics
- Forces and torques involved in motion
- Mass, inertia, friction effects
- Important for control system design

## Control Systems

### Basic Control Loop
1. **Sense**: Gather sensor data
2. **Plan**: Determine desired action
3. **Act**: Execute motor commands
4. **Repeat**: Continuous feedback loop

### Control Challenges
- **Real-time constraints**: Must respond quickly
- **Uncertainty**: Sensor noise, model inaccuracies
- **Disturbances**: External forces, changing environment
- **Stability**: Maintaining desired behavior

## Integration: Sensor-Based Navigation

### Obstacle Avoidance
- Use proximity sensors (sonar, lidar, IR)
- Reactive behaviors vs. planned paths
- Potential field methods

### Localization
- Determining robot's position in environment
- Dead reckoning (integrating motion)
- Landmark-based positioning
- Simultaneous Localization and Mapping (SLAM)

### Path Planning
- Finding route from start to goal
- Global planning vs. local planning
- Considering robot's locomotion constraints

## Key Takeaways
- Sensors provide critical information about robot state and environment
- Choice of locomotion method depends on application requirements
- Integration of sensing and locomotion enables autonomous behavior
- Control systems must handle uncertainty and real-time constraints

## Next Steps
- Hands-on experience with sensor programming
- Implementation of basic locomotion control
- Introduction to navigation algorithms