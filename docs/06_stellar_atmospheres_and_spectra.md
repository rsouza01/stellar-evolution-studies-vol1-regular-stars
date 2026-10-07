# Module 06: Stellar atmospheres and spectra

**Difficulty:** Medium to hard. **Time:** about 3 weeks.

## Goal

Understand how the observable spectrum forms, how a model atmosphere is built (hydrostatic equilibrium, radiative equilibrium, opacities, line formation), how spectra define the effective temperature scale and the composition, and how the atmosphere supplies the **outer boundary condition** for the stellar interior. Build a small non-gray LTE atmosphere code and a set of boundary-condition options for the capstone.

## Prerequisites

Modules 03, 04 and 05.

## Reading

- **Mih, H&M:** model atmospheres (LTE), radiative equilibrium, temperature correction, line formation, NLTE basics.
- **Gray:** photospheric analysis, line profiles, curve of growth, spectral classification, abundance determination.
- **BV (vol. 1), C&O:** atmospheres, spectral classification, the Saha-Boltzmann sequence.
- **KWW:** outer boundary conditions, the stellar surface and the transition to the envelope.

## Key equations and concepts (derive)

**Hydrostatic atmosphere.** With a constant gravity $g$,

$$\frac{dP}{d\tau}=\frac{g}{\kappa}\ (\text{with }P\text{ the total pressure}),\qquad H=\frac{k_BT}{\mu m_ug}\ \text{(scale height)}$$

For the solar photosphere ($T\approx5800$ K, $\mu\approx1.3$ for the neutral atmosphere, $g=2.74\times10^4$ cm/s$^2$) $H\approx1.4\times10^7$ cm (about 140 km).

**Radiative equilibrium** (constant total flux $\pi F=\sigma T_{\rm eff}^4$, no convection):

$$\int_0^\infty\kappa_\nu(J_\nu-S_\nu)\,d\nu=0,\qquad\int_0^\infty H_\nu\,d\nu=\frac{\sigma T_{\rm eff}^4}{4\pi}$$

The temperature is iterated with a correction scheme (Lucy-Unsold, using the flux error and the radiative-equilibrium error) until both conditions are met. In LTE the source function is $S_\nu=B_\nu(T)$ plus scattering.

