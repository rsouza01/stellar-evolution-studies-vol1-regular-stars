# Module 04: Radiative transfer

**Difficulty:** Medium. **Time:** about 2 weeks.

## Goal

Understand how radiation propagates through stellar matter, from the transfer equation to the diffusion approximation in the interior and the gray atmosphere at the surface. Implement numerical solvers (formal solution, Lambda iteration, Feautrier) that you will reuse for atmospheres (Module 06) and for the surface boundary condition of the stellar models.

## Prerequisites

Modules 01 and 03.

## Reading

- **RL:** the radiative transfer chapter (specific intensity, moments, transfer equation, optical depth, Eddington-Barbier relation), plus the chapters on emission and absorption processes for later.
- **Mih, H&M:** the transfer equation, plane-parallel atmospheres, the Eddington approximation, the gray atmosphere, the Feautrier method, accelerated Lambda iteration.
- **Gray, C&O:** photospheric radiation and limb darkening.
- **KWW, HKT:** radiative transport in stellar interiors and the diffusion approximation.

## Key equations (derive)

Specific intensity $I_\nu$, absorption coefficient $\chi_\nu=\kappa_\nu\rho$, emissivity $j_\nu$ and source function $S_\nu=j_\nu/\chi_\nu$. Transfer equation and optical depth:

$$\frac{dI_\nu}{ds}=-\chi_\nu I_\nu+j_\nu,\qquad d\tau_\nu=-\chi_\nu\,dz,\qquad\mu\frac{dI_\nu}{d\tau_\nu}=I_\nu-S_\nu$$

**Formal solution** (outward, $\mu>0$):

$$I_\nu(\tau,\mu)=\int_\tau^\infty S_\nu(t)\,e^{-(t-\tau)/\mu}\,\frac{dt}{\mu}$$

and the **Eddington-Barbier relation** $I_\nu(0,\mu)\approx S_\nu(\tau_\nu=\mu)$ (exact when $S$ is linear in $\tau$).

**Moments:** $J_\nu=\tfrac12\int I_\nu d\mu$, $H_\nu=\tfrac12\int I_\nu\mu\,d\mu$, $K_\nu=\tfrac12\int I_\nu\mu^2d\mu$. In radiative equilibrium with a constant flux, and with the **Eddington approximation** $K=J/3$:

$$\frac{dK}{d\tau}=H,\qquad\frac{dH}{d\tau}=J-S$$

**Gray atmosphere.** With $\pi F=\sigma T_{\rm eff}^4$ and $J=S=B=\sigma T^4/\pi$,

$$T^4=\frac34T_{\rm eff}^4\left(\tau+\frac23\right)\ \text{(Eddington)},\qquad T^4=\frac34T_{\rm eff}^4\left(\tau+q(\tau)\right)\ \text{(exact, Hopf function)}$$

with $q(0)=1/\sqrt3\approx0.577$ and $q(\infty)\approx0.710$. The gray limb darkening law in the Eddington approximation is $I(0,\mu)/I(0,1)=\tfrac25+\tfrac35\mu$.

**Diffusion approximation** (interior, $\tau\gg1$):

$$F_{\rm rad}=-\frac{4acT^3}{3\kappa_R\rho}\frac{dT}{dr}$$

**Rosseland mean** opacity (weights the opacity where radiation transports the most flux):

$$\frac1{\kappa_R}=\frac{\int_0^\infty\kappa_\nu^{-1}(\partial B_\nu/\partial T)\,d\nu}{\int_0^\infty(\partial B_\nu/\partial T)\,d\nu}$$

**Scattering.** With a thermalization parameter $\epsilon$, $S=(1-\epsilon)J+\epsilon B$. Photons thermalize over a depth $\tau\sim1/\sqrt\epsilon$, and the surface source function is about $\sqrt\epsilon B$ for a semi-infinite atmosphere.

## Hands-on problems

**1. [E] Planck function and moments.** Implement $B_\nu(T)$, $\partial B_\nu/\partial T$ and the Stefan-Boltzmann integral. Verify $\int B_\nu d\nu=\sigma T^4/\pi$ and Wien's law numerically. Compute the Rosseland mean for $\kappa_\nu\propto\nu^{-3}$ and for a toy opacity with a line and a continuum; observe that it is dominated by the *windows* where $\kappa_\nu$ is small.

