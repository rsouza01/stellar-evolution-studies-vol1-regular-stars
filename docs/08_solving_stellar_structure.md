# Module 08: Solving the stellar structure equations (shooting and Henyey)

**Difficulty:** Hard. **Time:** about 4 weeks.

## Goal

Assemble the physics of Modules 02 to 07 into a working stellar structure solver. First by the classical **shooting method**, then by the **Henyey relaxation method** with a Newton iteration and a block-tridiagonal linear solver, with a mesh, boundary conditions at the centre and the surface and a convergence strategy. By the end you can compute a zero-age main-sequence model and a static solar model from a given composition profile.

## Prerequisites

Modules 01 to 07.

## Reading

- **KWW:** the boundary conditions, shooting method, the Henyey method, mesh and variables, treatment of convection in the numerical scheme.
- **HKT, Pri, Pols, CG:** stellar model construction and numerical solution.
- **NR:** relaxation methods for two-point boundary value problems, Newton-Raphson for nonlinear systems.
- **ChD:** solar models and the standard solar model (Model S) as a benchmark.
- The MESA instrument papers and manual (articles) for the architecture of a modern code.

## Key equations (derive)

The static structure equations in Lagrangian form, with $\nabla$ from the transport module (radiative, or from MLT in convective zones):

$$\frac{dr}{dm}=\frac1{4\pi r^2\rho},\quad\frac{dP}{dm}=-\frac{Gm}{4\pi r^4},\quad\frac{dL}{dm}=\epsilon_{\rm nuc}-\epsilon_\nu,\quad\frac{dT}{dm}=-\frac{GmT}{4\pi r^4P}\nabla$$

**Central boundary conditions** (series expansion near $m=0$):

$$r=\left(\frac{3m}{4\pi\rho_c}\right)^{1/3},\quad P=P_c-\frac{3G}{8\pi}\left(\frac{4\pi}{3}\rho_c\right)^{4/3}m^{2/3},\quad L=(\epsilon_{\rm nuc}-\epsilon_\nu)\,m$$

with $T$ following from the gradient.

**Surface boundary conditions** from the atmosphere module (Module 06): at the photosphere $L=4\pi R^2\sigma T_{\rm eff}^4$ with $P_s$ and $T_s$ from the chosen $T$-$\tau$ relation.

**Shooting.** Integrate outwards from the centre from guessed $(P_c,T_c)$ and inwards from the surface from guessed $(R,L)$ to a fitting point $m_f$, and adjust the four unknowns to match $(r,P,T,L)$ there by Newton-Raphson.

**Henyey method.** Discretize on a mesh of $N$ shells. At each shell the unknowns are, for example, $(\ln r,\ln T,\ln P,L)$ (or $\ln\rho$ instead of $\ln P$). The finite-difference equations form a residual vector $\mathbf R(\mathbf y)=0$ of size $4N$. Newton iteration:

$$\mathbf J\,\delta\mathbf y=-\mathbf R,\qquad\mathbf y\to\mathbf y+\lambda\,\delta\mathbf y$$

where $\lambda\in(0,1]$ is a damping factor found by a line search. The Jacobian $\mathbf J$ is **block tridiagonal** because each equation at shell $k$ couples only shells $k-1$, $k$ and $k+1$. Solve it with the block Thomas algorithm of Module 01.

**Mesh.** Use a grid in mass (or in $\ln$ of mass or of pressure near the surface) refined where variables change quickly: near the centre, at convective boundaries, in burning shells, in ionization zones and near the surface. A common criterion bounds the relative change of $(\ln P,\ln T,\ln r,L/L_{\rm tot})$ between adjacent points.

## Hands-on problems

**1. [E] Residuals for a polytrope.** Write the structure equations as a residual function $\mathbf R(\mathbf y)$ in mass coordinates and verify that the $n=3$ polytropic profile from Module 02 makes the residual small and decreasing as the mesh is refined (second-order convergence).

**2. [M] Shooting for a ZAMS star.** Set a homogeneous composition (for example $X=0.70$, $Z=0.02$), use the EOS, opacity, MLT and network from the previous modules, and the Eddington boundary condition. Find the $1\,M_\odot$ model by shooting with a fitting point at $m=0.5\,M$. Report $R$, $L$, $T_{\rm eff}$, $T_c$, $\rho_c$, the extent of the convective envelope and compare with published zero-age main-sequence values (the ZAMS Sun has $L\approx0.7\,L_\odot$, $R\approx0.89\,R_\odot$; verify against KWW or a published model).

