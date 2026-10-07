# Module 15: White dwarfs, neutron stars and explosions

**Difficulty:** Hard. **Time:** about 3 weeks.

## Goal

Understand the endpoints of stellar evolution: white dwarf structure and cooling, neutron star structure (the TOV equations and the dense-matter EOS), the physics of core-collapse and thermonuclear supernovae, and supernova light curves. Add white dwarf and neutron star modules and a light-curve model to the capstone.

## Prerequisites

Modules 03, 07, 12, 13.

## Reading

- **ST, Cha:** white dwarfs, Chandrasekhar mass, neutron stars, black holes.
- **Gle:** dense matter and neutron star structure. Haensel, Potekhin and Yakovlev, *Neutron Stars 1*, as an extra.
- **Arn, Iben (vol. 2), KWW:** core collapse, supernova explosions, Type Ia supernovae. Branch and Wheeler, *Supernova Explosions*, as an extra.
- **HKT, C&O:** the degenerate remnants of stars.

## Key equations (derive)

**White dwarf structure.** Hydrostatic equilibrium with the Chandrasekhar degenerate EOS (Module 03). The mass-radius relation has $R\propto M^{-1/3}$ at low mass and $R\to0$ as $M\to M_{\rm Ch}\approx5.83/\mu_e^2\,M_\odot$ ($1.456\,M_\odot$ for $\mu_e=2$; Coulomb and general-relativistic corrections reduce it). A $0.6\,M_\odot$ white dwarf has $R\approx0.012\,R_\odot$.

**White dwarf cooling.** The interior is isothermal (conduction), with a thin non-degenerate envelope controlling the heat loss. Mestel's law for an ideal ion gas: $L\propto T_c^{7/2}$ (from Kramers opacity), giving a cooling time $t_{\rm cool}\propto L^{-5/7}$. Corrections: neutrino cooling at high $L$, latent heat and phase separation on crystallization ($\Gamma_C\approx175$), Debye cooling at low $T$.

**Neutron stars (TOV).** In general relativity,

$$\frac{dP}{dr}=-\frac{G\left(\rho+P/c^2\right)\left(m+4\pi r^3P/c^2\right)}{r^2\left(1-2Gm/rc^2\right)},\qquad\frac{dm}{dr}=4\pi r^2\rho$$

with $\rho$ the total energy density divided by $c^2$. The free neutron gas gives the Oppenheimer-Volkoff maximum mass of about $0.7\,M_\odot$. Realistic EOS with nuclear forces give $M_{\max}\gtrsim2\,M_\odot$ and radii of 11 to 13 km (verify against current observations).

**Core-collapse supernovae.** When the iron core exceeds its effective Chandrasekhar mass, it collapses to nuclear density ($\sim2.7\times10^{14}$ g/cm$^3$) in about 100 ms, bounces, and launches a shock that stalls because of photodisintegration and neutrino losses. Neutrino heating behind the stalled shock (delayed neutrino mechanism) revives it. Energies: the neutron star binding energy $\approx3\times10^{53}$ erg ($\approx99$ percent in neutrinos), the ejecta kinetic energy $\approx10^{51}$ erg.

**Type Ia supernovae.** Thermonuclear disruption of a carbon-oxygen white dwarf, either near the Chandrasekhar mass (single degenerate: deflagration then detonation) or sub-Chandrasekhar (double detonation, mergers). The energy release from burning $\sim0.5\,M_\odot$ of C/O into iron-group elements and intermediate-mass elements exceeds the binding energy of the white dwarf ($\sim5\times10^{50}$ erg).

**Supernova light curves (Arnett).** The radioactive chain $^{56}$Ni$\to^{56}$Co$\to^{56}$Fe powers the light curve, with $\tau_{\rm Ni}=8.8$ d and $\tau_{\rm Co}=111$ d. The heating rates per gram of $^{56}$Ni are approximately $\epsilon_{\rm Ni}\approx3.9\times10^{10}e^{-t/\tau_{\rm Ni}}$ and $\epsilon_{\rm Co}\approx6.8\times10^{9}e^{-t/\tau_{\rm Co}}$ erg/g/s (verify). Photons diffuse out on a timescale $\tau_m=\left(2\kappa M_{\rm ej}/\beta cv\right)^{1/2}$ with $\beta\approx13.8$. Arnett's rule says that the peak luminosity equals the radioactive power at the time of the peak: $L_{\rm peak}\approx M_{\rm Ni}\,\epsilon(t_{\rm peak})$.

## Hands-on problems

**1. [E] White dwarf mass-radius relation.** Integrate hydrostatic equilibrium with the Chandrasekhar EOS (Module 03) for $\mu_e=2$ over a range of central densities ($10^4$ to $10^{10}$ g/cm$^3$). Plot $R(M)$ and show the $M^{-1/3}$ law and the asymptote $M\to M_{\rm Ch}$. Verify $M_{\rm Ch}\approx1.456\,M_\odot$ and $R\approx0.012\,R_\odot$ at $0.6\,M_\odot$. Compare with the polytropic result of Module 02.

