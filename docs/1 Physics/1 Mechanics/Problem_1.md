Problem 1

Investigating the Dynamics of a Forced Damped Pendulum

# 1. Theoretical Background

A forced damped pendulum illustrates the dynamics of nonlinear oscillators. It models real systems where restoring forces, resistance (damping), and external periodic forces interact.

Governing Equation

The general motion is described by:



Where:

: angular position

: damping constant

: gravitational acceleration

: pendulum length

: driving force amplitude

: driving force frequency

Fundamental Mechanics Principles

This equation is rooted in Newton's Second Law:



In the rotational form:



The restoring torque  due to gravity is countered by damping and driven force.

The total mechanical energy of the pendulum in the absence of damping and driving is:



Linearized Approximation

Using the small-angle assumption (), the equation becomes:



This is a linear, second-order non-homogeneous differential equation, useful for analytical or numerical simulation.

# 2. Simulation in Python

We simulate three different pendulum conditions:

A frictionless simple pendulum

A damped pendulum

A driven pendulum with no damping

# Python Code

import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

def pendulum_dynamics(t, y, b, g, L, A, omega):
    theta, omega_ = y
    dtheta_dt = omega_
    domega_dt = -b * omega_ - (g / L) * np.sin(theta) + A * np.cos(omega * t)
    return [dtheta_dt, domega_dt]

# Time setup
t_range = (0, 50)
t_points = np.linspace(*t_range, 1000)
initial_state = [0.2, 0.0]

# Pendulum setups
scenarios = [
    {"label": "1) Undamped", "b": 0.0, "A": 0.0, "color": "salmon"},
    {"label": "2) Damped", "b": 0.4, "A": 0.0, "color": "slateblue"},
    {"label": "3) Driven", "b": 0.0, "A": 1.0, "color": "mediumseagreen"},
]

# Constants
g = 9.8
L = 1.0
omega_drive = 2.0

# Plotting results
fig, axes = plt.subplots(len(scenarios), 2, figsize=(12, 10))

for i, scenario in enumerate(scenarios):
    sol = solve_ivp(
        pendulum_dynamics,
        t_range,
        initial_state,
        args=(scenario["b"], g, L, scenario["A"], omega_drive),
        t_eval=t_points
    )
    theta, omega_ = sol.y
    axes[i, 0].plot(sol.t, theta, color=scenario["color"])
    axes[i, 0].set_title(f'{scenario["label"]} - Time Evolution')
    axes[i, 0].set_xlabel("Time (s)")
    axes[i, 0].set_ylabel("Angle θ (rad)")
    axes[i, 0].grid(True)

    axes[i, 1].plot(theta, omega_, color=scenario["color"])
    axes[i, 1].set_title(f'{scenario["label"]} - Phase Space')
    axes[i, 1].set_xlabel("\u03b8 (rad)")
    axes[i, 1].set_ylabel("\u03c9 (rad/s)")
    axes[i, 1].grid(True)

plt.tight_layout()
plt.show()

![alt text](<WhatsApp Image 2025-03-31 at 22.49.50_277e3b36.jpg>)
# 3. Observations

Undamped Case

Angle oscillates with constant amplitude

Phase space shows ideal ellipses

Damped Case

Oscillations decay gradually

Spiral phase diagram shows energy loss over time

Driven Case

Sustained or growing oscillations occur depending on the drive parameters

Phase diagram reveals complex periodic orbits

# 4. Summary

This model demonstrates how damping and external driving forces influence nonlinear oscillators. It is a foundational system for studying resonance, energy dissipation, and chaos.

Possible Extensions:

Investigate stronger drives leading to chaotic motion

Track total energy and dissipation over time

Explore amplitude vs. frequency relationships to analyze resonance
