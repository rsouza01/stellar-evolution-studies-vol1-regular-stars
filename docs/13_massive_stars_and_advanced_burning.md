# Module 13: Massive stars and advanced burning

**Difficulty:** Hard. **Time:** about 3 weeks.

## Goal

Understand how stars above about 8 solar masses live and die: radiation pressure, convective cores, strong winds, the proximity of the Eddington limit, the advanced burning stages up to silicon, the onion-skin structure and the conditions at core collapse. Extend your code with winds, large networks and (optionally) dynamics.

## Prerequisites

Modules 07, 09, 11 and 12.

## Reading

- **Mae, L&C:** massive stars, stellar winds (the CAK theory), Wolf-Rayet stars, luminous blue variables, mass loss and rotation.
- **Arn, Iben:** advanced burning stages (carbon, neon, oxygen, silicon), neutrino losses, the presupernova structure.
- **KWW, HKT:** late evolution, pair instability, the collapse of the core.
- **Cla, R&R:** the nuclear physics of advanced burning.

## Key concepts and equations

**Massive-star structure.** Large radiation pressure (Module 02: $\beta\approx0.57$ at $100\,M_\odot$ for $\mu=0.6$), $L\to L_{\rm Edd}$, convective cores of 30 to 80 percent of the mass, convective envelopes in red supergiants. The Eddington factor is

$$\Gamma=\frac{L}{L_{\rm Edd}}=\frac{\kappa L}{4\pi cGM}$$

and the mass-luminosity relation flattens to $L\propto M$ approaching $\Gamma\sim1$.

**Line-driven winds (CAK).** Radiation pressure on spectral lines drives winds with a force multiplier $M(t)=kt^{-\alpha}$ ($\alpha\approx0.6$ to $0.7$). The CAK scaling is (derive)

$$\dot M\propto L^{1/\alpha}\left[M(1-\Gamma)\right]^{1-1/\alpha},$$

with terminal speed of order two to three times the escape speed. Typical O star rates are $10^{-7}$ to $10^{-5}\,M_\odot$/yr. For practical use you will implement a fitted prescription (such as that of Vink and collaborators; verify the paper). Wolf-Rayet stars (stripped helium cores) have much denser winds.

**Advanced burning stages** (approximate central temperature, duration for a $25\,M_\odot$ star; verify with Arn or Woosley-Heger-Weaver):

| Stage | $T_c$ | Duration |
| --- | --- | --- |
| Hydrogen | $4\times10^7$ K | $\sim7\times10^6$ yr |
| Helium | $2\times10^8$ K | $\sim5\times10^5$ yr |
| Carbon | $8\times10^8$ K | $\sim600$ yr |
| Neon | $1.5\times10^9$ K | $\sim1$ yr |
| Oxygen | $2\times10^9$ K | $\sim0.5$ yr |
| Silicon | $3.5\times10^9$ K | $\sim1$ day |

After carbon burning, neutrino losses exceed photon losses and the timescale shrinks dramatically. Burning proceeds in a nested "onion-skin" structure of shells (H, He, C/O, Ne/O, Si) around an iron-group core of about $1.3$ to $2\,M_\odot$.

**Core collapse conditions.** The iron core approaches its (finite-temperature) Chandrasekhar mass; electron captures and photodisintegration reduce pressure support and the core collapses. A measure of the structure is the compactness $\xi_{2.5}=\dfrac{2.5}{R(M=2.5\,M_\odot)/1000\ {\rm km}}$ evaluated at core bounce or at collapse onset (verify the definition in the literature).

**Mass thresholds.** About $8\,M_\odot$ for carbon ignition; about $8$ to $10\,M_\odot$ for super-AGB stars with ONe cores and electron-capture supernovae; core collapse above about $10\,M_\odot$. Stars with helium cores of about 64 to 133 $M_\odot$ undergo **pair-instability** supernovae (the EOS $\Gamma_1$ falls below $4/3$ when pair production is important; verify the ranges).

## Hands-on problems

**1. [E] Eddington limit and massive-star tracks.** For stars of $10$ to $150\,M_\odot$ on the ZAMS, compute $\Gamma$ and $\beta$ with your models. At what mass does $\Gamma$ exceed $0.5$? Plot the luminosity against mass and compare the exponent with the low-mass value. Locate the Humphreys-Davidson limit in the HR diagram (the observed upper luminosity boundary for cool supergiants).

