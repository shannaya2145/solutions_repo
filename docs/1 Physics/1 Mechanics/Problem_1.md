## Problem 1
[Simulation](Simulation.Projectile.HTML)


## Investigating the Range as a Function of the Angle of Projection

Projectile motion is a classic problem in physics that combines simplicity with profound insights. Here, we analyze how the horizontal range of a projectile depends on its launch angle, derive the governing equations, explore parameter variations, and simulate the results computationally.

## 1. Theoretical Foundation

### Deriving the Equations of Motion
Projectile motion occurs under constant gravitational acceleration, with no initial forces other than the launch impulse. Assuming a flat surface and neglecting air resistance, we start with Newton’s second law in two dimensions (horizontal $x$ and vertical $y$):

- **Horizontal motion**: No acceleration ($a_x = 0$).
  $$
  \frac{d^2 x}{dt^2} = 0
 $$
  Initial conditions: $x(0) = 0$, $\frac{dx}{dt}(0) = v_0 \cos\theta$, where $v_0$ is the initial velocity and $\theta$ is the angle of projection.
  Solving:
  $$
  x(t) = v_0 \cos\theta \cdot t
 $$

- **Vertical motion**: Constant acceleration ($a_y = -g$), where $g$ is gravitational acceleration.
  $$
  \frac{d^2 y}{dt^2} = -g
 $$
  Initial conditions: $y(0) = 0$, $\frac{dy}{dt}(0) = v_0 \sin\theta$.
  Integrating twice:
  $$
  \frac{dy}{dt} = v_0 \sin\theta - g t
 $$
  $$
  y(t) = v_0 \sin\theta \cdot t - \frac{1}{2} g t^2
 $$

These equations describe a parabolic trajectory, parameterized by $v_0$, $\theta$, and $g$.

### Family of Solutions
The solutions form a family parameterized by initial conditions:
- $v_0$: Higher velocities stretch the parabola.
- $\theta$: Angles shift the balance between horizontal and vertical components.
- $g$: Stronger gravity compresses the trajectory vertically.
- Initial height ($h$): If $y(0) = h$, the equations adjust, affecting flight time and range.

## 2. Analysis of the Range

### Range as a Function of Angle
The range $R$ is the horizontal distance traveled when the projectile returns to $y = 0$. Set $y(t) = 0$:
$$
0 = v_0 \sin\theta \cdot t - \frac{1}{2} g t^2
$$
Factor out $t$:
$$
t (v_0 \sin\theta - \frac{1}{2} g t) = 0
$$
Solutions: $t = 0$ (start) or $t = \frac{2 v_0 \sin\theta}{g}$ (landing time). Substitute into $x(t)$:
$$
R = v_0 \cos\theta \cdot \frac{2 v_0 \sin\theta}{g} = \frac{2 v_0^2 \sin\theta \cos\theta}{g}
$$
Using the identity $2 \sin\theta \cos\theta = \sin 2\theta$:
$$
R = \frac{v_0^2 \sin 2\theta}{g}
$$
- **Maximum Range**: $R$ peaks when $\sin 2\theta = 1$, i.e., $2\theta = 90^\circ$, so $\theta = 45^\circ$. Then, $R_{\text{max}} = \frac{v_0^2}{g}$.
- **Symmetry**: $\theta$ and $90^\circ - \theta$ yield the same range (e.g., 30° and 60°).

### Influence of Parameters
- **Initial Velocity ($v_0$)**: Range scales with $v_0^2$, amplifying the effect of angle.
- **Gravity ($g$)**: Higher $g$ reduces $R$, compressing the trajectory.
- **Launch Height**: If $h > 0$, flight time increases, extending $R$. This requires solving a quadratic equation for $t$.

## 3. Practical Applications

- **Sports**: Optimizing a basketball shot or golf swing involves angle and velocity tuning.
- **Engineering**: Artillery and rocket launches adjust for terrain and air resistance.
- **Astrophysics**: Trajectories on other planets (different $g$) follow the same principles.
- **Uneven Terrain**: Adjust the landing condition (e.g., $y = h_2$) to model hills.
- **Air Resistance**: Introduce a drag term (proportional to velocity squared), requiring numerical solutions.

## 4. Implementation

Below is a Python script to simulate and visualize the range versus angle:

```python
import numpy as np
import matplotlib.pyplot as plt

def calculate_range(v0, theta_deg, g=9.81, h=0):
    """Calculate range given initial velocity, angle (degrees), gravity, and height."""
    theta = np.radians(theta_deg)
    if h == 0:
        return (v0**2 * np.sin(2 * theta)) / g
    else:
        # For h > 0, solve quadratic for time: 0 = h + v0*sin(theta)*t - (1/2)*g*t^2
        a = -0.5 * g
        b = v0 * np.sin(theta)
        c = h
        t = (-b + np.sqrt(b**2 - 4*a*c)) / (2*a)  # Positive root
        return v0 * np.cos(theta) * t

# Parameters
v0_values = [10, 15, 20]  # m/s
g_values = [9.81, 3.71]   # Earth, Mars
h_values = [0, 5]         # m
angles = np.linspace(0, 90, 91)  # 0° to 90°

# Plotting
plt.figure(figsize=(12, 8))
for v0 in v0_values:
    for g in g_values:
        for h in h_values:
            ranges = [calculate_range(v0, theta, g, h) for theta in angles]
            label = f"v0={v0} m/s, g={g} m/s², h={h} m"
            plt.plot(angles, ranges, label=label)

plt.xlabel("Angle of Projection (degrees)")
plt.ylabel("Range (meters)")
plt.title("Range vs. Angle of Projection")
plt.legend()
plt.grid(True)
plt.show()
```

### Output
This generates a plot showing $R$ versus $\theta$ for different $v_0$, $g$, and $h$. Key observations:
- Peak at 45° for $h = 0$.
- Higher $v_0$ increases range quadratically.
- Lower $g$ (e.g., Mars) extends range.
- Non-zero $h$ shifts the optimal angle below 45°.

## Limitations and Extensions
- **Idealized Model**: Assumes no air resistance, flat terrain, and constant $g$.
- **Drag**: Add $-k v^2$ terms to the differential equations, solved numerically (e.g., Runge-Kutta).
- **Wind**: Introduce a velocity-dependent force.
- **Terrain**: Model $y_{\text{ground}}(x)$ and solve numerically for landing.

## Conclusion
The range’s dependence on $\theta$ reveals a beautiful symmetry and optimization problem, with $45^\circ$ as the sweet spot under ideal conditions. Variations in parameters enrich the model, bridging theory to real-world applications. The simulation highlights these relationships vividly, inviting further exploration into complex scenarios.
