# Module 11: The main sequence and the Sun

**Difficulty:** Medium. **Time:** about 3 weeks.

## Goal

Use your code to understand the main sequence across the full mass range, calibrate a standard solar model, compare it with helioseismology and solar neutrinos, and reproduce the key physics of hydrogen exhaustion (the Schonberg-Chandrasekhar limit and the Hertzsprung gap).

## Prerequisites

Modules 08 and 09 (and 07 for neutrinos).

## Reading

- **KWW, HKT, Pri, Pols:** the main sequence, homology relations, convective cores and envelopes, the solar model, hydrogen exhaustion.
- **ChD:** the standard solar model, helioseismic constraints, solar neutrinos.
- **S&C, Iben:** main-sequence evolution and its dependence on mass and composition.
- **C&O:** the Sun, solar neutrinos.

## Key concepts and equations

**Structure along the main sequence.**
- $M\lesssim0.35\,M_\odot$: fully convective.
- $0.35\lesssim M\lesssim1.2\,M_\odot$: radiative core, convective envelope; energy from the pp chain.
- $M\gtrsim1.2\,M_\odot$: CNO burning, convective core, radiative envelope; with convective-core overshoot the mass range changes slightly (verify the transition mass for your input physics).

**Lifetimes.** The main-sequence lifetime scales as $t_{\rm MS}\propto M/L\propto M^{-2.5}$ to $M^{-3}$ near the solar mass: about 10 Gyr for $1\,M_\odot$, about 100 Myr for $5\,M_\odot$ and about 10 Myr for $15$ to $20\,M_\odot$ (verify with your own models).

**Standard solar model.** Calibrate the initial helium fraction $Y_0$, the initial metallicity $Z_0$ (equivalently $Z/X$) and the mixing length $\alpha_{\rm MLT}$ so that, at $t=4.57$ Gyr, the model matches $L_\odot$, $R_\odot$ and the present surface $(Z/X)_\odot$. Typical results (verify; the numbers depend on the composition table and the input physics): $Y_0\approx0.27$, $\alpha_{\rm MLT}\approx1.8$ to 2.1, a central temperature $T_c\approx1.57\times10^7$ K, $\rho_c\approx150$ g/cm$^3$, central hydrogen $X_c\approx0.34$, the base of the convection zone at $r\approx0.713\,R_\odot$ and a surface helium abundance $Y_s\approx0.245$ to $0.25$ (reduced from $Y_0$ by helium settling; see Module 17).

**Helioseismic comparison.** Compute the adiabatic sound speed $c_s^2=\Gamma_1P/\rho$ and compare with the inversion from observed oscillation frequencies (Model S and its updates). The agreement of better than 1 percent is the strongest test. The "solar abundance problem": lowering the heavy-element abundances (the Asplund et al. 2009 composition, $Z/X\approx0.018$) worsens the agreement with the older Grevesse-Sauval composition ($Z/X\approx0.023$); this is still an open question.

**Schonberg-Chandrasekhar limit.** An isothermal core under a radiative envelope can only support a pressure up to a maximum core mass fraction:

$$\left(\frac{M_c}{M}\right)_{\rm SC}\approx0.37\left(\frac{\mu_{\rm env}}{\mu_c}\right)^2\approx0.08\ \text{to }0.10$$

Beyond this the core contracts on a thermal timescale, the envelope expands and the star crosses the Hertzsprung gap.

## Hands-on problems

**1. [E] Homology on the ZAMS.** From your ZAMS grid of Module 08, compute the local exponents $d\ln L/d\ln M$ and $d\ln R/d\ln M$ and compare with the homology predictions of Module 02 for electron scattering and Kramers opacity, in the low-, intermediate- and high-mass ranges.

**2. [M] A grid of main-sequence tracks.** Evolve $0.3$, $0.5$, $0.8$, $1$, $1.5$, $2$, $3$, $5$, $10$ and $20\,M_\odot$ (solar metallicity) from the ZAMS to central hydrogen exhaustion. Plot the HR diagram, $X_c(t)$, $T_c(t)$, the convective core mass and the lifetimes. Verify the lifetime scaling and compare with a published grid (MESA, MIST, PARSEC, Geneva; verify).

**3. [M] The convective core.** Plot the convective core mass fraction against stellar mass on the ZAMS and on the way to the TAMS (the terminal-age main sequence). Find the mass at which the core first appears (in your input physics). Study how it grows or shrinks with time, and the "hook" in the tracks at the end of the main sequence. Add overshoot with $\alpha_{\rm ov}=0,0.1,0.2$ and quantify its effect on the lifetime and the track width.

