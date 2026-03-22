# Oscillations — Numerical Simulation

A classical oscillations simulator built in Python, developed as a class project for Classical Mechanics. Explores the nonlinear pendulum and damped harmonic oscillator across multiple physical regimes using the Velocity Verlet integration scheme. The project also benchmarks Velocity Verlet against the small-angle approximation and the Runge-Kutta (RK45) method via `scipy`.

---

## Models & Scope

The notebook is organized into three main studies:

**Nonlinear Pendulum** — Angular displacement $\varphi(t)$ of a simple pendulum on an elliptical path of radius $R = 5$ m, solved numerically under the full $\sin(\varphi)$ equation of motion. A user-supplied initial angle $\varphi_0 \in (0, \frac{\pi}{2}]$ governs the simulation. Results are compared against the small-angle approximation $\varphi(t) \approx \varphi_0 \cos(\omega t)$, with printed feedback on the accuracy of the approximation depending on the magnitude of $\varphi_0$.

**Damped Harmonic Oscillator (Free)** — A unit mass-spring system ($m = 1$ kg, $k = 1$ N/m, $\omega_0 = 1$ Hz) simulated across four damping regimes: no damping, underdamping ($\beta = 0.05$), overdamping ($\beta = 2$), and critical damping ($\beta = \omega_0$). Both position $x(t)$ and total mechanical energy $E(t) = T + U$ are tracked and plotted for each regime.

**Driven Damped Harmonic Oscillator** — The same four damping regimes subjected to a sinusoidal driving force $f(t) = f_0 \cos(\omega t)$ at resonance ($\omega = \omega_0 = 1$ Hz) with amplitude $f_0 = 10$. Explores how driving at the natural frequency produces qualitatively different behavior across regimes — resonance growth in the undamped/underdamped cases, and pseudo-harmonic steady-state motion in the over- and critically-damped cases.

---

## Numerical Methods

### Velocity Verlet (Primary Integrator)

All trajectories are computed using the **Velocity Verlet algorithm**, a second-order symplectic integrator well-suited for systems with position- and velocity-dependent forces. Each time step proceeds as:

1. $x(t + \Delta t) = x(t) + \Delta t \, x'(t) + \frac{\Delta t^2}{2} x''(t)$
2. $x''(t + \Delta t) = F\bigl(x(t + \Delta t),\, x'(t)\bigr) / m$
3. $x'(t + \Delta t) = x'(t) + \frac{\Delta t}{2}\bigl(x''(t + \Delta t) + x''(t)\bigr)$

Time step $\Delta t = 0.1$ s for the pendulum study; $\Delta t = 0.01$ s for the oscillator studies. All simulations run over $t_f = 20\pi$ s (10 complete natural periods).

### Runge-Kutta (Benchmark)

The pendulum system is additionally solved with **SciPy's `solve_ivp`** using the default RK45 scheme at $\Delta t = 0.001$ s, serving as a high-accuracy benchmark. Results are plotted alongside the Velocity Verlet solution to assess numerical agreement over long integration windows.

---

## System Parameters

### Nonlinear Pendulum

| Parameter | Value |
|---|---|
| Path radius | $R = 5$ m |
| Gravitational acceleration | $g = 9.8$ m/s² |
| Natural frequency | $\omega = \sqrt{g/R}$ |
| Initial angular velocity | $\varphi'_0 = 0$ |
| Initial angle | $\varphi_0 \in (0°, 90°]$ (user input) |

### Mass-Spring Oscillator

| Parameter | Value |
|---|---|
| Mass | $m = 1$ kg |
| Spring constant | $k = 1$ N/m |
| Natural frequency | $\omega_0 = \sqrt{k/m} = 1$ Hz |
| Initial position | $x_0 = 1$ m |
| Initial velocity | $x'_0 = 0$ |
| Driving force amplitude | $f_0 = 10$ (driven cases) |
| Driving frequency | $\omega = \omega_0 = 1$ Hz |

---

## Outputs

**Part a) — Free Oscillator**
- Four-panel position plots: no damping, underdamping, overdamping, critical damping (separate and overlaid)
- Four-panel energy plots: same regimes

**Part b) — Driven Oscillator**
- Four-panel position plots across damping regimes under resonant driving
- Four-panel energy plots; overdamped and critically-damped energy shown separately due to scale disparity
- Overlaid position comparison

**Part c) — RK45 vs. Velocity Verlet**
- Direct overlay of Runge-Kutta and Velocity Verlet pendulum trajectories at $\varphi_0 = \pi/2$

| Color/Style | Regime |
|---|---|
| 🟢 Green solid | No damping |
| 🔵 Blue dotted | Underdamping |
| 🟣 Magenta dashed | Overdamping |
| 🔴 Red solid | Critical damping |

---

## Notes

- The full nonlinear equation of motion $\varphi'' = -(g/R)\sin(\varphi)$ is used throughout — the small-angle linearization $\sin(\varphi) \approx \varphi$ is only used in the analytic comparison, not in the integrator.
- The small-angle approximation $\varphi_0 \cos(\omega t)$ begins to noticeably lag the Velocity Verlet solution for $\varphi_0 > 20°$, and the discrepancy grows as $\varphi_0 \to 90°$ and $t \to \infty$.
- Under resonant driving ($\omega = \omega_0$), the undamped and underdamped oscillators exhibit secular growth in both amplitude and energy rather than steady-state harmonic motion. Over- and critically-damped oscillators converge to pseudo-harmonic motion at amplitudes governed by the balance between driving and dissipation.
- `scipy.integrate.solve_ivp` is imported but used only in Part c). The `numpy` import in the oscillator cells is present but not actively used in the Velocity Verlet loops.

---

## Dependencies

```
numpy
matplotlib
scipy
```
