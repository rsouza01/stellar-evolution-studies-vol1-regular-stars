# Module 10: Star formation and the pre-main sequence

**Difficulty:** Medium. **Time:** about 2 weeks.

## Goal

Follow a star from a molecular cloud core to the zero-age main sequence: gravitational instability and collapse, the protostar and accretion, the Hayashi and Henyey tracks, deuterium burning, brown dwarfs and the initial mass function. Use your evolution code to compute pre-main-sequence tracks.

## Prerequisites

Modules 02 and 09.

## Reading

- **SP:** the whole book is the reference: molecular clouds, cores, isothermal collapse, protostellar accretion, pre-main-sequence evolution, the birthline, the IMF.
- **KWW, HKT, C&O:** star formation and pre-main-sequence evolution; Kelvin-Helmholtz contraction, the Hayashi track.
- **Pols, Pri:** pre-main-sequence models.

## Key equations (derive)

**Jeans mass, length and free-fall time** for an isothermal cloud of sound speed $c_s=\sqrt{k_BT/\mu m_u}$ and density $\rho$:

$$\lambda_J=\sqrt{\frac{\pi c_s^2}{G\rho}},\qquad M_J\sim\left(\frac{5k_BT}{G\mu m_u}\right)^{3/2}\left(\frac{3}{4\pi\rho}\right)^{1/2},\qquad t_{\rm ff}=\sqrt{\frac{3\pi}{32G\rho}}$$

(the numerical coefficient depends on the definition; derive yours).

**Bonnor-Ebert sphere.** An isothermal sphere in pressure equilibrium with an external medium $P_{\rm ext}$ satisfies the isothermal Lane-Emden equation ($n\to\infty$). It is stable up to a critical dimensionless radius $\xi_{\max}\approx6.45$ and a critical mass

$$M_{\rm BE}\approx1.18\frac{c_s^4}{G^{3/2}P_{\rm ext}^{1/2}}$$

**Accretion.** The inside-out collapse of a singular isothermal sphere gives a constant accretion rate (Shu 1977)

$$\dot M=0.975\frac{c_s^3}{G}\approx1.6\times10^{-6}\,M_\odot/{\rm yr}\ \text{for }T=10\ {\rm K}$$

and the accretion luminosity is $L_{\rm acc}=GM\dot M/R_*$.

**Hayashi track.** Fully convective stars lie close to a nearly vertical line in the HR diagram at $T_{\rm eff}\approx3000$ to 4500 K (depending on mass): to the right of it no hydrostatic equilibrium exists (the *forbidden region*). Reason: a fully convective star is well described by an $n=3/2$ polytrope joined to an atmosphere with a strongly temperature-dependent opacity ($\kappa\propto\rho^{1/2}T^{9}$ for H$^-$), which pins $T_{\rm eff}$.

**Kelvin-Helmholtz contraction and deuterium burning.** The star contracts on $t_{\rm KH}=GM^2/RL$, releasing $\sim$ half the gravitational energy. Deuterium ignites at $T\approx10^6$ K, acting as a thermostat that holds the radius at $R_*\sim2$ to $5\,R_\odot$ during accretion.

**Brown dwarfs.** The hydrogen-burning minimum mass is about $0.07$ to $0.075\,M_\odot$ (about $75$ to $80\,M_{\rm Jup}$); deuterium burning occurs down to about $13\,M_{\rm Jup}$.

**Initial mass function (IMF).** Salpeter: $dN/dm\propto m^{-2.35}$; Kroupa: broken power law with slopes about $1.3$ ($0.08$ to $0.5\,M_\odot$) and $2.3$ above; Chabrier: lognormal below $1\,M_\odot$ and a power law above.

## Hands-on problems

**1. [E] Jeans and free-fall.** Compute $M_J$, $\lambda_J$ and $t_{\rm ff}$ for typical conditions: a diffuse cloud ($T=100$ K, $n=100$ cm$^{-3}$) and a dense core ($T=10$ K, $n=10^4$ cm$^{-3}$). For the core, you should find a Jeans mass of a few solar masses and a free-fall time of order $3\times10^5$ yr. Compare with Shu's accretion rate.

**2. [E] Accretion luminosity.** Compute $L_{\rm acc}$ for a $1\,M_\odot$ protostar of radius $2\,R_\odot$ at $\dot M=10^{-5}\,M_\odot$/yr, and compare with the luminosity of the Sun. You should find tens of solar luminosities.

**3. [M] Bonnor-Ebert spheres.** Solve the isothermal Lane-Emden equation numerically, locate the maximum of the external pressure as a function of central density (critical sphere), and verify $\xi_{\max}\approx6.45$ and the factor $1.18$ in the critical mass.

