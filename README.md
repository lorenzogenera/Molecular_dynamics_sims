# 2D Molecular Dynamics Sims

A from-scratch 2D molecular dynamics engine in Python that simulates argon atoms interacting through the Lennard-Jones potential. The notebook builds the engine step by step — from a single pair of atoms to a small gas — and uses it to compare numerical integrators, recover thermodynamic quantities, and observe emergent solid/liquid/gas behavior.

Built with only numpy and matplotlib. The notebook is fully self-contained.

How to Run:
1. launch jupyter or any python interpreter
2. Run all cells top to bottom

Total run time is 2-3 minutes on laptop. 

** What's Inside:**
1. Lennard-Jones potential: implementation of $U(r)$ and $F(r)$, plus a bisection root-finder that recovers the equilibrium separation $r_{\min} = 2^{1/6}\sigma$ and the contact diameter $\sigma$ numerically.
2. Two-particle dynamics: atoms displaced slightly from equilibrium and integrated for 300 oscillation periods. Used to compare Euler vs RK2 energy conservation.
3. Velocity Verlet: symplectic integrator added to the comparison, showing bounded energy error vs RK2's drift.
4. N-particle gas: 9 atoms on a 3×3 grid with random initial velocities, bouncing inside a reflective box. Kinetic, potential, and total energy tracked over time.
5. Statistical mechanics: speed histogram from the equilibrated portion of the run, overlaid against the 2D Maxwell-Boltzmann distribution evaluated at the temperature measured from equipartition.
Phase behavior: same gas run at three temperatures (cold/warm/hot) showing solid-like, liquid-like, and gas-like configurations.
6. Pressure from wall collisions: momentum transferred to walls is accumulated and compared against the 2D ideal gas law.

# Main Physics

The pair potential:

$$U_{LJ}(r) = 4\epsilon\left[\left(\frac{\sigma}{r}\right)^{12} - \left(\frac{\sigma}{r}\right)^{6}\right]$$

Pairwise force on particle 1 from particle 2:

$$\vec{F}_1 = -\frac{dU}{dr}\cdot\frac{(\Delta x,,\Delta z)}{r}, \qquad \vec{F}_2 = -\vec{F}_1$$

Temperature from equipartition (2D, two translational degrees of freedom):

$$T = \frac{m\langle v^2\rangle}{2k_B}$$

Argon parameters used: $\sigma = 2.4$ $a_0$ , $\epsilon \approx 4 \times 10^{-4}$ $Ha$ , $m = 40\text{g/mol}$. 

The timestep is set to $dt = 0.01 \times T_{\text{osc}}$, where $T_{\text{osc}}$ is the period of small oscillations near the LJ minimum.

# Integrators Compared

1. **Euler:** per step accuracy is $O(dt)$;	Drifts upward; diverges over long runs
2. **RK2 (midpoint):** per-step accuracy is $O(dt^2)$; Small but systematic drift
3. **Velocity Verlet:** per-step accuracy is $O(dt^2)$; symplectic; Bounded oscillation around the true energy



# Key Results

1. Root-finding recovers $r_{\min}$ and $\sigma$ to within $10^{-14}\sigma$.
2. Two-particle, 300 periods: Euler's fractional energy drift is many orders of magnitude larger than RK2's. Verlet oscillates around the initial energy with no net drift.
3. N = 9 gas: the same hierarchy holds at scale — Euler diverges within the simulation window, RK2 drifts slightly, Verlet stays bounded.
4. Speed distribution follows Maxwell-Boltzmann at the measured temperature; sharper agreement would require more particles or a longer run.
5. Phase behavior: the ratio $KE / |PE|$ is a clean numerical fingerprint of phase — small for solid, near 1 for liquid, much greater than 1 for gas.
6. Pressure: simulated 2D pressure agrees with the ideal-gas prediction $P = Nk_BT/(2R)^2$ to within ~4%. The small deficit is attributable to LJ attraction reducing wall collision rates.


# Possible Extensions
1. 3D version
2. Periodic boundary conditions in place of reflective walls
3. Include Thermostats
4. Real-time animation of particle trajectories

# Author

Lorenzo Genera- physics major, at UNT
