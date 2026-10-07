# Module 09: Evolution numerics: time stepping, composition, mixing and I/O

**Difficulty:** Hard. **Time:** about 4 weeks.

## Goal

Turn the structure solver into an **evolution code**: add the time-dependent terms, evolve the composition with nuclear burning and mixing, control the time step, handle mass loss and regridding, and write histories and profiles. After this module you have the core of a MESA-like code.

## Prerequisites

Modules 01 to 08.

## Reading

- **KWW:** the evolutionary calculation: the time-dependent equations, treatment of the composition, mixing, time-step control, the gravothermal (entropy) term.
- **Pri, HKT, Pols:** numerical evolution of stellar models.
- **NR:** stiff ODEs and implicit methods.
- The MESA instrument papers and documentation (articles) for the design of a modular evolution code.

## Key equations and concepts (derive)

**Time-dependent terms.** In Lagrangian mass coordinates, at fixed $m$:

$$\frac{\partial L}{\partial m}=\epsilon_{\rm nuc}-\epsilon_\nu-T\frac{\partial s}{\partial t},\qquad\frac{\partial P}{\partial m}=-\frac{Gm}{4\pi r^4}\ \text{(hydrostatic, neglecting }\ddot r\text{)}$$

The gravothermal term $\epsilon_{\rm grav}=-T\,\partial s/\partial t$ (equivalently $-c_PT[(1-\nabla_{\rm ad}\chi_T)\partial\ln T/\partial t-\nabla_{\rm ad}\chi_\rho\partial\ln P/\partial t]$ up to conventions) is discretized with a backward difference, $\partial s/\partial t\approx(s^{n+1}-s^n)/\Delta t$ at fixed $m$. **Energy conservation test:** $\int(\epsilon_{\rm nuc}-\epsilon_\nu+\epsilon_{\rm grav})dm=L_{\rm surf}$, and $dE_{\rm tot}/dt=L_{\rm nuc}-L_\nu-L_{\rm surf}$ with $E_{\rm tot}$ the sum of internal and gravitational energy.

**Composition evolution with mixing:**

$$\frac{\partial X_i}{\partial t}\Big|_m=\left(\frac{\partial X_i}{\partial t}\right)_{\rm nuc}+\frac{\partial}{\partial m}\left[(4\pi r^2\rho)^2D\frac{\partial X_i}{\partial m}\right]$$

with $D=D_{\rm conv}+D_{\rm ov}+D_{\rm sc}+D_{\rm th}+\dots$ and $D_{\rm conv}\approx\tfrac13v_{\rm conv}\ell$. Discretize implicitly (tridiagonal in $m$ for each species).

**Coupling strategies.**
1. *Operator splitting:* solve the structure with fixed composition, then update the composition (burning plus mixing) with the new $\rho,T$, iterate to consistency.
2. *Simultaneous (fully coupled) solution:* include the composition $X_i$ (and the diffusion terms) as additional unknowns in the Newton vector. The Jacobian is still block tridiagonal with larger blocks. This is the robust approach for fast burning episodes (helium flash, thermal pulses) and is what MESA does by default.

**Time-step control.** Accept a step only if the Newton iteration converges and the changes in key variables remain below tolerances: $|\Delta\ln\rho_c|,|\Delta\ln T_c|,|\Delta\ln R|,|\Delta\ln L|$, the maximum change in the abundances (weighted by their importance), and the energy-conservation error. Grow or shrink $\Delta t$ with a controller (for example, $\Delta t_{\rm new}=\Delta t\cdot\min\{(\mathrm{tol}/\mathrm{err})^{p},f_{\max}\}$). If a step fails, *retry* with a smaller $\Delta t$ from the saved state.

**Mass loss and accretion** change the mass coordinate of the outer shells, which requires conservative remeshing.

## Hands-on problems

**1. [E] Burning at fixed structure.** Take your ZAMS $1\,M_\odot$ model and evolve only the composition at the fixed temperature and density profile with the network of Module 07, assuming a constant luminosity. Compute the time at which the central hydrogen is exhausted; compare with $t_{\rm nuc}\approx0.007\cdot0.1\cdot Mc^2/L\approx10^{10}$ yr.

**2. [M] The gravothermal term.** Implement $\epsilon_{\rm grav}$ with a backward difference in time and verify the global energy balance on a test problem: a homologously contracting $n=3$ polytrope with no nuclear burning, for which $L=-dE_{\rm tot}/dt$ and $E_{\rm tot}=\Omega/2$. Check the energy error as the time step is halved (first order in $\Delta t$).

