# Problem 2

[Simulation](Simulation_2.HTML)

### Markdown Document: Estimating π Using Monte Carlo Methods

#### Objective
Monte Carlo methods leverage randomness to estimate numerical values, such as π, through geometric probability. This project implements two approaches:
1. **Circle-Based Method**: Estimate π using the ratio of random points inside a unit circle to those in a bounding square.
2. **Buffon’s Needle Method**: Estimate π based on the probability of a needle crossing parallel lines on a plane.

The simulations demonstrate how randomness can approximate π, explore convergence rates, and compare computational efficiency, connecting probability, geometry, and computation.

---

### Part 1: Circle-Based Monte Carlo Method

#### Theoretical Foundation
Consider a unit circle (radius $r = 1$) centered at the origin, inscribed in a square with side length 2 (spanning $x, y \in [-1, 1]$).
- **Area of the circle**:
  $$
  A_{\text{circle}} = \pi r^2 = \pi \cdot 1^2 = \pi
  $$
- **Area of the square**:
  $$
  A_{\text{square}} = 2 \cdot 2 = 4
  $$
- **Ratio of areas**:
  $$
  \frac{A_{\text{circle}}}{A_{\text{square}}} = \frac{\pi}{4}
  $$
If $N$ random points are uniformly distributed in the square, and $N_{\text{circle}}$ fall inside the circle (where $x^2 + y^2 \leq 1$), the ratio approximates the area ratio:
  $$
  \frac{N_{\text{circle}}}{N} \approx \frac{\pi}{4}
  $$
Thus, the estimate for π is:
  $$
  \pi \approx 4 \cdot \frac{N_{\text{circle}}}{N}
  $$

#### Simulation
- Generate $N$ random points \((x, y)\) with $x, y \sim \text{Uniform}(-1, 1)$.
- Count points where $x^2 + y^2 \leq 1$ (inside the unit circle).
- Estimate π using the formula above.
- Test with $N = 10^3, 10^4, 10^5, 10^6$.

#### Visualization
- Plot points, coloring those inside the circle (e.g., blue) and outside (e.g., red).
- Include the unit circle boundary for reference.

#### Analysis
- Compute the error $|\hat{\pi} - \pi|$ for different $N$.
- Analyze convergence rate and computational efficiency.

---

### Part 2: Buffon’s Needle Method

#### Theoretical Foundation
Buffon’s Needle problem involves dropping a needle of length $l$ onto a plane with parallel lines spaced $d$ units apart ($l \leq d$).
- **Setup**: Assume $d = 1$, $l = 1$.
- **Parameters**:
  - $x$: Distance from the needle’s center to the nearest line, $x \sim \text{Uniform}(0, \frac{d}{2}) = \text{Uniform}(0, 0.5)$.
  - $\theta$: Angle of the needle relative to the lines, $\theta \sim \text{Uniform}(0, \pi)$.
- **Crossing condition**: A needle crosses a line if:
  $$
  x \leq \frac{l}{2} \sin(\theta) = \frac{1}{2} \sin(\theta)
  $$
- **Probability of crossing**:
  $$
  P(\text{cross}) = \frac{2l}{\pi d} = \frac{2 \cdot 1}{\pi \cdot 1} = \frac{2}{\pi}
  $$
- **Derivation**:
  $$
  P(\text{cross}) = \int_0^{\pi} \int_0^{\frac{l}{2} \sin(\theta)} \frac{1}{\frac{d}{2}} \cdot \frac{1}{\pi} \, dx \, d\theta = \int_0^{\pi} \frac{\frac{l}{2} \sin(\theta)}{\frac{d}{2}} \cdot \frac{1}{\pi} \, d\theta = \frac{l}{\pi d} \int_0^{\pi} \sin(\theta) \, d\theta = \frac{l}{\pi d} \cdot 2 = \frac{2l}{\pi d}
  $$
- **Estimation**: For $N$ needle drops, with $N_{\text{cross}}$ crossings:
  $$
  \frac{N_{\text{cross}}}{N} \approx \frac{2}{\pi}
  $$
  $$
  \pi \approx \frac{2N}{N_{\text{cross}}}
  $$

#### Simulation
- Drop $N$ needles with random $x \sim \text{Uniform}(0, 0.5)$, $\theta \sim \text{Uniform}(0, \pi)$.
- Count crossings where $x \leq \frac{1}{2} \sin(\theta)$.
- Estimate π using the formula above.
- Test with $N = 10^3, 10^4, 10^5, 10^6$.

#### Visualization
- Plot needles (line segments) relative to parallel lines, coloring crossing needles differently.

#### Analysis
- Compute the error $|\hat{\pi} - \pi|$.
- Compare convergence rate with the circle-based method.

---

### Python Implementation
The script implements both methods, generates visualizations, and analyzes convergence.