**2. [E] Formal solution.** For a plane-parallel slab with source function $S(\tau)$ given on a grid, compute $I(\tau,\mu)$ and the emergent $I(0,\mu)$ by numerical integration of the formal solution (short characteristics with exponential weights). Test with $S=a+b\tau$, for which $I(0,\mu)=a+b\mu$ exactly, and with a thin and a thick slab.

**3. [M] Gray atmosphere by Lambda iteration.** Solve for $J$ with $J=\Lambda[S]$, $S=J$ and a constant flux condition. Show that the plain Lambda iteration converges extremely slowly at depth. Then use the Eddington approximation to obtain the analytic solution and compare. Check $T(\tau=2/3)=T_{\rm eff}$ and the emergent limb darkening against $\tfrac25+\tfrac35\mu$.

**4. [M] Feautrier method.** Implement the second-order (Feautrier) form for the symmetric and antisymmetric combinations $u=(I^++I^-)/2$ with a tridiagonal solver on the $\tau$ grid, for an angle quadrature of $N$ Gauss-Legendre points. Solve the gray problem with constant flux and recover the Hopf function $q(\tau)$ and the Eddington factor $f=K/J$ (which tends to $1/3$ at depth and to $1/2$ at the surface). Plot them.

**5. [M] Scattering atmosphere.** Solve $S=(1-\epsilon)J+\epsilon B$ with a depth-constant $B$ for $\epsilon=10^{-2},10^{-4},10^{-6}$. Verify the thermalization depth $\sim\epsilon^{-1/2}$ and the emergent flux deficit $\sim\sqrt\epsilon B$ at the surface. This is the physics of strong spectral lines and resonance scattering.

**6. [M] Photon random walk.** Run a Monte Carlo of photons in a sphere with the density profile of an $n=3$ polytrope of solar mass and radius, gray opacity $\kappa=1$ cm$^2$/g (isotropic scattering). Measure the escape time distribution and compare with the diffusion estimate $t\sim3\kappa\bar\rho R^2/(\pi^2c)$. How does the answer scale with $\kappa$?

**7. [M] Accelerated Lambda iteration.** Implement an approximate Lambda operator (the diagonal operator) and show how it accelerates the scattering problem of 5 by orders of magnitude.

**8. [H] Flux-limited diffusion.** Implement a flux-limited diffusion scheme (Levermore-Pomraning limiter) in 1D. Test it on the transition from optically thick to thin in a spherical envelope and compare with Feautrier's exact solution. This is the numerical form of radiative transport in stellar winds and extended envelopes.

**9. [H] Moment hierarchy.** Derive the first two moments of the transfer equation and examine how different closures ($K=J/3$, variable Eddington factors) affect the temperature structure near the photosphere. Estimate the error in $T(\tau)$ from the Eddington closure.

## Software component

`starlite/rt/`: Planck function and derivatives, Rosseland and Planck mean routines, a formal-solution routine, a Feautrier solver for plane-parallel atmospheres, and the Hopf function. These are used in Module 06 to build the surface boundary condition.

## Checks

- Stefan-Boltzmann integral to $10^{-8}$; Wien's displacement to $10^{-6}$.
- $I(0,\mu)=a+b\mu$ for linear sources to $10^{-8}$.
- $q(0)\approx0.577$, $q(\infty)\approx0.710$, $f\to1/3$ at depth and $1/2$ at the surface.
- Thermalization-depth scaling with $\epsilon$.
- Random walk diffusion time agrees with the analytic estimate within a factor of order 2.

## Pitfalls

- Using the Planck mean where the Rosseland mean is required (and vice versa).
- Insufficient angle resolution in the Feautrier method near the surface.
- Treating $\tau$ as depth in physical space without remembering that it is frequency dependent.

## Gate questions

1. Why is the Rosseland mean the right average in the diffusion approximation?
2. What does the Eddington-Barbier relation say about where in the atmosphere you see the radiation emitted at each angle, and how does it explain limb darkening?
3. Why does plain Lambda iteration converge slowly in a scattering-dominated medium?

## Deliverable

`starlite/rt/` with tests, `notebooks/04_rt.ipynb`, solutions for problems 1 to 9.