**3. [M] Time-step control.** Implement the step controller with the tolerances above and the retry logic. Run the $1\,M_\odot$ star through the main sequence for three sets of tolerances. Plot the time step history, the number of Newton iterations per step and the number of retries. Study the convergence of the stellar age at a given $T_{\rm eff}$ as the tolerances tighten (the age shift should decrease as the tolerance decreases).

**4. [M] Operator-split evolution of the Sun.** Evolve the $1\,M_\odot$ star from the ZAMS to the end of the main sequence with the operator-split scheme. Record $L$, $R$, $T_{\rm eff}$, $T_c$, $\rho_c$, $X_c$ and the convective envelope depth versus age. Compare with a published track (MESA, MIST, PARSEC, BaSTI or the Geneva grids; verify). At the age of $4.57$ Gyr you should find $L\approx L_\odot$ (after calibration of Module 11), $R\approx R_\odot$ and $X_c\approx0.34$. The main-sequence lifetime should be about 10 Gyr.

**5. [M] Convective and overshoot mixing.** Implement the composition diffusion operator with $D$ from MLT and overshoot. Test: (a) total mass of each species conserved to $10^{-12}$ during pure mixing; (b) a convective zone homogenizes within a few turnover times; (c) different time steps produce the same result when $\Delta t$ is much larger than the turnover time (instantaneous-mixing limit). Compare the diffusion approach with a simple "instantaneous mixing" scheme.

**6. [H] Fully coupled solver.** Extend the Newton vector with the abundances and add the network and mixing Jacobian blocks. Compare robustness and cost against the operator-split scheme on (a) the main sequence, and (b) a fast episode (a shell flash, or a toy model with rapidly varying burning). Report the number of Newton iterations per step and the time-step sizes.

**7. [H] Mass loss and remeshing.** Add a prescribed mass loss rate (Reimers' law for giants; see Module 12) by moving the outer mass shells, with conservative remeshing that conserves mass, energy and the abundances of each species. Test on a model with large constant mass loss and check the global conservation laws.

**8. [H] Pre-main sequence start.** Construct a starting model on the Hayashi track (a fully convective $n=3/2$ polytrope with large radius), and evolve it to the ZAMS (Module 10 expands on this). Check the contraction timescale against $t_{\rm KH}$.

**9. [H] Software engineering of the driver.** Write `star/evolve.py` with a clean loop: `solve step -> test acceptance -> update composition -> write outputs -> choose next dt`, with hooks for extra physics (mass loss, rotation, binaries, user callbacks), checkpoint and restart, a TOML "inlist" configuration and structured outputs (HDF5 profiles with units, a history table). Add a **regression test suite** that runs short evolutions in continuous integration and compares against stored references with tolerances.

## Software component

- `starlite/star/evolve.py`: the evolution driver.
- `starlite/star/timestep.py`: time-step controller and retry logic.
- `starlite/star/mix.py`: composition mixing operator and coefficients.
- `starlite/star/io.py`: history and profile output, checkpoints.
- Configuration files (`inlists/*.toml`).

## Checks

- Energy conservation of the time-dependent terms to better than 0.1 percent integrated over the run.
- Total abundance of each species conserved to $10^{-10}$ during pure mixing.
- The $1\,M_\odot$ main-sequence lifetime of about 10 Gyr and the right trend of $R$ and $L$ along the main sequence.
- Convergence in time-step tolerances demonstrated.
- Restart from a checkpoint reproduces the continuation to machine precision.

## Pitfalls

- Allowing the time step to grow too fast after burning transitions, which drives the Newton iteration into failure.
- Evaluating the gravothermal term with a time level inconsistent with the structure.
- Lack of conservative regridding, causing spurious energy and mass changes.
- Treating the convective turnover time as much shorter than the time step without checking.

## Gate questions

1. Why do stellar evolution codes use implicit time integration even though the star evolves slowly?
2. When is operator splitting acceptable and when must the composition be solved simultaneously with the structure?
3. How does the energy equation in terms of entropy (rather than internal energy) simplify the time-dependent term?

## Deliverable

`starlite/star/{evolve,timestep,mix,io}.py`, `inlists/`, regression tests, `notebooks/09_evolution.ipynb`, solutions for problems 1 to 9.
