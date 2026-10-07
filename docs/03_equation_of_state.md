# Module 03: The equation of state

**Difficulty:** Medium. **Time:** about 2 weeks.

## Goal

Build a thermodynamically consistent equation of state (EOS) for stellar matter that covers ideal gas, radiation, partial ionization, arbitrary electron degeneracy (relativistic or not) and Coulomb corrections, together with all the thermodynamic derivatives the structure equations need.

## Prerequisites

Modules 01 and 02. Statistical mechanics (Fermi-Dirac statistics).

## Reading

- **KWW, HKT, CG:** the equation of state of stellar matter: ideal gas, radiation, mean molecular weight, ionization, degeneracy, Coulomb interactions, thermodynamic derivatives and adiabatic gradients.
- **ST, Cha:** degenerate electron gas, white dwarf EOS.
- **Cla:** stellar EOS and its role in structure.
- Landau and Lifshitz, *Statistical Physics*, for the thermodynamics (optional).

## Key equations (derive)

**Mean molecular weights** for fully ionized matter with mass fractions $X_i$, atomic mass numbers $A_i$ and charges $Z_i$:

$$\frac{1}{\mu_{\rm ion}}=\sum_i\frac{X_i}{A_i},\qquad\frac{1}{\mu_e}=\sum_i\frac{X_iZ_i}{A_i},\qquad\frac1\mu=\frac{1}{\mu_{\rm ion}}+\frac1{\mu_e}$$

For hydrogen-helium mixtures $1/\mu\approx2X+0.75Y+0.5Z$. At the solar surface $\mu\approx0.6$ and at the solar centre $\mu\approx0.85$ (check).

**Ideal gas and radiation:**

$$P=\frac{\rho k_BT}{\mu m_u}+\frac{aT^4}{3},\qquad u=\frac{3}{2}\frac{k_BT}{\mu m_u}\rho+aT^4$$

**Thermodynamic derivatives.** Define $\chi_\rho=(\partial\ln P/\partial\ln\rho)_T$, $\chi_T=(\partial\ln P/\partial\ln T)_\rho$, $\delta=-(\partial\ln\rho/\partial\ln T)_P=\chi_T/\chi_\rho$ and the specific heats $c_V$, $c_P$. Then (derive)

$$c_P-c_V=\frac{P}{\rho T}\frac{\chi_T^2}{\chi_\rho},\qquad\Gamma_1=\chi_\rho+\frac{P}{\rho Tc_V}\chi_T^2,\qquad\nabla_{\rm ad}=\frac{P\delta}{\rho Tc_P}=\frac{\Gamma_2-1}{\Gamma_2}$$

Limits: $\nabla_{\rm ad}=0.4$ for an ideal monatomic gas and $0.25$ for radiation.

**Saha equation** for ionization stage $i\to i+1$:

$$\frac{n_{i+1}n_e}{n_i}=\frac{2g_{i+1}}{g_i}\left(\frac{2\pi m_ek_BT}{h^2}\right)^{3/2}e^{-\chi_i/k_BT}$$

**Electron degeneracy.** With $F_k(\eta)$ the Fermi-Dirac integrals of order $k$ and $\eta=\mu_e/k_BT$ the degeneracy parameter, the non-relativistic electron number density and pressure are $n_e\propto T^{3/2}F_{1/2}(\eta)$ and $P_e\propto T^{5/2}F_{3/2}(\eta)$. The fully degenerate limits (to reproduce):

$$P_e\approx1.0036\times10^{13}\left(\frac{\rho}{\mu_e}\right)^{5/3}\ {\rm (non\text{-}rel.)},\qquad P_e\approx1.2435\times10^{15}\left(\frac{\rho}{\mu_e}\right)^{4/3}\ {\rm (ultra\text{-}rel.)}$$

The general zero-temperature result (Chandrasekhar), with $x=p_F/m_ec=(\rho/\mu_eB)^{1/3}$, $B\approx9.74\times10^5$ g/cm$^3$ and $A=\pi m_e^4c^5/(3h^3)\approx6.0\times10^{22}$ dyn/cm$^2$:

$$P_e=A\left[x(2x^2-3)\sqrt{1+x^2}+3\sinh^{-1}x\right]$$

**Coulomb corrections.** The plasma coupling parameter $\Gamma_C=Z^2e^2/(a_ik_BT)$ (with $a_i$ the ion-sphere radius) measures the importance of interactions. Debye-Hückel gives the weak-coupling correction to the pressure; the plasma crystallizes at $\Gamma_C\approx175$ (white dwarf interiors).

## Hands-on problems

**1. [E] Ideal gas plus radiation.** Implement $P$, $u$, $s$, $\chi_\rho$, $\chi_T$, $c_V$, $c_P$, $\Gamma_1$, $\nabla_{\rm ad}$ analytically for a fully ionized gas with radiation. Verify the limits (ideal: $\nabla_{\rm ad}=0.4$, $\Gamma_1=5/3$; radiation dominated: $0.25$, $4/3$) and that a numerical derivative of $P(\rho,T)$ agrees with the analytic $\chi$'s at $10^{-8}$.

