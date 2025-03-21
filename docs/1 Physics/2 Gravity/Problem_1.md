## Problem 1

[Simulation](Simulation_orbital.HTML)
## Orbital Period and Orbital Radius: Exploring Kepler's Third Law

Kepler's Third Law is a fundamental principle in celestial mechanics that links the square of a body’s orbital period ($T^2$) to the cube of its orbital radius ($r^3$). This relationship provides valuable insights into gravitational interactions and forms the basis of our understanding of orbits, including satellites and exoplanets. In this section, we derive this law for circular orbits, examine its implications, analyze real-world examples, and simulate it using computational methods.

## 1. Theoretical Derivation

### Circular Orbits
For a body in a circular orbit around a central mass $M$ (e.g., a planet around a star), two forces balance: the gravitational force and the centripetal force required for circular motion.

- **Gravitational Force**: 
 $$
  F_g = \frac{G M m}{r^2}
  $$
  where $G$ is the gravitational constant, $m$ is the orbiting body’s mass, and $r$ is the orbital radius.

- **Centripetal Force**: 
 $$
  F_c = \frac{m v^2}{r}
  $$
  where $v$ is the orbital velocity.

Equating these:
$$
\frac{G M m}{r^2} = \frac{m v^2}{r}
$$
Cancel $m$ (assuming $m \neq 0$) and simplify:
$$
\frac{G M}{r^2} = \frac{v^2}{r}
$$
$$
v^2 = \frac{G M}{r}
$$

The orbital period $T$ is the time for one complete orbit, related to velocity by:
$$
v = \frac{2\pi r}{T}
$$
Square this:
$$
v^2 = \frac{4\pi^2 r^2}{T^2}
$$
Substitute into the force balance:
$$
\frac{4\pi^2 r^2}{T^2} = \frac{G M}{r}
$$
Multiply through by $T^2$ and divide by $r$:
$$
4\pi^2 r^2 = \frac{G M T^2}{r}
$$
$$
T^2 = \frac{4\pi^2}{G M} r^3
$$
Thus:
$$
T^2 = k r^3
$$
where $k = \frac{4\pi^2}{G M}$ is a constant for a given central mass $M$. This is Kepler’s Third Law for circular orbits.

## 2. Implications for Astronomy

- **Planetary Masses**: If $T$ and $r$ are measured for a satellite or moon, $M$ of the central body can be calculated:
 $$
  M = \frac{4\pi^2 r^3}{G T^2}
  $$
- **Distances**: For known $M$ (e.g., the Sun), measuring $T$ determines $r$.
- **System Consistency**: The law holds across a planetary system, enabling comparisons (e.g., Earth vs. Mars orbits).

## 3. Real-World Examples

- **Moon around Earth**:
  - $r \approx 384,400$ km = $3.844 \times 10^8$ m.
  - $T \approx 27.32$ days = $2.36 \times 10^6$ s.
  - $M_{\text{Earth}} \approx 5.972 \times 10^{24}$ kg.
  - Check: $T^2 = (2.36 \times 10^6)^2 \approx 5.57 \times 10^{12}$, $r^3 = (3.844 \times 10^8)^3 \approx 5.67 \times 10^{25}$, ratio consistent with $k$.

- **Earth around Sun**:
  - $r \approx 1$ AU = $1.496 \times 10^{11}$ m.
  - $T \approx 365.25$ days = $3.156 \times 10^7$ s.
  - $M_{\text{Sun}} \approx 1.989 \times 10^{30}$ kg.
  - Verified similarly.

## 4. Implementation

### Python Simulation
This script simulates circular orbits and plots $T^2$ vs. $r^3$:

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
G = 6.67430e-11  # m³ kg⁻¹ s⁻²
M_sun = 1.989e30  # kg (Sun)
M_earth = 5.972e24  # kg (Earth)

# Orbital period function
def orbital_period(r, M):
    return np.sqrt((4 * np.pi**2 * r**3) / (G * M))

# Data for simulation
r_values = np.logspace(7, 11, 100)  # 10^7 to 10^11 m
T_sun = orbital_period(r_values, M_sun) / (24 * 3600)  # Convert to days
T_earth = orbital_period(r_values, M_earth) / (24 * 3600)

# Real-world examples
moon_r = 3.844e8  # m
moon_T = 27.32  # days
earth_r = 1.496e11  # m
earth_T = 365.25  # days

# Plotting
plt.figure(figsize=(12, 6))

# Circular orbit visualization
plt.subplot(1, 2, 1)
theta = np.linspace(0, 2 * np.pi, 100)
for r in [moon_r, earth_r / 100]:  # Scaled for visibility
    x = r * np.cos(theta)
    y = r * np.sin(theta)
    plt.plot(x, y, lw=1)
plt.plot(0, 0, 'yo', label="Central Mass")
plt.title("Circular Orbits (Scaled)")
plt.xlabel("x (m)")
plt.ylabel("y (m)")
plt.legend()
plt.axis("equal")

# T² vs r³
plt.subplot(1, 2, 2)
plt.loglog(r_values**3, T_sun**2, 'b-', label="Sun (Planets)")
plt.loglog(r_values**3, T_earth**2, 'g-', label="Earth (Satellites)")
plt.loglog([moon_r**3], [moon_T**2], 'ro', label="Moon")
plt.loglog([earth_r**3], [earth_T**2], 'ko', label="Earth")
plt.title("T² vs r³ (Kepler's Third Law)")
plt.xlabel("r³ (m³)")
plt.ylabel("T² (days²)")
plt.legend()
plt.grid(True)

plt.tight_layout()
plt.show()
```

### Output Description
- **Left Plot**: Scaled circular orbits (e.g., Moon, Earth-like) around a central mass.
- **Right Plot**: Log-log plot of $T^2$ vs. $r^3$, showing linearity (slope 1) for Sun and Earth systems, with Moon and Earth data points overlaid.

## Deliverables

### Detailed Explanation
- **Derivation**: $T^2 \propto r^3$ arises from balancing gravitational and centripetal forces.
- **Implications**: Enables mass and distance calculations, foundational for astronomy.

### Graphical Representations
- **Orbits**: Visualizes circular paths.
- **$T^2$ vs. $r^3$**: Confirms the law with real and simulated data.

### Extensions to Elliptical Orbits
Kepler’s Third Law generalizes to elliptical orbits using the semi-major axis $a$ instead of $r$:
$$
T^2 = \frac{4\pi^2}{G M} a^3
$$
This holds for planets, comets, and binary stars, with $a$ as the average distance. The simulation could be extended by parameterizing elliptical paths.

## Conclusion
Kepler’s Third Law elegantly connects the orbital period to the radius, supported by both theory and simulation. Its applications range from lunar orbits to exoplanet discovery, with extensions to elliptical orbits expanding its scope.  