**3. [M] The Henyey solver.** Implement the Henyey method. Start from the shooting solution or the scaled polytrope. Compute the Jacobian first by finite differences, then by automatic differentiation or by analytic derivatives from your EOS, opacity, MLT and network modules. Check convergence behaviour: quadratic near the solution, residual norm history, the number of iterations. Compare the solution with the shooting result to $10^{-6}$.

**4. [M] Mesh and convergence.** Implement the mesh criterion and a regridding routine that splits and merges zones while conserving mass, interpolating the variables conservatively. Verify that global quantities ($R$, $L$, $T_c$) converge as the number of zones increases from 100 to 2000, and estimate the order of convergence.

**5. [M] Surface boundary conditions.** Run the same $1\,M_\odot$ model with the Eddington, Hopf and tabulated atmosphere boundary conditions (Module 06). Quantify the differences in $R$ and $T_{\rm eff}$ (of order one percent).

**6. [M] Mass grid of ZAMS models.** Compute ZAMS models for $M=0.3$, $0.5$, $1$, $2$, $5$, $10$, $20\,M_\odot$ and tabulate $L$, $R$, $T_{\rm eff}$, $T_c$, $\rho_c$, the convective core mass fraction (for $M\gtrsim1.2\,M_\odot$) and the convective envelope depth. Compare the trends with the homology predictions of Module 02.

**7. [H] Static solar model.** Take the composition profile $X(m),Z(m)$ of a published standard solar model (the classic "Model S" of Christensen-Dalsgaard et al. is publicly available; verify where) and solve the static structure equations for the Sun at its present mass, luminosity and radius, adjusting $\alpha_{\rm MLT}$ and the central temperature. Compare $r(m)$, $T(m)$, $\rho(m)$, $P(m)$ and the sound speed profile $c_s(r)$ with the published model; they should agree to better than $1$ percent over most of the star. Report where and by how much they differ. This is the strongest verification of Modules 03, 05 and 06.

**8. [H] Robustness.** Make the solver robust: damped Newton with backtracking, rejection of steps that make $\rho$ or $T$ negative or the Jacobian ill-conditioned, scaling of variables so that the residuals are comparable in size, smoothing of $\nabla$ across convective boundaries, and a fallback strategy (reduce the damping, switch to finite-difference Jacobians, restart from a coarser mesh). Run a stress test: converge models from a deliberately poor initial guess.

**9. [H] Jacobian strategies.** Compare three ways of obtaining Jacobian entries: finite differences (cost and noise), analytic derivatives from each module (accuracy and effort), and automatic differentiation with JAX (cost and compatibility with table lookups). Choose one and justify with timing data on a model with 1000 zones.

## Software component

- `starlite/star/structure.py`: residual and Jacobian assembly for the Lagrangian equations with the central and surface boundary conditions.
- `starlite/star/solver.py`: damped Newton iteration with a block-tridiagonal linear solve.
- `starlite/star/mesh.py`: mesh criteria, refinement and regridding.
- `starlite/star/model.py`: the model state (a dataclass holding mesh variables, composition and derived quantities).

## Checks

- Henyey versus shooting agreement to $10^{-6}$.
- Second-order convergence with the number of zones.
- ZAMS Sun: $L\approx0.7\,L_\odot$, $R\approx0.89\,R_\odot$ (within the published tolerance), central temperature about $1.3\times10^7$ K (verify).
- Static solar model agrees with the published standard solar model to about 1 percent in $T$, $\rho$ and $c_s$ across most of the star.

## Pitfalls

- Using variables that lose precision near the centre or surface (always use logarithmic variables for $\rho,T,P,r$ where they span many decades).
- Letting the Jacobian be inconsistent with the residual (a sign error will show as slow convergence).
- Treating convective boundaries as discontinuities: the Newton iteration may oscillate across them.
- Forgetting that the central $L\to0$ and $r\to0$ boundary conditions are singular.

## Gate questions

1. Why is the Henyey method preferable to shooting for evolution calculations?
2. Why does the Jacobian have a block-tridiagonal structure and how does that make the cost linear in the number of zones?
3. What can go wrong when $\nabla$ switches between its radiative and convective forms, and how do you avoid it?

## Deliverable

`starlite/star/{structure,solver,mesh,model}.py`, a regression test for the ZAMS $1\,M_\odot$ and the static solar model, `notebooks/08_structure.ipynb`, solutions for problems 1 to 9.
