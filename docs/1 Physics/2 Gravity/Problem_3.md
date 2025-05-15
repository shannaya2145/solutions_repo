
# Problem 3
# Trajectories of a Freely Released Payload Near Earth

[Simulation](Simulation_Trajectories.HTML)

## Motivation

When a payload is released from a moving rocket near Earth, its subsequent path is influenced by its initial velocity and direction, as well as Earth's gravitational force. Analyzing these trajectories is critical for space mission operations such as satellite deployment, reentry planning, or escape trajectories.

## Types of Trajectories

- **Sub-orbital:** The object follows a curved path and reenters Earth due to insufficient speed.
- **Circular Orbit:** The object moves at just the right speed to remain in a stable orbit at a fixed altitude.
- **Elliptical Orbit:** The object has enough speed to stay in orbit but follows an elliptical path.
- **Escape Trajectory:** The object has enough speed (greater than 11.2 km/s) to escape Earth’s gravitational pull.

## Governing Equations

The motion of the payload is determined by Newton's Law of Universal Gravitation:


 $$
F=\frac{GMm}{r^2} 
$$


And the corresponding acceleration:
$$
a=\frac{GM}{R^2}
$$


Where:

- `G` is the gravitational constant (6.67430 × 10⁻¹¹ m³·kg⁻¹·s⁻²)
- `M` is Earth's mass (5.972 × 10²⁴ kg)
- `r` is the distance from Earth's center

## Numerical Simulation

Using Euler's method, the simulation updates the position and velocity of the payload over small time steps. The acceleration is recalculated at each step using the current position.

Key steps include:

1. Initialize position and velocity based on altitude, speed, and angle.
2. Calculate gravitational acceleration at each step.
3. Update velocity and position using Newtonian mechanics.
4. Terminate if the payload impacts Earth or exceeds simulation time.

## Python Code

The code simulates several scenarios to illustrate different trajectory types:

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
G = 6.67430e-11
M_earth = 5.972e24
R_earth = 6371000

# Time step and duration
dt = 1
t_max = 20000

def simulate_trajectory(v0, theta_deg, h0):
    theta = np.radians(theta_deg)
    r0 = R_earth + h0
    x, y = r0 * np.cos(0), r0 * np.sin(0)
    vx, vy = v0 * np.cos(theta), v0 * np.sin(theta)
    x_list, y_list = [], []

    for _ in np.arange(0, t_max, dt):
        r = np.sqrt(x**2 + y**2)
        if r < R_earth:
            break
        a = -G * M_earth / r**2
        ax, ay = a * x / r, a * y / r
        vx += ax * dt
        vy += ay * dt
        x += vx * dt
        y += vy * dt
        x_list.append(x)
        y_list.append(y)
    return x_list, y_list

scenarios = [
    (7500, 0, 300000),
    (7800, 45, 300000),
    (11200, 0, 300000),
    (7900, 90, 300000),
]
colors = ['b', 'g', 'r', 'purple']
labels = ['Sub-orbital', 'Elliptical', 'Escape', 'Circular']

plt.figure(figsize=(10,10))
for i, (v0, theta, h0) in enumerate(scenarios):
    x_traj, y_traj = simulate_trajectory(v0, theta, h0)
    plt.plot(x_traj, y_traj, label=labels[i], color=colors[i])

plt.gca().add_patch(plt.Circle((0,0), R_earth, color='gray', alpha=0.5))
plt.axis('equal')
plt.xlabel('x (m)')
plt.ylabel('y (m)')
plt.title('Payload Trajectories Near Earth')
plt.legend()
plt.grid(True)
plt.show()
```


# Conclusion

This analysis highlights the critical importance of initial conditions in determining the fate of a payload. Whether it reenters, orbits, or escapes Earth depends entirely on its launch velocity and angle. The numerical model provides a powerful tool to visualize and predict these paths, aiding in mission planning and orbital mechanics studies.