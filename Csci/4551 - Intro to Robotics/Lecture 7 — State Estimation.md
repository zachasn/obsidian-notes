# State Estimation
- A mobile robot moves while collecting sensor measurements from the environment
- Two steps, action and sensing:
	- **Prediction/Propagation**: What is the robots pose $x$ after action $A$
	- **Update**: Given measurement $Z$, correct the pose $x'$
- The uncertainty of the final pose that you estimate after these two steps is going to in the form of some probability density function
- The underlying math question we're asking is:
	- What is the [[Probability Distributions#^3923bf|probability density function]] that describes the *uncertainty* $P$ of the poses $x$ and $x'$?
## Bayesian Filter
- Estimate state $x$ from data $Z$ → What is the probability of the robot being at $x$
- $x$ could be robot location, map information, locations for targets, etc...
- $Z$ could be sensor readings such as range, actions, odometry from encoders, etc...
- This is a general formalism that does not depend on the particular probability representation
- Bayes filter recursively computes the Posterior Distribution (Belief):
$$Bel(x_t) = P(x_t | z_{1:t}, u_{1:t})$$
#### Bayes Filter Algorithm
The filter operates in two steps:
###### Propagate (Robot's state) using the motion model
Compute the current state estimate before taking a sensor reading by integrating over all possible previous state estimates and applying the motion model
$$\overline{Bel}(x_t) = \int P(x_t | a_{t-1}, x_{t-1}) \, Bel(x_{t-1}) \, dx_{t-1}$$
Where:
- $x_{t-1}$ is the robot's state at the previous time step (where the robot was)
- $x_t$ is the robot's state at the current time step (where the robot is now)
- $\overline{Bel}(x_t)$ is the predicted belief (probability distribution) of the current state **BEFORE** incorporating sensor measurements (the bar indicates prediction)
- $P(x_t | a_{t-1}, x_{t-1})$ is the motion model - probability of being at state $x_t$ given you were at $x_{t-1}$ and took action $a_{t-1}$
- $Bel(x_{t-1})$ is the belief (probability distribution) of where the robot was at the previous time step
###### Update using the sensor model
 Compute the current state estimate by taking a sensor reading and multiplying by the current estimate based on the most recent motion history
$$Bel(x_t) = \eta \, P(z_t | x_t) \, \overline{Bel}(x_t)$$
Where:
- $Bel(x_t)$ is the updated belief (posterior probability) after incorporating the sensor measurement
- $\eta$ is the normalization constant to ensure the probability distribution sums to 1
- $P(z_t | x_t)$ is the sensor model, probability of getting sensor reading $z_t$ if you're actually at state $x_t$ (likelihood)
- $\overline{Bel}(x_t)$ is the predicted belief from the propagate step (prior belief)

**Principle**
- Robot sees initially no clue
- Starts moving, sees landmarks, then its uncertainty goes down
- Then it keeps moving, and eventually all the crazy beliefs it had disappear and then localizes
### Problems with Bayes Filters
#### 1. Motion Model Accuracy Issues
- **Too accurate**: If $P(x_t | a_{t-1}, x_{t-1})$ has very low uncertainty, the propagated belief $\overline{Bel}(x_t)$ becomes very confident, potentially making sensor measurements less influential (don't need to observe much)
- **Too inaccurate**: High motion uncertainty means $\overline{Bel}(x_t)$ spreads out, requiring more sensor measurements to localize
#### 2. Sensor Model Problems
- **Low discriminability**: If $P(z_t | x_t)$ is similar for many different states (e.g., featureless hallway), sensors can't distinguish between locations
- **Sensor noise**: High noise makes the likelihood function flat, providing little information
#### 3. Computational Complexity
The propagation step requires integrating over all possible previous states:
$$\overline{Bel}(x_t) = \int P(x_t | a_{t-1}, x_{t-1}) \, Bel(x_{t-1}) \, dx_{t-1}$$
- This integral can be intractable for continuous state spaces
#### 4. Filter Divergence
- Accumulated errors from motion model can cause beliefs to drift from true state
- Without sufficient sensor corrections, the filter can become overconfident in wrong locations
## The Kalman Filter
- Uses a series of measurements over time, which contain statistical noise and other inaccuracies, to produce an estimate of unknown variables, meaning pose ($x$, $y$, $\theta$) that is more accurate than any single measurement
- Very useful for systems that change over time, such as tracking a moving object
- Known for its computational efficiency
- Mathematically optimal given assumptions hold:
	1. **Linear system dynamics**: System you're trying to track moves in a linear fashion (y = mx + c)
		- Measurement must be linear function of  system state 
		- State transition has to be a linear function of current state & control input
	2. **Gaussian Noise**: Uncertainty
		- Random noise in both the system and the measurements must be Gaussian (normally distributed) and have a mean of zero
- Strict assumptions fail in most real-world applications
	- Systems are often nonlinear
	- Noise may not be perfectly Gaussian
- Variations of Kalman Filter exist to address this:
	- **Extended Kalman Filter (EKF)**: Linearizes motion and measurement models using Taylor series expansion (Jacobian matrix)
	- **Unscented Kalman Filter (UKF)**: Uses deterministic sampling to capture 2nd-order non-linearity, at higher computation cost than **EKF**
#### Definitions
- **State Vector ($\hat{x}$)**: Represents the estimated state of the system (e.g., position, velocity, acceleration) → defines what you are trying to predict ^10aaa0
- **Process Covariance Matrix ($P$)**: Quantifies the uncertainty of the state estimate → tells you how confident you are about each element of your state vector and how those uncertainties relate to each other
	- Diagonal elements represent the variance of each state variable
	- Off-diagonal elements represent their covariance → how
- **State Transition Matrix ($F$)**: Defines how the state changes from one time step to the next, based on the system's dynamic model →
- **Control-Input Matrix ($B$)**: Optional matrix that relates any control inputs ($u$) to the state →
- **Process Noise Covariance ($Q$)**: Represents the uncertainty added to the system by un-modeled factors or noise in the dynamic model →
- **Measurement Vector ($z$)**: Actual measurements from a sensor
- **Measurement Matrix ($H$)**: Maps the state vector into the measurement vector's domain → "if I'm at state $x$, what sensor reading should I expect?"
- **Measurement Noise Covariance ($R$)**: Quantifies the uncertainty in the sensor measurements → how noisy/unreliable your sensors are
- **Kalman Gain ($K$)**: Determines how much the filter weighs the new measurement against its own prediction → how much to trust sensor vs. prediction
#### Examples
**State Vector ($\hat{x}$)**:
For a flying drone trying to predict its pose, state vector is $\hat{x} = [x, y, z, \phi, \theta, \psi]$
For a car, $\hat{x} = [x, y, v_x, v_y]$ - position (x,y) and velocities in both directions.

**Process Covariance Matrix ($P$)**:
For a 2D robot with state vector $\hat{x} = [x, y, \theta]$, the process covariance matrix is:
$$P = \begin{bmatrix} \sigma_x^2 & \sigma_{xy} & \sigma_{x\theta} \\ \sigma_{xy} & \sigma_y^2 & \sigma_{y\theta} \\ \sigma_{x\theta} & \sigma_{y\theta} & \sigma_\theta^2 \end{bmatrix}$$
- **Diagonal elements** ($\sigma_x^2$, $\sigma_y^2$, $\sigma_\theta^2$): These represent **individual uncertainty** in each state variable
	- $\sigma_x^2$ → how uncertain am I about x **by itself**?
	- $\sigma_y^2$ → how uncertain am I about y **by itself**?
	- $\sigma_\theta^2$ → how uncertain am I about 𝜃 **by itself**?
- **Off-diagonal elements** ($\sigma_{xy}$, $\sigma_{x\theta}$, $\sigma_{y\theta}$, $\sigma_{y\theta}$): These represent **correlation** between uncertainties
	- If my estimate of x is wrong, is my estimate of y likely to be wrong in a related way?
- Diagonal Example: $\sigma_x^2 = 0.5m^2$ (uncertainty in x-position), $\sigma_y^2 = 0.5m^2$ (uncertainty in y-position), $\sigma_\theta^2 = 0.1rad^2$ (uncertainty in heading)
- Off-diagonal Example: $\sigma_{xy}$ shows how x and y errors correlate (if robot drifts northeast, both increase together)
- This tells us: "I think I'm at position (10, 5) with heading 45°, but I'm uncertain by ±0.7m in x, ±0.7m in y, and ±0.3rad in heading"

**State Transition Matrix ($F$)**:
For a robot moving with constant velocity over time step $\Delta t$:
$$F = \begin{bmatrix} 1 & 0 & -v\Delta t \sin(\theta) \\ 0 & 1 & v\Delta t \cos(\theta) \\ 0 & 0 & 1 \end{bmatrix}$$
This predicts: new position = old position + velocity × time in the direction of heading
Example: If robot at (0, 0, 0°) moves at 1 m/s for 1 second → predicts new state (1, 0, 0°)

**Control-Input Matrix ($B$)**:
For a robot where control input $u = [v, \omega]$ (linear velocity, angular velocity):
$$B = \begin{bmatrix} \cos(\theta)\Delta t \\ \sin(\theta)\Delta t \\ \Delta t \end{bmatrix}$$
This maps: "if I command velocity $v$ and turn rate $\omega$, how does my state change?"
Example: Command $u = [0.5 m/s, 0.1 rad/s]$ for 1 sec → robot moves 0.5m forward and rotates 0.1 radians

**Process Noise Covariance ($Q$)**:
Represents uncertainty from wheel slip, unmodeled dynamics, etc.:
$$Q = \begin{bmatrix} 0.01 & 0 & 0 \\ 0 & 0.01 & 0 \\ 0 & 0 & 0.005 \end{bmatrix}$$
Example: Even if you command "drive straight 1m", reality adds noise → might actually move 1.02m ± 0.1m due to bumps, wheel slip
Larger $Q$ = "I don't trust my motion model much, motors are unreliable"

**Measurement Vector ($z$)**:
Actual sensor readings at time $t$:
For a GPS sensor: $z = [x_{GPS}, y_{GPS}]$ = [10.2, 5.1] meters
For a landmark detector: $z = [range, bearing]$ = [3.5m, 45°] to a known landmark
For an IMU: $z = [\theta_{compass}]$ = [47°]

**Measurement Matrix ($H$)**:
Maps state to expected sensor reading. For GPS measuring position only:
$$H = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \end{bmatrix}$$
Meaning: "GPS measures x and y, but not heading θ"
If state is $\hat{x} = [10, 5, 45°]$, expected measurement is $H\hat{x} = [10, 5]$

For a range sensor measuring distance to landmark at $(x_L, y_L)$:
$$H = \begin{bmatrix} \frac{x - x_L}{\sqrt{(x-x_L)^2 + (y-y_L)^2}} & \frac{y - y_L}{\sqrt{(x-x_L)^2 + (y-y_L)^2}} & 0 \end{bmatrix}$$
This computes: "if I'm at state $(x, y, \theta)$, I should measure this distance to the landmark"

**Measurement Noise Covariance ($R$)**:
Quantifies sensor reliability:
For GPS: $R = \begin{bmatrix} 2.0 & 0 \\ 0 & 2.0 \end{bmatrix}$ (GPS accurate to ±1.4m in x and y)
For laser scanner: $R = \begin{bmatrix} 0.01 & 0 \\ 0 & 0.001 \end{bmatrix}$ (very accurate, ±0.1m range, ±0.03rad bearing)
Larger $R$ = "I don't trust this sensor, it's very noisy"

**Kalman Gain ($K$)**:
Computed as: $K = P H^T (H P H^T + R)^{-1}$
Balances prediction vs. measurement:
- If $K \approx 1$: Trust sensor completely, ignore prediction
- If $K \approx 0$: Trust prediction, ignore noisy sensor

Example: You predict you're at (10, 5) with $P = 0.5m^2$ uncertainty. GPS says (10.5, 5.2) with $R = 2.0m^2$ noise.
Since your prediction is more certain than GPS, $K$ will be small (~0.2), so you only adjust a little toward the GPS reading.
Final estimate: $(10, 5) + 0.2 \times [(10.5, 5.2) - (10, 5)] = (10.1, 5.04)$
#### Kalman Filter Algorithm
Works on a **predict-update** cycle, update means update estimate *after* measurement
###### Predict
Uses the system's dynamic model to project the current state estimate and its uncertainty forward in time

**Predicted State Estimate**:
$$\hat{x}_{k|k-1} = F_k \hat{x}_{k-1|k-1} + B_k u_k$$
Where:
- $\hat{x}_{k-1}$ = the current state
- $\hat{x}_{k|k-1}$ = predicted next state at time $k$ given measurements up to the current state
- $F_k$ = state transition matrix
- $\hat{x}_{k-1|k-1}$ = previous state estimate (after update)
- $B_k$ = control-input matrix
- $u_k$ = The control input at time $k$

**Predicted Uncertainty**:
$$P_{k|k-1} = F_k P_{k-1|k-1} F_k^T + Q_k$$
Where:
- $P_{k|k-1}$ = predicted process covariance matrix
- $P_{k-1|k-1}$ = previous covariance estimate (after update)
- $F_k^T$ = transpose of state transition matrix
- $Q_k$ = process noise covariance (adds uncertainty from motion)

###### Update
Incorporates new sensor measurements to correct the predicted state estimate and reduce the uncertainty

**Kalman Gain**:
Minimizes the uncertainty of the update state
$$K_k = \frac {P_{k|k-1}H_k^T} {H_{k}P_{k|k-1}H_k^T + R_k}$$
Where:
- $P_{k|k-1}H_k^T$ = the uncertainty in the measurement
- $H_{k}P_{k|k-1}H_k^T + R_k$ = uncertainty in the measurement + uncertainty in the sensor noise (manufacture provided)
- $K_k$ = Kalman gain (optimal weighting factor)
- Determines how much to trust the measurement vs. the prediction

**The Step towards State Update**:
Corrects the predicted state ($\hat{x}_{k|k-1}$) using the new measurement ($z_k$)
$$\hat{x}_k = \hat{x}_{k|k-1} + K_{k}(z_{k}-H_{k}\hat{x}_{k|k-1})$$
Where: 
$$z_{k}-H_{k}\hat{x}_{k|k-1}$$
- Is the *measurement residual* or the *innovation term* → the difference between the actual measurement and the predicted measurement, scaled by the Kalman Gain
**Uncertainty Update**:
Updates the uncertainty matrix ($P_k$), reducing it as the new measurement provides more information

$$P_k = (I - K_{k} H_{k})P_{k|k-1}$$
Where:
- $P_{k}$ = updated uncertainty matrix
- $I$ = identity matrix
- Measurement reduces uncertainty in the estimate

### Summary
Kalman filter constantly performs two actions:
- **Predicting**: Uses the robot's motor commands to guess its new position, but also increases the uncertainty in its guess
- **Updating**: Uses the sonar measurement to correct its guess
	- If the sonar reading is very different from what was predicted, and the filter's uncertainty is high, the correction will be large
	- If the sonar is very noisy (High $R$) and the filter is very confident in its prediction (low $P$), the correction will be small
- An iterative process that allows the robot to maintain a highly accurate estimate of its pose despite having noisy sensors and un-modeled movements
## Particle Filter

^50f4c6

- Also called Monte-Carlo State Estimation, 
- Uses a *Bayesian Monte-Carlo simulation* technique for pose estimation
- Uses N samples as a discrete representation of the [[Probability Distributions#^3923bf|probability density function]] of the variable of interest:
$$S = [x_{i}, w_{i}:i=1 \cdots N]$$
Where:
- $x_i$ is a copy of the variable of interest
- $w_i$ is a weight signifying the quality of that sample
Each particle can be regarded as an **alternative hypothesis for the robot pose**
#### Particle Filter Algorithm
Operates in three stages:
###### 1. Prediction (Motion Update)
After a motion action $a_t$, each particle is moved according to the motion model
$$x_i^{[t]} = f(x_i^{[t-1]}, a_t) + \text{noise}$$
Where:
- $x_i^{[t]}$ is the predicted state of particle $i$ at time $t$
- $x_i^{[t-1]}$ is the previous state of particle $i$
- $f$ is the motion model function (e.g., kinematic equations)
- Noise is sampled from the motion noise distribution to represent uncertainty

Example: If robot moves forward 1m, each particle moves ~1m but with added noise, so they spread out

###### 2. Update (Measurement Update)
When a sensor measurement $z_t$ becomes available, the weights of the particles are updated based on how well each particle explains the measurement
$$w_i^{[t]} = w_i^{[t-1]} \cdot P(z_t | x_i^{[t]})$$
Where:
- $w_i^{[t]}$ is the updated weight of particle $i$
- $P(z_t | x_i^{[t]})$ is the measurement model (likelihood of getting measurement $z_t$ if robot is at particle $x_i$)
- Particles whose predicted measurements match actual measurements get higher weights
- Normalize weights: $w_i^{[t]} = \frac{w_i^{[t]}}{\sum_{j=1}^N w_j^{[t]}}$ so they sum to 1

Example: If particle predicts seeing landmark at 3m but sensor measures 5m, that particle gets low weight

###### 3. Resampling
Particles with higher weights are more likely to survive; low-weight particles are discarded
- Draw N new particles from current particle set with probability proportional to their weights
- Particles with high weights may be duplicated multiple times
- Particles with low weights likely disappear
- Reset all weights to $\frac{1}{N}$ after resampling

Result: Particle cloud concentrates around high-probability regions

#### Advantages of Particle Filters
- **Non-parametric**: No assumption about shape of probability distribution (unlike Kalman which assumes Gaussian)
- **Multi-modal distributions**: Can represent multiple hypotheses simultaneously (e.g., "robot could be in room A OR room B")
- **Nonlinear systems**: Works with arbitrary nonlinear motion and sensor models
- **Non-Gaussian noise**: Handles any noise distribution

#### Disadvantages of Particle Filters
- **Particle depletion**: In low-dimensional spaces or with poor motion model, particles may not cover the true state
- **Computational cost**: Requires many particles (often 1000s) for accurate representation, especially in high-dimensional spaces
- **Sample impoverishment**: After many resampling steps, diversity is lost - all particles become copies of a few good ones
- **Curse of dimensionality**: Number of particles needed grows exponentially with state dimension

#### Comparison: Kalman vs. Particle Filter
| Feature | Kalman Filter | Particle Filter |
|---------|---------------|-----------------|
| **Distribution** | Gaussian only | Any distribution |
| **System model** | Linear (or locally linear in EKF) | Any nonlinear model |
| **Computational cost** | Low (matrix operations) | High (many particles) |
| **Multi-modal** | No (single Gaussian peak) | Yes (multiple clusters) |
| **Best for** | Linear systems, unimodal, low noise | Nonlinear, multi-modal, global localization |

The updated particles represent the posterior distribution of the moving robot