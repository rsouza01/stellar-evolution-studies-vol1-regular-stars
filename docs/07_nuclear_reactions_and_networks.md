# Module 07: Nuclear reactions, energy generation and reaction networks

**Difficulty:** Medium to hard. **Time:** about 3 weeks.

## Goal

Understand thermonuclear reaction rates (Gamow peak, S-factor, resonances, screening), the main burning stages and their energetics, neutrino losses, and implement a reaction network with stiff implicit integration. This module supplies the nuclear physics of the capstone code.

## Prerequisites

Modules 01 and 03.

## Reading

- **Cla, R&R, Ili:** thermonuclear reaction rates, the Gamow peak, S-factors, resonances, electron screening, pp chains, CNO cycles, triple alpha, advanced burning, weak interactions, neutrino processes, reaction networks, nuclear statistical equilibrium.
- **KWW, HKT:** nuclear energy generation and its temperature dependence.
- **Arn:** advanced burning stages and their timescales.

## Key equations (derive)

**Reaction rate per volume** for particles 1 and 2:

$$r_{12}=\frac{n_1n_2}{1+\delta_{12}}\langle\sigma v\rangle,\qquad\langle\sigma v\rangle=\sqrt{\frac{8}{\pi m_r}}\frac{1}{(k_BT)^{3/2}}\int_0^\infty S(E)\,e^{-E/k_BT-b/\sqrt E}\,dE$$

with the Gamow parameter $b=\pi\alpha Z_1Z_2\sqrt{2m_rc^2}$. For non-resonant reactions the integrand peaks at the **Gamow energy** $E_0=(bk_BT/2)^{2/3}$:

$$E_0=1.22\,(Z_1^2Z_2^2A_rT_6^2)^{1/3}\ {\rm keV},\qquad\Delta\approx0.749\,(Z_1^2Z_2^2A_rT_6^5)^{1/6}\ {\rm keV}$$

with $T_6=T/10^6$ K and $A_r$ the reduced mass number. For $p+p$ at $T=1.5\times10^7$ K this gives $E_0\approx5.9$ keV.

**Narrow resonances:** $\langle\sigma v\rangle=\left(\frac{2\pi}{m_rk_BT}\right)^{3/2}\hbar^2(\omega\gamma)_r\,e^{-E_r/k_BT}$ (derive).

**Temperature dependence.** Locally $\epsilon\propto\rho T^\nu$ with $\nu\approx4$ (pp chain, $T\approx1.5\times10^7$ K), $\nu\approx16$ to 18 (CNO), $\nu\approx40$ (triple alpha at $10^8$ K).

**Energetics.** Net $4\,^1{\rm H}\to{}^4{\rm He}$: $Q=26.73$ MeV (0.7 percent of the rest mass), of which a fraction is carried by neutrinos (about 2 percent in the pp chain). $3\,^4{\rm He}\to{}^{12}{\rm C}$: 7.275 MeV; ${}^{12}{\rm C}(\alpha,\gamma){}^{16}{\rm O}$: 7.162 MeV.

**Weak screening** (Salpeter): the rate is multiplied by $f=\exp(0.188\,Z_1Z_2\,\zeta\,\rho^{1/2}T_6^{-3/2})$ with $\zeta=\left[\sum_iX_i(Z_i^2+Z_i)/A_i\right]^{1/2}$.

**Network equations.** With molar abundances $Y_i=X_i/A_i$,

$$\frac{dY_i}{dt}=\sum_j\lambda_jY_j+\sum_{j,k}\rho\,N_A\langle\sigma v\rangle_{jk}Y_jY_k+\dots,\qquad\epsilon_{\rm nuc}=N_A\sum_k Q_k\,r_k$$

The system is **stiff**: rates differ by tens of orders of magnitude, so one needs implicit methods with the Jacobian. Mass conservation requires $\sum_iA_iY_i=1$.

**REACLIB format.** Each reaction rate is parameterized as $\lambda=\exp\left(a_0+a_1/T_9+a_2T_9^{-1/3}+a_3T_9^{1/3}+a_4T_9+a_5T_9^{5/3}+a_6\ln T_9\right)$, summed over several sets. Reverse rates follow from detailed balance with partition functions.

**Neutrino losses.** Pair annihilation (dominant above $T\sim10^9$ K), photoneutrino, plasma neutrino and bremsstrahlung, plus Urca-type processes at high density. Standard fits (Beaudet-Petrosian-Salpeter, Itoh et al.) give $\epsilon_\nu(\rho,T,\mu_e)$.

**Nuclear statistical equilibrium (NSE).** At $T_9\gtrsim5$ the abundances follow from nuclear chemical equilibrium and are fixed by $\rho$, $T$ and $Y_e$ (Saha-like formula; see Cla, Ili, Arn).

## Hands-on problems

**1. [E] The Gamow peak.** Compute the Gamow integrand $e^{-E/k_BT-b/\sqrt E}$ for $p+p$ and for ${}^{14}{\rm N}+p$ at the solar central temperature. Locate the peak numerically and verify $E_0\approx5.9$ keV and $\Delta\approx6.4$ keV for $p+p$. Compare with the analytic saddle-point estimate of the rate.

