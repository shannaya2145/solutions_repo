## Problem 2

[Simulation](Simulation_Pendulum.HTML)
## Investigating the Dynamics of a Forced Damped Pendulum

The forced damped pendulum is a classic nonlinear system that exhibits a wide range of behaviors—from simple oscillations to chaos—due to the interplay of damping, gravitational restoring forces, and external periodic driving. This exploration bridges fundamental physics with computational modeling, offering insights into both theoretical principles and real-world applications.

## 1. Theoretical Foundation

### Governing Differential Equation
The motion of a forced damped pendulum is described by a second-order nonlinear differential equation:
$$
\frac{d^2\theta}{dt^2} + b \frac{d\theta}{dt} + \frac{g}{L} \sin\theta = F \cos(\omega_d t)
$$
Where:
- $\theta$: Angular displacement (radians).
- $b$: Damping coefficient (s⁻¹).
- $g$: Gravitational acceleration (m/s²).
- $L$: Pendulum length (m).
- $F$: Driving force amplitude (s⁻²).
- $\omega_d$: Driving frequency (rad/s).

This equation balances angular acceleration, viscous damping, gravitational restoring force, and periodic forcing.

### Small-Angle Approximation
For small $\theta$, $\sin\theta \approx \theta$, simplifying the equation to a linear forced damped oscillator:
$$
\frac{d^2\theta}{dt^2} + b \frac{d\theta}{dt} + \omega_0^2 \theta = F \cos(\omega_d t)
$$
Where $\omega_0 = \sqrt{g/L}$ is the natural frequency. The general solution is:
- **Homogeneous**:
 $\theta_h(t) = A e^{-\frac{b}{2}t} \cos(\omega t + \phi)$, with $\omega = \sqrt{\omega_0^2 - (b/2)^2}$ (underdamped case).

- **Particular**: 
$\theta_p(t) = A_d \cos(\omega_d t - \delta)$, where:
  $$
  A_d = \frac{F}{\sqrt{(\omega_0^2 - \omega_d^2)^2 + (b \omega_d)^2}}, \quad \delta = \tan^{-1}\left(\frac{b \omega_d}{\omega_0^2 - \omega_d^2}\right)
  $$

- **Total**: 
$\theta(t) = \theta_h(t) + \theta_p(t)$. The transient ($\theta_h$) decays, leaving the steady-state oscillation.

### Resonance
Resonance occurs when $\omega_d \approx \omega_0$, maximizing $A_d$. For light damping ($b \ll \omega_0$), the amplitude peaks sharply, amplifying energy input from the driving force.

## 2. Analysis of Dynamics

### Parameter Influence
- **Damping ($b$)**: Reduces amplitude and prevents unbounded growth at resonance. High $b$ can suppress chaos.
- **Driving Amplitude ($F$)**: Small $F$ yields periodic motion; large $F$ can drive the system into chaos.
- **Driving Frequency ($\omega_d$)**: Near $\omega_0$, resonance amplifies motion. Far from $\omega_0$, motion may become quasiperiodic or chaotic.

### Transition to Chaos
Beyond small angles, the $\sin\theta$ nonlinearity introduces complex dynamics:
- **Periodic Motion**: At low $F$ or $\omega_d \approx \omega_0$, motion synchronizes with the drive.
- **Quasiperiodic Motion**: Multiple incommensurate frequencies emerge as $F$ increases.
- **Chaotic Motion**: High $F$ and specific $\omega_d$ lead to unpredictable, aperiodic behavior, sensitive to initial conditions.

## 3. Practical Applications
- **Energy Harvesting**: Oscillatory motion in piezoelectric devices converts mechanical energy to electricity.
- **Suspension Bridges**: External forces (wind) can induce resonant or chaotic vibrations, informing design.
- **Circuits**: Driven RLC circuits mirror this system, aiding oscillator design.

## 4. Implementation

### Python Simulation
Below is a Python script using the Runge-Kutta (RK4) method to solve the nonlinear equation and visualize the dynamics:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# Parameters
g = 9.81  # m/s²
L = 1.0   # m
omega0 = np.sqrt(g / L)

# Differential equation
def pendulum(t, y, b, F, omega_d):
    theta, theta_dot = y
    return [theta_dot, -b * theta_dot - (g / L) * np.sin(theta) + F * np.cos(omega_d * t)]

