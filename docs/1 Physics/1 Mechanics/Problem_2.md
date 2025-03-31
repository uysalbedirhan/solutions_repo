# Problem 2
Problem 2

Dynamics of a Forced Damped Pendulum: A Computational Exploration

#  Motivation

The forced damped pendulum is a classic system in nonlinear dynamics. When both damping and an external periodic force are applied to a pendulum, its behavior ranges from steady oscillations to complex, even chaotic motion. This diversity mimics many real-world systems found in engineering and nature.

By varying physical parameters such as damping, force amplitude, and frequency, we can observe behaviors including resonance, phase locking, and chaos. These insights are essential for understanding oscillatory systems in fields like mechanics, electronics, and biomechanics.

# 1. Mathematical Framework

Differential Equation

The motion of the forced damped pendulum is described by a second-order nonlinear ordinary differential equation (ODE):



Where:

: angular displacement

: damping coefficient

: acceleration due to gravity

: pendulum length

: driving force amplitude

: angular frequency of the external force

Basic Physics Connection

This system obeys Newton's Second Law:



For rotational motion, this becomes:



Here, the torque  is due to gravity, damping, and the driving force.

Linear Approximation (Small Angle)

For small angular displacements:



The equation simplifies to:



This form allows for analytical approximation and is useful to analyze predictable, non-chaotic behaviors.

# 2. Parameter-Based Behavior Analysis

By varying:

Damping 

Drive amplitude 

Drive frequency 

We can observe:

Underdamped vs. overdamped responses

Resonance peaks

Onset of complex or chaotic behavior

# 3. Real-World Applications

This model represents:

Car suspension systems

RLC electrical circuits

Buildings under seismic vibrations

Human and animal gait patterns

# 4. Python Simulation

Using solve_ivp from scipy.integrate, we simulate pendulum motion for different parameter combinations.

import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

def dynamics(t, y, b, g, L, A, omega):
    theta, omega_ = y
    return [omega_, -b * omega_ - (g / L) * np.sin(theta) + A * np.cos(omega * t)]

# Initial state and setup
y0 = [0.15, 0.0]
t_range = (0, 60)
t_eval = np.linspace(*t_range, 1500)

cases = [
    {"label": "Light Damping", "b": 0.1, "A": 0.8, "omega": 1.0, "color": "royalblue"},
    {"label": "Critical Damping", "b": 0.6, "A": 0.8, "omega": 1.0, "color": "firebrick"},
    {"label": "High Drive", "b": 0.3, "A": 1.8, "omega": 3.0, "color": "darkgreen"},
]

g = 9.8
L = 1.0

fig, axes = plt.subplots(len(cases), 2, figsize=(12, 11))

for i, c in enumerate(cases):
    sol = solve_ivp(
        dynamics,
        t_range,
        y0,
        args=(c["b"], g, L, c["A"], c["omega"]),
        t_eval=t_eval
    )
    theta, omega_ = sol.y

    axes[i, 0].plot(sol.t, theta, color=c["color"])
    axes[i, 0].set_title(f'{c["label"]} - θ(t)')
    axes[i, 0].set_xlabel("Time (s)")
    axes[i, 0].set_ylabel("θ (rad)")
    axes[i, 0].grid()

    axes[i, 1].plot(theta, omega_, color=c["color"])
    axes[i, 1].set_title(f'{c["label"]} - Phase Diagram')
    axes[i, 1].set_xlabel("θ (rad)")
    axes[i, 1].set_ylabel("ω (rad/s)")
    axes[i, 1].grid()

plt.tight_layout()
plt.show()
![alt text](<WhatsApp Image 2025-03-31 at 22.47.22_5f245b09.jpg>)
# 5. Simulation Results

Light Damping

Oscillations persist over time

Phase diagram shows periodic elliptical orbits

Critical Damping

System returns to rest quickly

Oscillations vanish without overshooting

High Drive

Non-periodic, possibly chaotic motion

Phase diagram shows scattered, non-repeating behavior

# 6. Summary & Future Work

This simulation shows that the forced damped pendulum can model a wide range of physical behaviors. By modifying just a few parameters, we observe regular oscillations, damping effects, and signs of chaos.

Possible Extensions:

Create bifurcation diagrams with varying 

Introduce nonlinear damping terms

Analyze chaos through Lyapunov exponents or Poincaré maps

These tools deepen our understanding of dynamic systems in physics, engineering, and beyond.