**2. [M] A wind recipe.** Derive the CAK scaling and implement a wind prescription for hot stars (O, B and Wolf-Rayet types) with the transitions between regimes. Compute $\dot M$ for $20$, $40$ and $60\,M_\odot$ ZAMS stars and the amount of mass lost over the main sequence (of order a few to tens of percent). Plot the dependence on $Z$ (the winds are weaker at low metallicity) and discuss the consequence for the upper-mass remnants.

**3. [M] Evolving $15$ and $25\,M_\odot$ to the end of helium burning.** Use your code with winds, overshoot and the Ledoux criterion. Plot the tracks (a $25\,M_\odot$ star moves to a red supergiant or blue loops depending on mass loss and mixing), the central abundances, the convective-core evolution and the lifetimes of each phase (about $10^7$ yr for hydrogen at $15\,M_\odot$ and $6$ to $7\times10^6$ yr at $25\,M_\odot$).

**4. [H] Advanced burning with a larger network.** Couple a 21-isotope alpha network (Module 07), or better a network of 100 to 200 isotopes with weak reactions (to follow $Y_e$) to the structure code and follow carbon, neon, oxygen and silicon burning in the core of the $15\,M_\odot$ and $25\,M_\odot$ models. Record $T_c$, $\rho_c$, the neutrino luminosity and the duration of each stage; compare with the table. Plot the onion-skin composition profile at the start of silicon burning and at the last model you can compute.

**5. [M] Neutrino-dominated evolution.** Plot $L_\nu/L_\gamma$ during the evolution and show that after carbon ignition neutrinos dominate. Explain why this leads to a contraction timescale far shorter than the Kelvin-Helmholtz time for photons.

**6. [M] Pre-collapse structure.** At the end of your calculation, compute the iron core mass, the entropy profile, the compactness $\xi_{2.5}$ and the binding energy of the outer layers. Repeat for $12$, $15$, $20$, $25$, $30\,M_\odot$ (with the same input physics) and plot the compactness against the initial mass; the literature finds a non-monotonic pattern (verify), so check whether your results show it and how sensitive it is to the network, the mixing and the convection treatment.

**7. [H] Pair instability.** Build the structure of a helium core of $80\,M_\odot$ (or a pure helium star) and implement the dynamical terms in your structure equations (the inertial term $\ddot r$) for hydrodynamic evolution. Show that after oxygen ignition during contraction the $\Gamma_1<4/3$ region leads to a collapse and an explosive burning phase. Use a Lagrangian hydrodynamic scheme with a nuclear network. Compare the energy release with the binding energy of the core to see whether the star is disrupted or pulsates.

**8. [H] Radiation-dominated envelopes.** Study the inflated envelopes of massive stars close to the Eddington limit: with MLT, the temperature gradient in the iron-opacity-peak zone can exceed the adiabatic one by a lot, producing density inversions. Implement the "MLT++" artificial reduction of the superadiabaticity (optional flag) and show how it affects the radius, the time step and the HR diagram position.

## Software component

- `starlite/winds/`: hot-star and Wolf-Rayet prescriptions with a metallicity dependence.
- `starlite/net/`: larger networks (alpha chain, 100 to 200 isotopes) with weak rates.
- A dynamical option in `star/structure.py` (inertial term and a velocity variable).
- A set of diagnostic routines in `star/io.py`: core masses, compactness, binding energies.

## Checks

- $\Gamma\to1$ for the most massive models; the luminosity-mass exponent flattens.
- Mass lost over the main sequence of a few percent at $15\,M_\odot$ and tens of percent at $60\,M_\odot$ (with solar $Z$; verify).
- Duration of burning stages and central temperatures approximately as in the table.
- Neutrino losses dominate after carbon ignition.
- Iron core masses between $1.3$ and $2\,M_\odot$ at collapse.

## Pitfalls

- Applying the Schwarzschild criterion near the Eddington limit without regard for radiation-dominated convection.
- Using too small a network for advanced burning: it gives wrong energy generation and does not track $Y_e$.
- Ignoring that advanced stages are extremely sensitive to the treatment of convective boundaries and mixing, so the pre-collapse structures differ between codes.

## Gate questions

1. Why are massive stars convective in the core and radiative in the envelope?
2. Why do the lifetimes of the stages shrink so drastically after helium burning?
3. Why does a pair-instability supernova not leave a remnant while a core-collapse supernova does?

## Deliverable

`starlite/winds/`, extended networks, tracks and pre-collapse profiles for $12$ to $30\,M_\odot$, `notebooks/13_massive.ipynb`, solutions for problems 1 to 8.