**4. [M] Hayashi track analysis.** Derive the slope of the Hayashi track in the HR diagram from a fully convective polytrope joined to a photosphere with $\kappa\propto\rho^aT^b$. Then compute fully convective models of different masses numerically with your structure code and find the $T_{\rm eff}$ of the Hayashi line for $0.1$, $0.5$ and $1\,M_\odot$.

**5. [M] Pre-main-sequence evolution.** Start from a fully convective $1\,M_\odot$ model with $L\sim10\,L_\odot$ and evolve it to the ZAMS with your code. Include deuterium burning (extend the network), the non-gray atmosphere boundary (Module 06), and the full EOS. Plot the track in the HR diagram: the Hayashi descent, the transition to a radiative core (Henyey track), and arrival on the ZAMS after roughly $3\times10^7$ yr (verify against published tracks). Do the same for $0.1$, $0.5$, $2$ and $5\,M_\odot$ and plot the isochrone at $1$ and $10$ Myr.

**6. [M] Brown dwarfs.** Build models from $0.01$ to $0.2\,M_\odot$ with the degenerate-electron EOS and a non-gray atmosphere. Compute the mass-radius relation at 1 Gyr: it should show a plateau near one Jupiter radius with a minimum near the hydrogen-burning limit. Find the minimum mass for stable hydrogen burning by evolving $0.06$ to $0.09\,M_\odot$ models to 10 Gyr and checking whether they reach thermal equilibrium (about $0.07$ to $0.075\,M_\odot$; verify the dependence on metallicity).

**7. [M] IMF and stellar populations.** Sample $10^5$ stars from the Kroupa IMF and compute the number of massive stars, the mean stellar mass and the mass fraction in stars above $8\,M_\odot$. Estimate the total luminosity and the energy output of winds and supernovae relative to the binding energy of a molecular cloud core of a given mass.

**8. [H] Collapse hydrodynamics.** Write a one-dimensional spherical Lagrangian hydrodynamic code (with artificial viscosity and self-gravity) for the isothermal collapse of a supercritical Bonnor-Ebert sphere. Verify the Larson-Penston supersonic inflow and the formation of an $r^{-2}$ density profile. Add a barotropic equation of state with a switch at $\rho\sim10^{-13}$ g/cm$^3$ (optical depth) to see the formation of the first adiabatic core. Compare the accretion rate with Shu's $0.975\,c_s^3/G$.

**9. [H] Accreting protostars.** Add accretion to your evolution code: new mass is added at the surface with a prescribed specific entropy (cold or hot accretion, with a free parameter) and prescribed $\dot M$. Evolve from a $0.1\,M_\odot$ seed at $\dot M=10^{-5}\,M_\odot/{\rm yr}$ to $1\,M_\odot$. Observe the swelling of the protostar, the onset of deuterium shell burning and the position on the birthline in the HR diagram. Discuss the dependence on the accretion entropy and on the accretion history.

## Software component

- `starlite/pms/`: initial-model builder (fully convective polytropic starting model scaled to a given mass and radius), accretion boundary condition and the deuterium network extension.
- `starlite/tools/imf.py`: IMF samplers and moments.

## Checks

- Jeans mass for the dense core of a few solar masses; free-fall time of order a few $10^5$ yr.
- $\xi_{\max}\approx6.45$ and $1.18$ in $M_{\rm BE}$.
- Hayashi track at $3000$ to $4500$ K for the masses in problem 4.
- $1\,M_\odot$ reaches the ZAMS in a few $10^7$ yr; $0.1\,M_\odot$ in about 1 Gyr (verify).
- Brown dwarf radius plateau and hydrogen-burning limit near $0.07\,M_\odot$.

## Pitfalls

- Starting pre-main-sequence models with the wrong initial entropy or radius: the first few Myr of the track depend on it ("initial condition problem").
- Using the Eddington boundary condition for cool low-mass stars, where molecular opacities make it very inaccurate.
- Forgetting deuterium burning, which matters for the accretion phase and for brown dwarfs.

## Gate questions

1. Why is there a forbidden region to the right of the Hayashi track?
2. Why does the pre-main-sequence phase end earlier for more massive stars, and why do massive stars never have a Hayashi phase visible in clusters?
3. Why is the minimum mass for hydrogen burning set by electron degeneracy?

## Deliverable

`starlite/pms/`, `starlite/tools/imf.py`, `notebooks/10_pms.ipynb`, solutions for problems 1 to 9.