```python
import numpy as np
import matplotlib.pyplot as plt

# Set random seed for reproducibility
np.random.seed(42)

# Parameters
sample_sizes = [10**3, 10**4, 10**5, 10**6]
true_pi = np.pi

# --- Circle-Based Method ---
def circle_monte_carlo(N):
    """Estimate pi using circle method and return points for plotting."""
    x = np.random.uniform(-1, 1, N)
    y = np.random.uniform(-1, 1, N)
    distances = x**2 + y**2
    inside = distances <= 1
    pi_estimate = 4 * np.sum(inside) / N
    return pi_estimate, x, y, inside

# --- Buffon's Needle Method ---
def buffon_needle(N):
    """Estimate pi using Buffon's Needle method and return needle positions."""
    x = np.random.uniform(0, 0.5, N)  # Distance to nearest line
    theta = np.random.uniform(0, np.pi, N)  # Angle
    crosses = x <= 0.5 * np.sin(theta)
    pi_estimate = 2 * N / np.sum(crosses) if np.sum(crosses) > 0 else np.inf
    return pi_estimate, x, theta, crosses

# --- Convergence Analysis ---
circle_errors = []
buffon_errors = []
for N in sample_sizes:
    # Circle method
    pi_circle, x, y, inside = circle_monte_carlo(N)
    circle_errors.append(abs(pi_circle - true_pi))
    
    # Buffon method
    pi_buffon, x_buffon, theta, crosses = buffon_needle(N)
    buffon_errors.append(abs(pi_buffon - true_pi))
    
    print(f"N={N}: Circle π={pi_circle:.4f}, Error={circle_errors[-1]:.4f} | "
          f"Buffon π={pi_buffon:.4f}, Error={buffon_errors[-1]:.4f}")

# --- Visualization: Circle Method (for N=1000) ---
pi_circle, x, y, inside = circle_monte_carlo(1000)
plt.figure(figsize=(6, 6))
plt.scatter(x[inside], y[inside], c='blue', s=10, label='Inside Circle')
plt.scatter(x[~inside], y[~inside], c='red', s=10, label='Outside Circle')
# Plot unit circle
theta = np.linspace(0, 2*np.pi, 100)
plt.plot(np.cos(theta), np.sin(theta), 'k-', label='Unit Circle')
plt.gca().set_aspect('equal')
plt.title(f'Circle Method (N=1000, π≈{pi_circle:.4f})')
plt.xlabel('x')
plt.ylabel('y')
plt.legend()
plt.show()

# --- Visualization: Buffon's Needle (for N=100) ---
pi_buffon, x, theta, crosses = buffon_needle(100)
plt.figure(figsize=(10, 4))
for i in range(100):
    x_center = x[i]
    angle = theta[i]
    x_ends = [x_center - 0.5 * np.cos(angle), x_center + 0.5 * np.cos(angle)]
    y_ends = [0.5 * np.sin(angle), -0.5 * np.sin(angle)]
    color = 'blue' if crosses[i] else 'red'
    plt.plot(x_ends, y_ends, color, alpha=0.5)
for line in [0, 1]:
    plt.axvline(line, color='black', linestyle='--')
plt.title(f'Buffon’s Needle (N=100, π≈{pi_buffon:.4f})')
plt.xlabel('x')
plt.ylabel('y')
plt.xlim(-0.5, 1.5)
plt.ylim(-0.75, 0.75)
plt.gca().set_aspect('equal')
plt.show()

# --- Convergence Plot ---
plt.figure(figsize=(8, 5))
plt.plot(sample_sizes, circle_errors, 'o-', label='Circle Method')
plt.plot(sample_sizes, buffon_errors, 's-', label='Buffon’s Needle')
plt.xscale('log')
plt.yscale('log')
plt.xlabel('Number of Points/Drops (N)')
plt.ylabel('Absolute Error |π - π̂|')
plt.title('Convergence of π Estimates')
plt.legend()
plt.grid(True)
plt.show()
```

---

### Expected Results
- **Circle Method Visualization**:
  - Scatter plot with 1000 points: blue for inside the unit circle, red for outside, with the circle boundary in black.
  - Example π estimate (varies with seed): ~3.14 for $N=1000$.
- **Buffon’s Needle Visualization**:
  - Plot of 100 needles (line segments) between lines at $x=0, 1$, with crossing needles in blue, non-crossing in red.
  - Example π estimate: ~3.2 for $N=100$.
- **Convergence Analysis**:
  - Printed output shows π estimates and errors for $N = 10^3, 10^4, 10^5, 10^6$.
  - Example (approximate, depends on seed):
    ```
    N=1000: Circle π=3.1360, Error=0.0056 | Buffon π=3.2258, Error=0.0843
    N=10000: Circle π=3.1468, Error=0.0052 | Buffon π=3.1746, Error=0.0331
    N=100000: Circle π=3.1402, Error=0.0014 | Buffon π=3.1410, Error=0.0006
    N=1000000: Circle π=3.1419, Error=0.0003 | Buffon π=3.1421, Error=0.0005
    ```
  - Log-log plot shows error decreasing with $N$, roughly as $O(1/\sqrt{N})$.

---

### Analysis
- **Convergence Rate**:
  - Both methods exhibit Monte Carlo convergence: error scales as $O(1/\sqrt{N})$, as variance of the estimate is proportional to $1/N$.
  - Circle method typically converges faster due to a higher signal-to-noise ratio (area ratio is more stable than crossing probability).
- **Accuracy**:
  - Circle method: Errors often <0.01 for $N \geq 10^5$.
  - Buffon’s Needle: Larger variance in estimates, errors may exceed 0.05 for $N = 10^3$.
- **Computational Efficiency**:
  - Circle method: Simple distance calculation ($x^2 + y^2 \leq 1$), computationally lightweight.
  - Buffon’s Needle: Requires trigonometric computation ($\sin(\theta)$), slightly slower.
  - Both scale linearly with $N$, but Buffon’s Needle has higher variance, requiring more samples for similar accuracy.

---

### Discussion
The circle-based method leverages a straightforward geometric ratio, making it intuitive and efficient. Buffon’s Needle, while elegant, is less accurate due to the lower probability of crossings ($2/\pi \approx 0.636$), leading to higher variance. Both methods confirm Monte Carlo’s power to approximate π through randomness, with the circle method being more practical for high accuracy. These simulations highlight applications in physics (e.g., particle scattering), finance (e.g., option pricing), and computer science (e.g., randomized algorithms).
