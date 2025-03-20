# Problem 1
[Simulation](Simulation.Projectile.HTML)

## Projectile motion is when something is thrown and moves in a curved path due to gravity. It moves forward at a steady speed and falls downward, forming a parabola. Here are the key formulas

- **Horizontal distance**: $x = v_x \cdot t$ (where $v_x$ is horizontal speed, $t$ is time)
- **Vertical height**: $y = v_y \cdot t - \frac{1}{2} g t^2$ (where $v_y$ is initial vertical speed, $g$ is 9.8 m/s²)
- **Time of flight** (for level ground): $t = \frac{2 v_y}{g}$
- **Range** (max distance): $R = \frac{v^2 \sin(2\theta)}{g}$ (where $v$ is initial speed, $\theta$ is launch angle)

The starting speed, angle, and gravity control how far and high it goes, like a thrown ball.

Below is a concise Markdown document with an embedded Python script that addresses your investigation into the range of a projectile as a function of the angle of projection. It includes the theoretical foundation, analysis, practical applications, and visualizations as requested.

---

## Investigating the Range as a Function of the Angle of Projection

## Motivation

Projectile motion combines simplicity with depth, revealing how initial conditions shape trajectories. This investigation focuses on how the range depends on the projection angle, exploring a versatile framework applicable to sports, engineering, and beyond.

## 1. Theoretical Foundation

Projectile motion arises from Newton’s laws under gravity alone. For an object launched at speed $v$ and angle $\theta$ from a flat surface:

- **Horizontal motion**: $\ddot{x} = 0$, so $x = v \cos(\theta) t$.
- **Vertical motion**: $\ddot{y} = -g$, integrating gives $y = v \sin(\theta) t - \frac{1}{2} g t^2$.

The time of flight occurs when $y = 0$:
$$0 = v \sin(\theta) t - \frac{1}{2} g t^2$$
$$t (v \sin(\theta) - \frac{1}{2} g t) = 0$$
$$t = \frac{2 v \sin(\theta)}{g}$$ (non-zero solution).

$Range$ $$R$$ is the horizontal distance at landing:

$$R = v \cos(\theta) \cdot \frac{2 v \sin(\theta)}{g} = \frac{2 v^2 \sin(\theta) \cos(\theta)}{g}$$
Using the identity $\sin(2\theta) = 2 \sin(\theta) \cos(\theta)$:
$$R = \frac{v^2 \sin(2\theta)}{g}$$

This family of solutions varies with $v$, $\theta$, and $g$, describing diverse trajectories.

## 2. Analysis of the Range

- **Angle Dependence**: $R$ peaks at $\theta = 45^\circ$ where $\sin(2\theta) = 1$, maximizing range. At $0^\circ$ or $90^\circ$, $R = 0$.
- **Other Parameters**:
  - **Initial Velocity ($v$)**: $R \propto v^2$, so doubling $v$ quadruples $R$.
  - **Gravity ($g$)**: $R \propto \frac{1}{g}$, so lower gravity (e.g., Moon) increases range.

## 3. Practical Applications

- **Sports**: Optimizing a javelin throw’s angle for maximum distance.
- **Engineering**: Artillery or rocket launches adjust for terrain or wind.
- **Astrophysics**: Trajectories on other planets with varying $g$.

## 4. Implementation

Below is a Python script simulating and visualizing the range vs. angle for different conditions.

```python
import numpy as np
import matplotlib.pyplot as plt

# Parameters
g_earth = 9.8  # m/s^2
g_moon = 1.62  # m/s^2
v1, v2 = 20, 30  # initial velocities (m/s)
angles = np.arange(0, 91, 1)  # angles from 0 to 90 degrees
theta_rad = np.radians(angles)

# Range function
def range_proj(v, theta, g):
    return (v**2 * np.sin(2 * theta)) / g

# Calculate ranges
R_v1_earth = range_proj(v1, theta_rad, g_earth)
R_v2_earth = range_proj(v2, theta_rad, g_earth)
R_v1_moon = range_proj(v1, theta_rad, g_moon)

# Plotting
plt.figure(figsize=(10, 6))
plt.plot(angles, R_v1_earth, label=f'v = {v1} m/s, g = {g_earth} m/s² (Earth)')
plt.plot(angles, R_v2_earth, label=f'v = {v2} m/s, g = {g_earth} m/s² (Earth)')
plt.plot(angles, R_v1_moon, label=f'v = {v1} m/s, g = {g_moon} m/s² (Moon)')
plt.xlabel('Angle of Projection (degrees)')
plt.ylabel('Range (meters)')
plt.title('Range vs. Angle of Projection')
plt.legend()
plt.grid(True)
plt.show()
```

### Output

- **Graph**: Shows range vs. angle for $v = 20 \, \text{m/s}$ and $30 \, \text{m/s}$ on Earth, and $v = 20 \, \text{m/s}$ on the Moon.
- **Observations**:
  - Max range at $45^\circ$.
  - Higher $v$ increases range significantly.
  - Lower $g$ (Moon) extends range dramatically.

## Discussion

### Limitations

- **Idealized Model**: Assumes no air resistance, flat terrain, and constant gravity.
- **Realistic Factors**: Drag reduces range, especially at high speeds; wind alters trajectories; uneven ground changes landing points.

### Suggestions

- **Incorporate Drag**: Add a term like $-k v$ to the equations, solved numerically.
- **Wind**: Adjust horizontal velocity with a wind component.
- **Height**: Modify equations for initial height $h$, affecting time of flight.

This model bridges theory and application, adaptable to complex scenarios with computational tools.

---

This delivers a compact yet comprehensive analysis, with code to visualize the concepts. Let me know if you’d like to expand any section!