**4. [M] Schonberg-Chandrasekhar limit.** Derive the limit analytically with a two-zone model (an isothermal ideal-gas core of mean molecular weight $\mu_c$ and a radiative envelope of $\mu_{\rm env}$) and verify the factor $0.37(\mu_{\rm env}/\mu_c)^2$. Then identify it in your $1.5$ to $5\,M_\odot$ tracks: the core mass fraction at the start of the rapid contraction phase.

**5. [H] Standard solar model.** Write a driver that adjusts $(Y_0,Z_0,\alpha_{\rm MLT})$ with a Newton or Broyden method until your $1\,M_\odot$ evolution reproduces $L_\odot$, $R_\odot$ and $(Z/X)_\odot$ at $4.57$ Gyr. Report the converged parameters and the quantities $T_c$, $\rho_c$, $X_c$, $r_{\rm cz}$, $Y_s$. Compare with the helioseismic values ($r_{\rm cz}=0.713\pm0.001\,R_\odot$, $Y_s\approx0.2485$; verify). Run it for two composition tables to see the solar abundance problem.

**6. [M] The sound speed profile.** Compute $c_s(r)$ for your calibrated solar model and compare with Model S (or a published inversion). Plot the relative difference $\delta c_s/c_s$ and discuss where you differ (the base of the convection zone, the core) and why (the inclusion of helium and heavy-element settling affects the profile; see Module 17).

**7. [M] Solar neutrinos.** With the network of Module 07 compute the production of the pp, $^7$Be and $^8$B neutrinos as a function of radius in your solar model and the fluxes at Earth. Compare with the published predictions and measurements ($^8$B about $5\times10^6$ cm$^{-2}$s$^{-1}$, pp about $6\times10^{10}$, $^7$Be about $5\times10^9$; verify). Perturb $T_c$ by a few percent and measure the change in the $^8$B flux (the exponent of about 24 to 25).

**8. [H] M dwarfs.** Compute models of $0.1$ to $0.5\,M_\odot$ with non-gray atmosphere boundary conditions, an EOS including non-ideal effects and partial degeneracy. Plot $R(M)$, $L(M)$ and compare with eclipsing-binary data (DEBCat; verify). Observed radii exceed the models by a few to ten percent (the radius inflation problem; verify the magnitude). Explore what could cause it: higher $\alpha_{\rm MLT}$ (can it?), magnetic inhibition of convection (add an ad hoc reduction of the convective flux), starspots, metallicity. Find the fully convective transition near $0.35\,M_\odot$.

**9. [H] Convective core evolution with composition gradients.** In intermediate-mass stars, follow how the convective core retreats as it burns hydrogen (CNO), leaving a composition gradient that can trigger semiconvection or growth by overshoot. Implement the criterion from Module 05 (Ledoux and a semiconvection coefficient) and compare the tracks with and without it.

## Software component

- `tools/grids.py`: running grids of models in parallel with a driver and gathering outputs.
- `tools/solar_calibrate.py`: the solar-calibration driver.
- Regression tests: ZAMS values at several masses, and the solar calibration residuals.

## Checks

- $t_{\rm MS}(1\,M_\odot)\approx10$ Gyr, $t_{\rm MS}(5\,M_\odot)\approx10^8$ yr, $t_{\rm MS}(15\,M_\odot)\approx10^7$ yr.
- Solar model: $L,R$ within $10^{-3}$, $r_{\rm cz}$ within $0.01\,R_\odot$ of $0.713$, $Y_s$ within $0.01$ of $0.2485$, sound speed within 1 percent of Model S.
- Schonberg-Chandrasekhar fraction about 0.08 to 0.10.
- $^8$B neutrino exponent about 24.

## Pitfalls

- Calibrating the Sun with a different input physics from the rest of your grid.
- Neglecting diffusion in the solar model and then wondering about the sound speed discrepancy.
- Interpreting differences from published tracks without checking differences in opacity tables, composition tables, convection treatment and boundary conditions.

## Gate questions

1. Why do stars above about $1.2\,M_\odot$ have convective cores and below it convective envelopes?
2. Why is the main-sequence lifetime so strongly dependent on the mass?
3. Why does the helioseismic sound speed constrain the solar composition and the opacity?

## Deliverable

`tools/grids.py`, `tools/solar_calibrate.py`, a calibrated solar model, track grids and `notebooks/11_main_sequence.ipynb`, solutions for problems 1 to 9.