**2. [E] Mean molecular weights.** Write a function that returns $\mu$, $\mu_e$, $\mu_{\rm ion}$ for a composition vector. For $X=0.7381$, $Y=0.2485$, $Z=0.0134$ at the surface you should get $\mu\approx0.60$. For the central composition $X=0.34$, $Y=0.64$, $Z=0.02$ you should get about $0.85$.

**3. [M] Partial ionization and the dip in $\nabla_{\rm ad}$.** Implement the Saha equation for H, He (two stages) and a few metals (or a simplified average). Compute $\nabla_{\rm ad}$ and $\Gamma_1$ at fixed density as a function of $T$ from $10^3$ to $10^6$ K. You should find that $\nabla_{\rm ad}$ drops to about 0.1 and $\Gamma_1$ to about $1.1$ to $1.2$ in the hydrogen and helium ionization zones. This is the physical origin of the convective envelope and of the $\kappa$ and $\gamma$ mechanisms in pulsation (Module 16).

**4. [M] Fermi-Dirac integrals.** Implement the generalized Fermi-Dirac integrals (including the relativity parameter $\beta_r=k_BT/m_ec^2$) by numerical quadrature or a published approximation. Compute $n_e$, $P_e$, $u_e$ and $s_e$ for arbitrary $(\rho,T)$ and verify: non-degenerate limit $P_e=n_ek_BT$, degenerate non-relativistic and ultra-relativistic limits above, and the Chandrasekhar formula at $T=0$.

**5. [M] Full EOS.** Combine ions (ideal), electrons (Fermi-Dirac), radiation and the Debye-Hückel correction. Check **thermodynamic consistency** with the Maxwell relation $(\partial s/\partial\rho)_T=-(1/\rho^2)(\partial P/\partial T)_\rho$ numerically over the $(\rho,T)$ plane relevant for stars ($\rho=10^{-10}$ to $10^{10}$ g/cm$^3$, $T=10^3$ to $10^{10}$ K). Mark the regions in which each component dominates (a classic EOS map).

**6. [M] Chandrasekhar's white dwarf EOS.** Use the zero-temperature formula to build the $P(\rho)$ relation for $\mu_e=2$ and verify the two power-law limits and the transition near $x\approx1$ ($\rho\approx10^6$ g/cm$^3$).

**7. [H] A tabulated EOS.** Tabulate the Helmholtz free energy $F(\rho,T)$ (and its first and second derivatives) on a $(\log\rho,\log T)$ grid and interpolate with biquintic or bicubic Hermite polynomials so that thermodynamic consistency is preserved by construction (the approach of the widely used Helmholtz EOS of Timmes and Swesty). Measure the interpolation error and the speed-up over the on-the-fly evaluation. Decide the resolution you need.

**8. [H] Non-ideal effects in the Sun.** Include the Debye-Hückel (and optionally the Ichimaru or OCP) correction and estimate its effect on $P$, $\Gamma_1$ and $c_s$ in the solar interior. You should find corrections of order one percent in pressure and larger in $\Gamma_1$ relevant for helioseismology. Compare with the literature (verify).

## Software component

`starlite/eos/`: a function `eos(rho, T, comp)` returning $P$, $E$, $S$, $\chi_\rho$, $\chi_T$, $c_V$, $c_P$, $\Gamma_1$, $\Gamma_2$, $\nabla_{\rm ad}$, $\mu$, $\mu_e$, $\eta$, and the free electron fraction, with analytic derivatives with respect to $\ln\rho$ and $\ln T$, an inverse routine returning $(\rho,T)$ for given $(P,T)$ or $(\rho,E)$, and a tabulated version.

## Checks

- Ideal gas and radiation limits of $\nabla_{\rm ad}$ and $\Gamma_1$ to $10^{-8}$.
- $\mu\approx0.60$ (surface), $0.85$ (solar centre).
- Dips of $\nabla_{\rm ad}$ to about 0.1 in the ionization zones.
- Degenerate limits $1.0036\times10^{13}$ and $1.2435\times10^{15}$ reproduced to 0.1 percent.
- Maxwell-relation consistency below $10^{-6}$ over the grid.

## Pitfalls

- Mixing derivatives at fixed $\rho$ and fixed $P$.
- Numerical overflow in $e^\eta$ at high degeneracy (use asymptotic expansions).
- Letting the EOS be thermodynamically inconsistent: this destroys the convergence of the Newton solver in Module 08 in subtle ways.
- Ionization at high density: the Saha equation fails when pressure ionization sets in.

## Gate questions

1. Why does the adiabatic gradient drop in an ionization zone, and what is its consequence for convection?
2. How does electron degeneracy change the response of the star to heating? (Think of the helium flash.)
3. Why is thermodynamic consistency of the EOS essential for a numerical stellar code?

## Deliverable

`starlite/eos/` with tests, `notebooks/03_eos.ipynb` including the EOS map and the $\nabla_{\rm ad}$ plot, solutions for problems 1 to 8.