**Opacity sources in cool stars:** H$^-$ bound-free and free-free, H bound-free (with Kramers cross sections, $\sigma_n\approx7.9\times10^{-18}\,n\,(\nu_n/\nu)^3\,g_{\rm bf}$ cm$^2$ for hydrogen, with $\nu_n=3.29\times10^{15}/n^2$ Hz), Rayleigh scattering by H and H$_2$, electron (Thomson) scattering $\sigma_T=6.65\times10^{-25}$ cm$^2$, and the line opacity of millions of metal lines (handled with opacity distribution functions or sampling). Use published fits for H$^-$ (for example the polynomial fits tabulated in Gray's book).

**Excitation and ionization (Saha-Boltzmann):** the strengths of hydrogen Balmer lines peak near $T\approx9500$ K (A0 stars) because they require atoms in $n=2$, which is a compromise between excitation and ionization.

**Line formation.** The line profile is the Voigt profile $H(a,v)$ with damping parameter $a$. In the Milne-Eddington model $S=B_0(1+\beta\tau)$ the emergent line depth follows analytically. The equivalent width $W_\lambda$ versus column density $Nf$ follows the *curve of growth*: linear ($W\propto Nf$), flat (saturated, $W\propto\sqrt{\ln Nf}$) and damping ($W\propto\sqrt{Nf}$).

**NLTE two-level atom.** Statistical equilibrium gives $S=(1-\epsilon)\bar J+\epsilon B$ with the thermalization parameter $\epsilon=C_{21}/(C_{21}+A_{21})$ (neglecting stimulated emission), which you solved in Module 04.

**Atmosphere as boundary condition for the interior.** The simplest choice is the Eddington $T(\tau)$ relation integrated to $\tau=2/3$:

$$P_{\rm s}=\frac23\frac{g}{\kappa},\qquad T_{\rm s}=T_{\rm eff}\ \text{at }\tau=\tfrac23$$

More accurate options: the Hopf $T$-$\tau$ relation, the semi-empirical Vernazza-Avrett-Loeser solar relation, integrating an atmosphere with convection, or interpolating $P_{\rm b},T_{\rm b}$ at a fixed optical depth from a grid of detailed model atmospheres (ATLAS, MARCS, PHOENIX, TLUSTY).

## Hands-on problems

**1. [E] Scale heights and structures.** Compute $H$ for a few stars (white dwarf, Sun, red giant, supergiant) from their $T_{\rm eff}$ and $g$ and compare with the stellar radius. Explain why the atmosphere is a thin skin for dwarfs and a considerable fraction of the radius for supergiants.

**2. [E] H$^-$ opacity.** Implement H$^-$ bound-free and free-free opacity from a published fit and compute the Rosseland-like total with H bound-free and electron scattering for a gas of solar composition. Verify that H$^-$ dominates at $T\approx5000$ to 8000 K and that the photospheric opacity of the Sun is about $0.3$ to $0.4$ cm$^2$/g.

**3. [M] Gray atmosphere with hydrostatic equilibrium.** Combine the Eddington (or Hopf) $T(\tau)$ with hydrostatic integration, a Saha ionization calculation and your opacity to produce $P(\tau)$, $\rho(\tau)$ for the Sun. Find where $\nabla_{\rm rad}$ exceeds $\nabla_{\rm ad}$ (the top of the convection zone); it should happen just below the photosphere.

**4. [M] A non-gray LTE atmosphere.** Build a plane-parallel atmosphere with about 20 to 50 frequency points (or opacity bins), H$^-$, H b-f, Rayleigh and electron-scattering opacities, using your Feautrier solver (Module 04). Iterate temperature with Lucy-Unsold corrections until the flux and radiative equilibrium errors fall below $1$ percent. Compare $T(\tau)$ with the Eddington relation and compute the emergent spectrum. Estimate the Sun's colour index $B-V\approx0.65$ approximately from your spectrum and bandpasses.

**5. [M] Line formation.** Write the Voigt function with `scipy.special.wofz`. Compute the emergent line profile for a Milne-Eddington atmosphere and then for the atmosphere of problem 4. Build the curve of growth numerically and identify the linear, flat and damping regimes.

**6. [M] Balmer line strength.** Using Saha and Boltzmann equations compute the relative population of hydrogen in $n=2$ over $T=4000$ to 40000 K at a fixed electron pressure. Reproduce the maximum near 9500 K and explain the spectral sequence OBAFGKM in terms of ionization and excitation.

**7. [M] Boundary-condition comparison.** Implement the Eddington, Hopf and a tabulated (Vernazza-Avrett-Loeser or grid-based) boundary condition for the surface of the structure code, returning $(P_b,T_b)$ at a given optical depth. Evaluate how the stellar radius at fixed $L$ and $M$ changes between them (of order one percent for the Sun, larger for cool stars). This sets the accuracy of the radius in Module 11.

**8. [H] NLTE two-level problem.** Solve the two-level atom with continuum background for several $\epsilon$ using Accelerated Lambda Iteration, and compare the emergent line profile with the LTE result. Show the scattering-induced central depth and the core-to-wing contrast.

**9. [H] Convective atmospheres.** Add MLT convection to the radiative-equilibrium atmosphere (the flux must be carried partially by convection below the photosphere) and see how the temperature structure changes compared with the radiative one. Estimate the impact on the boundary condition and compare with the claim that three-dimensional hydrodynamic atmospheres give a different $T(\tau)$ (read the literature; you do not need to run 3D models).

**10. [H] Synthetic photometry.** Convolve your emergent spectra (and a few tabulated ones from public grids) with filter curves (Johnson-Cousins, 2MASS, Gaia) to compute colours and bolometric corrections. Check that the Sun has $G\approx4.67$ and $BP-RP\approx0.82$ (verify with the Gaia documentation), and build a table $BC(T_{\rm eff},\log g,[{\rm Fe/H}])$ for Module 19.

## Software component

`starlite/atm/`: boundary-condition functions with a common interface `boundary(L, M, R, comp, option) -> (P_b, T_b)`, a small non-gray atmosphere solver, Voigt profile and curve-of-growth tools, and synthetic-photometry routines.

## Checks

- $H_\odot\approx135$ to 150 km.
- Photospheric Rosseland opacity of the Sun about $0.3$ to $0.4$ cm$^2$/g.
- Balmer maximum near 9500 K.
- Emergent flux error below 1 percent after Lucy-Unsold iterations.
- Radius sensitivity to the boundary condition at the one percent level for the Sun.

## Pitfalls

- Using the Eddington relation as if it were exact; its error matters for precise radii.
- Using a constant gravity in atmospheres of extended supergiants where radiation pressure and sphericity matter.
- Taking LTE for granted in hot stars and in strong lines.

## Gate questions

1. Why does the effective temperature correspond to $\tau\approx2/3$ rather than $\tau=1$ in the Eddington model?
2. Why do spectral lines in an LTE atmosphere appear in absorption, and what changes in NLTE?
3. How does the choice of the surface boundary condition affect the stellar radius and why?

## Deliverable

`starlite/atm/` with tests, `notebooks/06_atmospheres.ipynb`, solutions for problems 1 to 10.