**2. [E] pp chain versus CNO.** Using approximate power-law energy generation rates calibrated so that the Sun's luminosity is reproduced, compute the temperature at which CNO and pp contribute equally (around $1.7\times10^7$ K in a solar mixture; verify). Explain why massive stars burn through CNO and why they have convective cores.

**3. [M] Reaction rates by numerical integration.** Implement the thermonuclear integral for a non-resonant reaction with a constant $S$-factor and for a narrow resonance. Compare with the saddle-point approximation and the REACLIB fit. How wrong is the saddle point approximation at $T_9=0.1$ for ${}^{12}{\rm C}+{}^{12}{\rm C}$?

**4. [M] REACLIB.** Download the public REACLIB library (JINA), write a parser, evaluate the rates and their reverse rates for a few reactions (for example ${}^{12}{\rm C}(\alpha,\gamma){}^{16}{\rm O}$, ${}^{14}{\rm N}(p,\gamma){}^{15}{\rm O}$, $3\alpha$) and plot them versus $T_9$. Check detailed balance by comparing $(\alpha,\gamma)$ and $(\gamma,\alpha)$ rates.

**5. [M] A minimal hydrogen and helium network.** Build the network ${}^1{\rm H}$, ${}^3{\rm He}$, ${}^4{\rm He}$, ${}^{12}{\rm C}$, ${}^{13}{\rm C}$, ${}^{14}{\rm N}$, ${}^{15}{\rm N}$, ${}^{16}{\rm O}$ with effective pp, CNO and $3\alpha$ links. Implement an implicit integrator (backward Euler with Newton iteration and a dense LU factorization, then BDF or Radau from `scipy` for validation). Test: $\sum X_i=1$ to machine precision, energy release equals $Q\times$ (mass burned), and the **CNO equilibrium** (${}^{14}{\rm N}$ dominating, ${}^{12}{\rm C}/{}^{13}{\rm C}\approx3$ to 4) is reached on the expected timescale at $T=2\times10^7$ K.

**6. [M] Screening.** Implement weak screening and check that it enhances the solar-core pp rate by about five percent (verify). Implement intermediate and strong screening (Graboske, Itoh) and map the regimes in the $(\rho,T)$ plane.

**7. [M] Neutrino losses.** Implement a standard analytic fit for $\epsilon_\nu$. Map where each process dominates and compare $\epsilon_\nu$ with $\epsilon_{\rm nuc}$ along the central track of a massive star. Show that at $T\approx10^9$ K neutrino losses exceed the photon luminosity and drive the contraction timescale.

**8. [H] An alpha-chain network.** Build the 13- to 21-isotope alpha-chain network (${}^4$He to ${}^{56}$Ni with $(\alpha,\gamma)$ plus the effective $(\alpha,p)(p,\gamma)$ links and photodisintegrations, the "aprox13/21" approach). Integrate carbon burning at $T_9=0.8$, oxygen burning at $T_9=2$ and silicon burning at $T_9=3.5$, with sparse Jacobians. Compare timescales and products with the textbook values and benchmark your solver against `scipy.integrate.solve_ivp(method='BDF')`.

**9. [H] NSE.** Solve for NSE abundances at $T_9=5,6,8$ and $\rho=10^7,10^9$ g/cm$^3$ with $Y_e=0.5$ and $0.47$ by Newton iteration for the proton and neutron chemical potentials. Show the dominance of ${}^{56}$Ni for $Y_e=0.5$ and of neutron-rich Fe-group nuclei for lower $Y_e$. Compare with your alpha-chain network at the same conditions.

**10. [H] Solar neutrinos.** With the temperature profile of a solar model (Module 08/11), compute the production profile of the pp, ${}^7$Be and ${}^8$B neutrinos and show that the ${}^8$B flux scales as about $T_c^{24}$. Compare the fluxes with the published solar-model values and measurements (verify the numbers).

## Software component

- `starlite/rates/`: REACLIB parser, rate evaluation with derivatives, reverse rates, screening.
- `starlite/net/`: networks (pp+CNO+3$\alpha$ "basic", alpha-chain, larger), implicit integrator with sparse Jacobian, NSE solver.
- `starlite/neu/`: neutrino loss module.
- Interface: `net.burn(rho, T, Y, dt) -> Y_new, eps_nuc, eps_nu, dY_dlnT, dY_dlnrho`.

## Checks

- $E_0\approx5.9$ keV and $\Delta\approx6.4$ keV for $p+p$ at $1.5\times10^7$ K.
- $\sum X_i=1$ to $10^{-12}$ after any integration step.
- CNO equilibrium abundance ratios and timescales.
- Screening enhancement of order 5 percent in the solar core.
- ${}^8$B neutrino flux exponent near 24.

## Pitfalls

- Using explicit integrators for a stiff network (the step collapses).
- Dropping reverse rates at high temperature.
- Mixing mass fractions and molar abundances.
- Forgetting that the energy generation from an implicit step should be computed from the *abundance change*, not the instantaneous rates, for energy conservation.

## Gate questions

1. Why is a star's central temperature nearly independent of its mass while it burns hydrogen?
2. Why are the reaction rates so sensitive to temperature and why does this produce a stable thermostat in non-degenerate stars?
3. What changes about this thermostat when the gas is degenerate?

## Deliverable

`starlite/rates/`, `starlite/net/`, `starlite/neu/` with tests, `notebooks/07_nuclear.ipynb`, solutions for problems 1 to 10.