**2. [M] Cooling of white dwarfs.** Build an isothermal-core white dwarf model with a non-degenerate envelope of given mass and your radiative opacity. Compute the $L(T_c)$ relation, verify Mestel's law at intermediate luminosity, and integrate the cooling equation $dT_c/dt$ with the heat capacity of the ion gas. Add (a) neutrino losses, (b) latent heat release at crystallization and (c) the Debye correction. Compute the cooling age of a $0.6\,M_\odot$ carbon-oxygen white dwarf to $T_{\rm eff}=5000$ K and compare with the literature (several Gyr; verify). Compare with your evolution code run to the white dwarf stage.

**3. [M] The white dwarf luminosity function.** Combine your cooling tracks with the IMF (Module 10), a constant star formation rate and the initial-final mass relation (Module 12) to compute the luminosity function. Identify the cutoff at low luminosity and relate it to the age of the disk.

**4. [M] Neutron star structure with your TOV solver.** Reuse your TOV solver (the neutron-star solver you already wrote). Test it against the free-neutron-gas EOS: $M_{\max}\approx0.71\,M_\odot$. Then use a realistic EOS (a piecewise polytropic parameterization of several nuclear EOS, or a tabulated EOS from the literature; verify the sources) to compute $M(R)$, the maximum mass and the radius at $1.4\,M_\odot$ (about 11 to 13 km), the moment of inertia and the tidal deformability.

**5. [M] Core collapse energetics.** Evaluate the gravitational binding energy of a neutron star of $1.4\,M_\odot$ and $12$ km radius ($\approx3\times10^{53}$ erg) and the energy of photodisintegration of the iron core. Estimate the neutrino luminosity and the duration of the neutrino burst ($\sim10^{52}$ erg/s over $\sim10$ s). Explain why only about one percent goes into the explosion.

**6. [M] The Arnett light curve model.** Implement the Arnett (1982) analytic model (or the semi-analytic diffusion approach with $^{56}$Ni heating and a homologously expanding ejecta). Compute a Type Ia light curve for $M_{\rm Ni}=0.6\,M_\odot$, $M_{\rm ej}=1.4\,M_\odot$ and a characteristic velocity $v\approx10^9$ cm/s. You should obtain a peak luminosity of order $10^{43}$ erg/s after about 18 days, with a late-time decline set by the $^{56}$Co decay. Vary $M_{\rm Ni}$ and the ejecta mass to reproduce the width-luminosity trend (the Phillips relation).

**7. [H] A core bounce hydrodynamics code.** Write a one-dimensional spherically symmetric Lagrangian hydrodynamics code with artificial viscosity, Newtonian gravity (with an effective potential for general relativity), a tabulated nuclear EOS, electron captures and a simple neutrino leakage scheme. Collapse a model iron core (from your massive star calculation) and show the bounce at about nuclear density, the shock formation and its stall at 100 to 200 km. This is a substantial project: consider restricting to the collapse phase and bounce first.

**8. [H] Type Ia energetics.** Using your alpha-chain network and the NSE solver (Module 07) compute the energy release per gram of burning of C/O to (a) silicon-group elements and (b) iron-group elements, as a function of the fuel density. Determine how much mass must be burned to unbind a $1.4\,M_\odot$ white dwarf and what this implies for the density at which a deflagration must start. Then build a toy 1D burning wave with a prescribed flame speed and compute the explosion energy.

**9. [H] Black hole formation and fallback.** From your pre-collapse profiles (Module 13), estimate the mass that falls back onto the proto-neutron star for a given explosion energy using a simple energy-based criterion; determine the remnant (neutron star or black hole) as a function of initial mass and discuss the role of compactness.

## Software component

- `starlite/wd/`: white dwarf EOS and structure, isothermal cooling model with microphysics.
- `starlite/ns/`: TOV solver with piecewise-polytrope and tabulated EOS interface.
- `starlite/sn/`: Arnett light-curve model, simple explosion energy diagnostics.
- A white dwarf boundary-condition option in `star/` for evolution into the cooling track.

## Checks

- White dwarf: $M_{\rm Ch}\approx1.456\,M_\odot$, $R(0.6\,M_\odot)\approx0.012\,R_\odot$.
- Mestel cooling law reproduced in the intermediate luminosity range.
- TOV: $M_{\max}\approx0.71\,M_\odot$ for the free neutron gas; $M_{\max}\gtrsim2\,M_\odot$ for a stiff realistic EOS.
- Type Ia light curve peaks at about $10^{43}$ erg/s after roughly 18 days.
- Binding energy of a neutron star about $3\times10^{53}$ erg.

## Pitfalls

- Using Newtonian gravity for neutron stars.
- Neglecting crystallization and phase separation in the cooling of old white dwarfs.
- Treating Arnett's rule as exact: it is an approximation to the time-dependent diffusion.

## Gate questions

1. Why is the mass of a white dwarf bounded, and what changes it from the simple $1.456\,M_\odot$?
2. Why does the neutron star EOS stiffness set the maximum mass?
3. Why does the supernova light curve decline with the $^{56}$Co decay timescale at late times?

## Deliverable

`starlite/wd/`, `starlite/ns/`, `starlite/sn/`, `notebooks/15_remnants.ipynb`, solutions for problems 1 to 9.
