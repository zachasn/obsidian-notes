# Motion Planning II - Stochastic Methods
**Stochastic Methods** algorithms use randomized methods to explore and find feasible paths. 
## Randomized Methods
- They are generally not complete but they are **probabilistically complete**
- **Based on drawing samples** → Can be on-line (samples of next motion) or off-line (samples in C-space)
## Key Concepts
- **Discretization:** Diving continuous space into subsets
- **Resolution:** How small or subsets are
- **Completeness:** if a path exists, can we find it in finite time
- **Probabilistic complete:** The probability of finding a path approaches 1 as sampling time or number of samples approaches infinity → With infinite sampling time, if a path exists, you're guaranteed to find it
- **Resolution complete:** A path will be found in finite time if one *exists at that specified resolution*
-  **Measure:** Function that assigns (positive) scalar values for subsets of some space → Can be very hard in some cases (e.g., infinite spaces)
## Random Walk
- Take a path that is sequence of random steps
- **stochastic processes:**
	- Future position of the walk depends on its *current* position and a *random*, *unpredictable* element
- Biased by where the walk is currently located
- Random methods are useful *locally* but not *globally* because they can get stuck in local minima and you can't guarantee that you will find a path to the goal
###### Properties 
1. **Randomness:** Each direction and or length of each step is chosen at random from a specific distribution
2. **Independence (Markov process):** Each step is independent of all previous steps
3.  **Law of Large Numbers:** The average position of a walk will tend to stay near the starting point, but the variance of the position will increase with the number of steps taken
4. **Central Limit Theorem:** The distribution of the position of a walk will tend to be Gaussian (normal) distribution as the number of steps increases
## RRTs
- Short for Rapidly 