# Simulation function
def simulate_pendulum(b, F, omega_d, t_max=50, theta0=0.1, theta_dot0=0):
    t_span = (0, t_max)
    t_eval = np.linspace(0, t_max, 1000)
    sol = solve_ivp(pendulum, t_span, [theta0, theta_dot0], args=(b, F, omega_d), 
                    method='RK45', t_eval=t_eval, rtol=1e-6)
    return sol.t, sol.y[0], sol.y[1]

# Poincaré section
def poincare_section(t, theta, theta_dot, omega_d):
    period = 2 * np.pi / omega_d
    indices = np.where(np.mod(t, period) < 0.01)[0]  # Sample at drive phase
    return theta[indices], theta_dot[indices]

# Plotting
plt.figure(figsize=(15, 10))

# Cases to explore
cases = [
    {"b": 0.1, "F": 0.5, "omega_d": omega0, "label": "Resonance"},
    {"b": 0.5, "F": 1.2, "omega_d": 1.2, "label": "Moderate Forcing"},
    {"b": 0.2, "F": 1.5, "omega_d": 1.4, "label": "Chaotic"}
]

for i, case in enumerate(cases, 1):
    t, theta, theta_dot = simulate_pendulum(case["b"], case["F"], case["omega_d"])
    
    # Time series
    plt.subplot(3, 3, i)
    plt.plot(t, theta, 'b-', lw=1)
    plt.title(f"{case['label']} (b={case['b']}, F={case['F']}, ωd={case['omega_d']:.1f})")
    plt.xlabel("Time (s)")
    plt.ylabel("θ (rad)")
    
    # Phase portrait
    plt.subplot(3, 3, i + 3)
    plt.plot(theta, theta_dot, 'r-', lw=0.5)
    plt.title("Phase Portrait")
    plt.xlabel("θ (rad)")
    plt.ylabel("dθ/dt (rad/s)")
    
    # Poincaré section
    theta_p, theta_dot_p = poincare_section(t, theta, theta_dot, case["omega_d"])
    plt.subplot(3, 3, i + 6)
    plt.scatter(theta_p, theta_dot_p, s=5, c='k')
    plt.title("Poincaré Section")
    plt.xlabel("θ (rad)")
    plt.ylabel("dθ/dt (rad/s)")

plt.tight_layout()
plt.show()
```

### Output Description
- **Time Series**: Shows $\theta(t)$ for resonance (periodic), moderate forcing (quasiperiodic), and chaotic motion.
- **Phase Portraits**: Plots $\theta$ vs. $\dot{\theta}$, revealing closed loops (periodic), layered patterns (quasiperiodic), or dense filling (chaotic).
- **Poincaré Sections**: Samples at the driving period, showing points clustering (periodic), forming curves (quasiperiodic), or scattering (chaotic).

## Deliverables

### General Solutions
- **Small Angles**: Linear solution with decaying transient and steady-state oscillation.
- **Nonlinear**: Numerical solutions reveal periodic, quasiperiodic, or chaotic regimes depending on parameters.

### Graphical Representations
- **Resonance ($b=0.1, F=0.5, \omega_d=\omega_0$)**: Large, stable oscillations.
- **Moderate Forcing ($b=0.5, F=1.2, \omega_d=1.2$)**: Complex but bounded motion.
- **Chaotic ($b=0.2, F=1.5, \omega_d=1.4$)**: erratic, unpredictable swings.

### Limitations and Extensions
- **Limitations**: Assumes constant $b$, periodic forcing, and no friction irregularities.
- **Extensions**: Add nonlinear damping ($b |\dot{\theta}|$), stochastic forcing, or multi-pendulum coupling.

### Visualizations
- **Phase Portraits**: Illustrate dynamic complexity.
- **Poincaré Sections**: Highlight transitions to chaos.
- **Bifurcation Diagrams**: Plot max $\theta$ vs. $F$ or $\omega_d$ (requires additional coding).

## Conclusion
The forced damped pendulum reveals a spectrum of behaviors governed by damping, forcing amplitude, and frequency. This simulation captures its essence, from resonance to chaos, offering a window into nonlinear dynamics with broad applications.
