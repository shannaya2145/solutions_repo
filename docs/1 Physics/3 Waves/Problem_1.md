# Problem 1


### **Introduction**

This report presents an investigation into the interference patterns formed by circular water waves emitted from multiple coherent point sources positioned at the vertices of a regular polygon. The objective is to analyze and visualize the resulting wave displacement patterns on the surface due to the principle of superposition.

A square configuration was selected for this analysis, and all sources were assumed to emit waves with identical amplitude, wavelength, frequency, and initial phase. The resulting displacement field was computed and visualized using a Python simulation.

---

### **1. Theoretical Background**

The displacement of the water surface at a given point $(x, y)$ and time $t$ from a single circular wave source located at $(x_i, y_i)$ is given by:

$$
\eta_i(x, y, t) = A \cos(k r_i - \omega t)
$$

where:

* $A$ is the amplitude,

* $k = \frac{2\pi}{\lambda}$ is the wave number,

* $\omega = 2\pi f$ is the angular frequency,

* $r_i = \sqrt{(x - x_i)^2 + (y - y_i)^2}$ 

is the radial distance from the source to the point,

* and all sources are coherent (initial phase $\phi = 0$).

The total displacement at a point due to $N$ sources is the sum of the individual displacements:

$$
\eta(x, y, t) = \sum_{i=1}^{N} \eta_i(x, y, t)
$$

---

### **2. Source Configuration**

A regular square was chosen as the polygonal configuration. The square is centered at the origin and has a side length of $2a$. The coordinates of the four sources are:

* $S_1 = (-a, -a)$
* $S_2 = (-a, a)$
* $S_3 = (a, a)$
* $S_4 = (a, -a)$

For simulation purposes, $a = 1.0$ was used.

---

### **3. Simulation Method**

A 2D spatial grid was created to represent the water surface. For each point on the grid, the total displacement $\eta(x, y, t)$ was calculated by summing the contributions from all four sources. A snapshot at a fixed time $t = 0$ was used to generate a contour plot of the displacement field.

The Python code used for the simulation is shown below:

```python
import numpy as np
import matplotlib.pyplot as plt

# Parameters
A = 1.0           # Amplitude
λ = 1.0           # Wavelength
f = 1.0           # Frequency
k = 2 * np.pi / λ # Wavenumber
ω = 2 * np.pi * f # Angular frequency
a = 1.0           # Half-side of square

# Time snapshot
t = 0

# Grid setup
x = np.linspace(-3, 3, 500)
y = np.linspace(-3, 3, 500)
X, Y = np.meshgrid(x, y)

# Source positions
sources = [(-a, -a), (-a, a), (a, a), (a, -a)]

# Superposition of waves
eta = np.zeros_like(X)
for (x0, y0) in sources:
    R = np.sqrt((X - x0)**2 + (Y - y0)**2)
    eta += A * np.cos(k * R - ω * t)

# Plotting
plt.figure(figsize=(8, 6))
plt.contourf(X, Y, eta, levels=100, cmap='viridis')
plt.colorbar(label='Displacement η(x, y, t)')
plt.title('Interference Pattern from 4 Wave Sources (Square Configuration)')
plt.xlabel('x')
plt.ylabel('y')
plt.axis('equal')
plt.show()
```

---

### **4. Results and Discussion**

The resulting interference pattern exhibits symmetrical regions of constructive and destructive interference due to the coherent interaction of the waves. Key observations include:

* **Constructive interference** occurs at points where wavefronts from the sources arrive in phase, leading to higher amplitude regions (shown as bright bands or peaks).
* **Destructive interference** arises where the waves arrive out of phase, resulting in cancellation (dark nodes or lines).
* The overall pattern demonstrates **square symmetry**, reflecting the geometry of the source arrangement.

The simulation effectively illustrates the spatial periodicity and complexity of interference patterns created by multiple circular wave sources.

---

### **5. Conclusion**

This study successfully demonstrates the use of mathematical modeling and numerical simulation to analyze the interference of circular waves from sources arranged in a regular polygon. The square configuration produced predictable, symmetric patterns, confirming theoretical expectations based on wave superposition.

The methodology can be extended to other polygonal arrangements (e.g., triangle, pentagon) or adapted to include phase shifts, damping, and boundary effects for more realistic scenarios.

