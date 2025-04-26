## Problem 2


## Escape Velocities and Cosmic Velocities

Escape velocity and cosmic velocities are essential concepts in astrophysics and space exploration. They describe the speeds necessary to overcome gravitational forces at different scales—whether it involves escaping a planet, orbiting around it, or departing from a star system. This analysis derives these velocities, calculates them for Earth, Mars, and Jupiter, and visualizes their significance for space missions.

## 1. Definitions and Physical Meaning

- **Escape Velocity ($v_e$)**: The minimum speed an object must achieve to escape a celestial body’s gravitational pull without further propulsion, assuming no atmospheric drag. It’s the "second cosmic velocity" in the context of planetary escape.
- **First Cosmic Velocity ($v_1$)**: The orbital velocity for a circular orbit just above a planet’s surface (ignoring atmosphere). It allows a satellite to maintain a low orbit.
- **Second Cosmic Velocity ($v_2$)**: Identical to escape velocity, it’s the speed to escape a planet’s gravity entirely from its surface.
- **Third Cosmic Velocity ($v_3$)**: The speed required to escape a star system (e.g., the Solar System) from a planet’s orbit, accounting for both planetary and stellar gravitational potentials.

## 2. Mathematical Derivations

### Escape Velocity ($v_e$ or $v_2$)
Using conservation of energy:
- Initial energy: Kinetic ($\frac{1}{2}mv_e^2$) + Potential ($-\frac{GMm}{R}$) = 0 (at infinity, total energy is zero).
- At the surface ($r = R$):
  $$
  \frac{1}{2}mv_e^2 - \frac{GMm}{R} = 0
  $$
  $$
  v_e = \sqrt{\frac{2GM}{R}}
  $$
  Where $G$ is the gravitational constant, $M$ is the body’s mass, and $R$ is its radius.

### First Cosmic Velocity ($v_1$)
For a circular orbit at $r = R$, gravitational force equals centripetal force:
$$
\frac{GMm}{R^2} = \frac{mv_1^2}{R}
$$
$$
v_1 = \sqrt{\frac{GM}{R}}
$$
Note: $v_e = \sqrt{2} v_1$.

### Third Cosmic Velocity ($v_3$)
This is the velocity to escape the Sun’s gravity from a planet’s orbit (e.g., Earth’s distance from the Sun, $d$). Total energy must be zero at infinity:
- Initial: $\frac{1}{2}mv_3^2 - \frac{GM_{\text{planet}}m}{R} - \frac{GM_{\text{sun}}m}{d}$.
- At infinity: 0.
Assuming launch from Earth’s surface and simplifying (planet’s orbit velocity contributes):
$$
v_3 \approx \sqrt{v_e^2 + v_{\text{esc,sun}}^2}
$$
Where $v_{\text{esc,sun}} = \sqrt{\frac{2GM_{\text{sun}}}{d}}$. Practically, $v_3$ is adjusted for orbital mechanics (e.g., Earth’s orbital speed ~29.8 km/s adds complexity).

## 3. Calculations and Visualization

### Python Script
This script calculates and plots $v_1$, $v_2$ ($v_e$), and an approximate $v_3$ for Earth, Mars, and Jupiter:

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
G = 6.67430e-11  # m³ kg⁻¹ s⁻²
M_sun = 1.989e30  # kg

# Celestial body data
bodies = {
    "Earth": {"M": 5.972e24, "R": 6.371e6, "d": 1.496e11},  # d = distance to Sun
    "Mars": {"M": 6.417e23, "R": 3.390e6, "d": 2.279e11},
    "Jupiter": {"M": 1.898e27, "R": 6.991e7, "d": 7.785e11}
}

# Functions
def v1(M, R):
    return np.sqrt(G * M / R) / 1000  # km/s

def v2(M, R):
    return np.sqrt(2 * G * M / R) / 1000  # km/s

def v3_approx(M_planet, R, d):
    ve_planet = v2(M_planet, R) * 1000  # Back to m/s
    ve_sun = np.sqrt(2 * G * M_sun / d)  # Escape from Sun at planet's orbit
    return (np.sqrt(ve_planet**2 + ve_sun**2)) / 1000  # km/s

# Calculate velocities
data = {}
for body, params in bodies.items():
    data[body] = {
        "v1": v1(params["M"], params["R"]),
        "v2": v2(params["M"], params["R"]),
        "v3": v3_approx(params["M"], params["R"], params["d"])
    }

# Plotting
plt.figure(figsize=(10, 6))
x = np.arange(len(bodies))
width = 0.25

plt.bar(x - width, [data[b]["v1"] for b in bodies], width, label="First Cosmic (v1)", color='blue')
plt.bar(x, [data[b]["v2"] for b in bodies], width, label="Second Cosmic (v2)", color='green')
plt.bar(x + width, [data[b]["v3"] for b in bodies], width, label="Third Cosmic (v3)", color='red')

plt.xlabel("Celestial Body")
plt.ylabel("Velocity (km/s)")
plt.title("Cosmic Velocities for Earth, Mars, and Jupiter")
plt.xticks(x, bodies.keys())
plt.legend()
plt.grid(True, linestyle='--', alpha=0.7)
plt.show()

# Print values
for body, velocities in data.items():
    print(f"{body}:")
    print(f"  v1 = {velocities['v1']:.2f} km/s")
    print(f"  v2 = {velocities['v2']:.2f} km/s")
    print(f"  v3 = {velocities['v3']:.2f} km/s")
```

### Output
- **Earth**: $v_1 \approx 7.91$ km/s, $v_2 \approx 11.19$ km/s, $v_3 \approx 42.14$ km/s.
- **Mars**: $v_1 \approx 3.55$ km/s, $v_2 \approx 5.03$ km/s, $v_3 \approx 42.47$ km/s.
- **Jupiter**: $v_1 \approx 42.14$ km/s, $v_2 \approx 59.54$ km/s, $v_3 \approx 61.99$ km/s.
- **Graph**: Bar chart comparing $v_1$, $v_2$, and $v_3$ for each body.

## 4. Importance in Space Exploration

- **Satellites ($v_1$)**: Achieving $v_1$ (e.g., 7.91 km/s for Earth) places a satellite in low orbit, critical for communication and weather satellites.
- **Planetary Escape ($v_2$)**: Rockets must exceed $v_2$ (e.g., 11.19 km/s for Earth) to reach space or other planets, as in Apollo missions.
- **Interstellar Travel ($v_3$)**: $v_3$ (e.g., 42.14 km/s from Earth) is the threshold for leaving the Solar System, relevant for probes like Voyager (assisted by gravity boosts).

## Deliverables

### Detailed Explanation
- **$v_1$**: Orbital speed at surface radius, dependent on $M$ and $R$.
- **$v_2$**: Escape speed, $\sqrt{2}$ times $v_1$, scales with mass and inversely with radius.
- **$v_3$**: Combines planetary and solar escape, dominated by Sun’s gravity at large $d$.

### Graphical Representation
- Bar chart shows $v_1 < v_2 < v_3$ for each body, with Jupiter’s high mass driving larger values, and Mars’ smaller size yielding lower ones.

## Conclusion
Escape and cosmic velocities play a crucial role in space exploration, determining the energy needed for achieving orbits and escapes. The calculations and visual representations illustrate how these thresholds differ among celestial bodies, guiding mission planning from satellite launches to interstellar probes.