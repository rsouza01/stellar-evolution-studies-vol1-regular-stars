# Module 18: Mass loss and binary evolution

**Difficulty:** Hard. **Time:** about 2 weeks.

## Goal

Understand the main mass-loss mechanisms across the HR diagram and the physics of close binary evolution: Roche geometry, mass transfer and its stability, orbital evolution, common envelope evolution, tides and angular momentum loss. Add a binary module to the capstone code that evolves two stars and an orbit together.

## Prerequisites

Modules 09, 12, 13.

## Reading

- **Egg, Hil:** Roche geometry, mass transfer, tides, evolution of close binaries, cataclysmic variables, X-ray binaries.
- **L&C, Iben, KWW:** stellar winds and mass loss on the RGB, AGB and in massive stars; the evolution of close binary systems.
- **Pri, Pols:** summaries of binary evolution.
- For more depth on accretion and compact binaries: Pringle and King, *Astrophysical Flows*, and the chapters on compact-object binaries in the literature.

## Key concepts and equations (derive)

**Mass loss regimes.** Solar-type wind ($\sim2\times10^{-14}\,M_\odot$/yr); Reimers-type winds in red giants (Module 12); dust-driven superwinds on the AGB ($10^{-5}$ to $10^{-4}\,M_\odot$/yr); line-driven winds of hot stars (Module 13); eruptive mass loss of luminous blue variables; pulsation-driven mass loss.

**Roche lobe.** In a binary with masses $M_1$ (donor) and $M_2$ and separation $a$, the Roche lobe radius of the donor is approximated by the Eggleton formula, with $q=M_1/M_2$:

$$\frac{R_L}{a}=\frac{0.49\,q^{2/3}}{0.6\,q^{2/3}+\ln\left(1+q^{1/3}\right)}$$

**Orbital evolution with conservative mass transfer.** Total mass $M$ and orbital angular momentum $J=\mu\sqrt{GMa}$ conserved give $a\propto(M_1M_2)^{-2}$, so

$$\frac{\dot a}{a}=-2\frac{\dot M_1}{M_1}(1-q)\ ,\quad q=\frac{M_1}{M_2}$$

and the orbit shrinks while mass flows from the more massive to the less massive star, widening after the mass ratio reverses.

**Stability of mass transfer.** Define the logarithmic responses $\zeta_{\rm ad}=d\ln R/d\ln M$ for the donor (adiabatic) and $\zeta_L=d\ln R_L/d\ln M_1$. For conservative transfer with $R_L\propto a\,(M_1/M)^{1/3}$ one finds $\zeta_L=2q-5/3$ (derive). Mass transfer is stable when $\zeta_{\rm ad}\ge\zeta_L$. For a fully convective donor ($n=3/2$ polytrope, $\zeta_{\rm ad}=-1/3$) this gives the critical mass ratio $q_{\rm crit}=2/3$ (derive and check). Unstable transfer leads to a common envelope.

**Cases of mass transfer** (Kippenhahn and Weigert): case A (during the main sequence), case B (after the main sequence but before core helium ignition) and case C (after core helium exhaustion).

**Common envelope.** In the energy formalism, the orbital energy released spirals the companion in and ejects the envelope:

$$\alpha_{\rm CE}\left(\frac{GM_cM_2}{2a_f}-\frac{GM_1M_2}{2a_i}\right)=\frac{GM_1M_{\rm env}}{\lambda R_1}$$

with efficiency $\alpha_{\rm CE}$ and structure parameter $\lambda$ (both uncertain).

**Tides.** For stars with convective envelopes, Zahn's equilibrium tide gives a circularization timescale $t_{\rm circ}\propto(a/R)^8$; the period above which the binaries in old clusters are not circularized is of order 10 d (verify). Synchronization is faster than circularization.

**Angular momentum loss.** Magnetic braking (Rappaport, Verbunt and Joss 1983): $\dot J\approx-3.8\times10^{-30}M_\odot R_\odot^4\,(R/R_\odot)^{\gamma}\,\omega^3$ dyn cm, with $\gamma\sim3$ to 4 (verify). It drives cataclysmic variables to short periods and explains the period gap at 2 to 3 hours. Gravitational radiation drives close compact binaries together.

## Hands-on problems

**1. [E] Roche geometry.** Compute the Roche potential numerically for a given $q$, find the Lagrange points and the Roche lobe volume-equivalent radius, and compare with the Eggleton formula for $q=0.1$ to $10$ (accuracy of about 1 percent). Compute the ratio $R/R_L$ for stars in a binary as they evolve through the expansion on the RGB and AGB and find the onset of mass transfer for given separations.

**2. [E] Conservative mass transfer.** Integrate the orbital evolution for a $1.2+1.0\,M_\odot$ pair as mass transfers from the more massive star. Verify the period minimum when $q=1$ and the final period when the donor becomes a low-mass remnant.

**3. [M] Stability of mass transfer.** Compute $\zeta_{\rm ad}$ for polytropes of $n=3/2$ (convective), for the radiative donors from your stellar models and for giants with deep convective envelopes. Derive and plot $q_{\rm crit}$ for conservative and non-conservative transfer. Show that giants with deep convective envelopes are unstable even for moderate $q$.

**4. [M] Tides.** Implement Zahn's equilibrium-tide synchronization and circularization timescales for convective-envelope and radiative-envelope stars. Evolve the eccentricity and rotation of a $1.0+0.8\,M_\odot$ binary with $P=5$ and $15$ d for 5 Gyr and compare with the observed circularization period in old open clusters.

**5. [M] Mass transfer rate prescription.** Implement a donor mass-transfer rate from the exponential Kolb-Ritter formula $\dot M\propto\exp\left[(R-R_L)/H_P\right]$ and the "implicit" scheme in which $\dot M$ is determined within the structure iteration so that $R\approx R_L$ (what MESA does). Test on a conservative case and compare the two schemes in smoothness and cost.

**6. [H] The binary evolution driver.** Evolve two stars and their orbit in the same time loop in your code: each star is an instance of `star`, with its own solver; the binary driver computes $\dot M$, orbital changes (mass transfer, winds, tides, magnetic braking, gravitational waves), updates the stars' masses and spins, and controls the global time step. Test: (a) total mass and angular momentum conserved in the conservative case to $10^{-6}$; (b) case B evolution of a $1.5+1.0\,M_\odot$ system reproduces the final white dwarf plus main-sequence binary; (c) the final period against the white dwarf mass in wide low-mass X-ray binaries matches the relation of Rappaport et al. (verify).

**7. [M] Common envelope.** For a $1.5\,M_\odot$ giant with a $0.8\,M_\odot$ companion at several initial separations, compute the final separation from the energy formalism for $\alpha_{\rm CE}=0.3,1$ and $\lambda$ from your giant models. Estimate the fraction of systems that merge and discuss the uncertainty in $\lambda$.

**8. [H] Cataclysmic variables.** With the magnetic braking law and the donor's response to mass loss, evolve a white dwarf plus low-mass main-sequence star binary from a period of 10 h to below 80 min. Reproduce the period gap (donor becomes fully convective at about $0.2$ to $0.3\,M_\odot$, magnetic braking decreases, the donor shrinks back inside its Roche lobe, mass transfer stops until gravitational radiation brings it back at about 2 h) and the period minimum near 80 min (verify).

**9. [H] A rapid binary population synthesis code.** Using fitting formulae for single-star evolution (the Hurley-Pols-Tout analytic formulae are standard; verify) or your own interpolated grids, plus simple prescriptions for mass transfer, common envelope, supernova kicks and gravitational-wave inspiral, generate a population of binaries from initial distributions (IMF, mass ratio, period) and compute the numbers of white dwarf-main sequence binaries, double white dwarfs, Algols and, if you wish, compact-object mergers. Compare with observed numbers or with the literature. This is the connection to modern population studies.

## Software component

- `starlite/binary/`: Roche geometry, orbit update, mass-transfer schemes, tidal and angular momentum loss modules, and the two-star driver.
- `starlite/winds/` additions: dust-driven AGB winds and eruptive-mass-loss placeholders.
- A rapid population synthesis tool under `tools/bps.py` (optional).

## Checks

- Eggleton formula versus numerical Roche lobe radius within 1 percent.
- Orbital period minimum at $q=1$ for conservative transfer; $q_{\rm crit}=2/3$ for a fully convective donor.
- Conservation of total mass and orbital angular momentum to $10^{-6}$ in conservative evolution.
- Period gap of cataclysmic variables between about 2 and 3 hours.

## Pitfalls

- Mixing definitions of the mass ratio $q$ (donor over accretor or the reverse).
- Neglecting the response of the donor's radius to mass loss (thermal timescale versus dynamical timescale).
- Using the common envelope formalism as if it were a prediction; $\alpha_{\rm CE}$ and $\lambda$ are essentially free parameters.

## Gate questions

1. Why does the orbit widen once the donor becomes the less massive star?
2. Why are giants with deep convective envelopes prone to dynamically unstable mass transfer?
3. What is the physical origin of the period gap of cataclysmic variables?

## Deliverable

`starlite/binary/`, `notebooks/18_binaries.ipynb`, solutions for problems 1 to 9.
