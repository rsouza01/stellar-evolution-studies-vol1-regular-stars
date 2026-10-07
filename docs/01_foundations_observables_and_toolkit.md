# Module 01: Foundations, observables and the numerical toolkit

**Difficulty:** Easy. **Time:** about 1 week.

## Goal

Know what is observed about stars, what the theory must explain, the characteristic scales and timescales, and build the numerical building blocks (root finders, ODE solvers, interpolation, a block-tridiagonal solver, automatic differentiation) that the stellar code will rest on.

## Prerequisites

Graduate-level physics. Python. You already have a PhD in astrophysics, so treat this module as a calibration of notation and tools.

## Reading

- **KWW, HKT:** the introductory chapters on observed stellar properties, the Hertzsprung-Russell diagram, masses, radii, luminosities and composition.
- **C&O:** magnitudes and colours, the HR diagram, binary systems and the determination of stellar parameters.
- **Pols:** overview of stellar structure and evolution.
- **NR:** root finding, ODE integration, interpolation, sparse and banded linear algebra.

### Task 0: build your reading map

Open the tables of contents of the books you own. In `notes/reading_map.md` fill a table with columns *Module, KWW, HKT, C&O, Pri, CG, Cla, SP, ...*, using the overview's topic list as a first guess. Update it as you go. One hour here saves weeks later, and it becomes the "further reading" list of your own book.

## Concepts

1. **Observables.** Luminosity $L$, effective temperature $T_{\rm eff}$ (defined by $L=4\pi R^2\sigma T_{\rm eff}^4$), radius, mass (only from binaries, asteroseismology or microlensing), surface gravity, composition from spectra, ages from clusters and seismology.
2. **The HR diagram.** Main sequence, red giant branch, horizontal branch, asymptotic giant branch, white dwarf sequence, instability strip.
3. **Mass, radius, luminosity relations.** Roughly $L\propto M^{2.3}$ for $M\lesssim0.4\,M_\odot$, $L\propto M^{4}$ near $1\,M_\odot$, $L\propto M^{3.5}$ for intermediate masses, flattening toward $L\propto M$ for the most massive stars (verify the exponents in your books; you will fit them in problem 4).
4. **Fundamental timescales.** Dynamical (free-fall), thermal (Kelvin-Helmholtz) and nuclear. Their huge separation is why stellar structure can be treated as a sequence of equilibria.
5. **The structure of the problem.** Four differential equations (mass, momentum, energy, transport), constitutive physics (EOS, opacity, nuclear rates), boundary conditions, and a composition that evolves.

## Key quantities (cgs)

| Quantity | Value (verify) |
| --- | --- |
| $G$ | $6.674\times10^{-8}$ |
| $c$ | $2.998\times10^{10}$ |
| $k_B$ | $1.381\times10^{-16}$ |
| $\sigma_{\rm SB}$ | $5.670\times10^{-5}$ |
| $a=4\sigma/c$ | $7.566\times10^{-15}$ |
| $m_u$ | $1.6605\times10^{-24}$ g |
| $M_\odot$ | $1.989\times10^{33}$ g |
| $R_\odot$ | $6.957\times10^{10}$ cm |
| $L_\odot$ | $3.828\times10^{33}$ erg/s |
| $T_{\rm eff,\odot}$ | 5772 K |
| age of the Sun | 4.57 Gyr |

Timescales for a star of mass $M$, radius $R$ and luminosity $L$:

$$t_{\rm ff}\approx\sqrt{\frac{3\pi}{32G\bar\rho}},\qquad t_{\rm KH}\approx\frac{GM^2}{RL},\qquad t_{\rm nuc}\approx\frac{0.007\,f\,Mc^2}{L}$$

where $f$ is the fraction of the mass burned (about 0.1 for the main sequence). For the Sun these are about 30 minutes, $10^7$ years (a few times $10^7$ depending on the factor) and $10^{10}$ years.

Magnitudes: $m-M=5\log_{10}(d/10\,{\rm pc})$, $M_{\rm bol}=-2.5\log_{10}(L/L_\odot)+4.74$.

## Hands-on problems

**1. [E] Constants and units.** Write `const.py` with cgs constants, solar quantities, and conversion helpers (Msun to g, years to s, MeV to erg). Test that $4\pi R_\odot^2\sigma T_{\rm eff}^4$ reproduces $L_\odot$ to 0.1 percent.

**2. [E] Timescales.** For the Sun, a $0.1\,M_\odot$ dwarf, a $10\,M_\odot$ star and a $100\,M_\odot$ star (use approximate radii and luminosities), compute $t_{\rm ff}$, $t_{\rm KH}$, $t_{\rm nuc}$ and tabulate them. Explain why $t_{\rm nuc}/t_{\rm KH}$ is huge for all of them.

**3. [M] The HR diagram from Gaia.** Download a Gaia sample within 100 pc (or use a local catalogue), compute absolute magnitudes and colours, and plot the colour-magnitude diagram. Identify the main sequence, the white dwarf sequence and the giants. Count stars per region. Why does the sample have so few massive stars, and what selection effects enter?

**4. [M] Mass-luminosity relation.** From a catalogue of detached eclipsing binaries with accurately measured masses and radii (DEBCat is the standard compilation; verify access), fit $\log L$ against $\log M$ in mass bins. Report the exponents in each bin and compare with the values quoted above.

**5. [M] Photon diffusion time.** Estimate the photon random-walk escape time of the Sun for a mean opacity $\kappa=1$ and $10\ {\rm cm^2/g}$ with $t\sim3\kappa\bar\rho R^2/(\pi^2c)$ (derive the coefficient). You should find a result between $10^5$ and $10^7$ years. Contrast it with the dynamical time and comment on why the energy is transported by diffusion.

**6. [M] Root finding and ODEs.** Implement and test in `starlite/math/`:
- bracketing root finder (Brent) and multidimensional Newton-Raphson with a backtracking line search;
- RK4 and an adaptive RK45 (Dormand-Prince) compared with `scipy.integrate.solve_ivp`;
- cubic Hermite and monotone (PCHIP) 1D interpolation, and bicubic interpolation on a 2D table;
- a complex-step derivative helper to validate analytic derivatives.

**7. [M] Block-tridiagonal solver.** Implement the block Thomas algorithm for systems with $N$ blocks of size $b\times b$ (use $b=4$ to $10$). Test against dense `numpy.linalg.solve` for random diagonally dominant systems up to $N=2000$. Time it: it should scale as $N\,b^3$. This is the engine of the Henyey method (Module 08).

**8. [H] Automatic differentiation.** Implement forward-mode automatic differentiation with dual numbers (or use JAX), compute the Jacobian of a toy residual (for example an ideal-gas EOS with ionization from Module 03) and compare it with finite differences and with analytic derivatives for accuracy and cost. Decide, with numbers, which approach you will use for the Jacobian of the stellar structure equations.

## Software component

`starlite/const.py` and `starlite/math/` (root finding, ODE integrators, interpolation, block-tridiagonal solver, derivative checkers).

## Checks

- $L_\odot$ from $\sigma T_{\rm eff}^4$ to 0.1 percent; $M_{\rm bol,\odot}=4.74$.
- Block-tridiagonal solver agrees with dense solve to $10^{-10}$ relative and scales linearly in $N$.
- Fitted mass-luminosity exponents close to the quoted ones within the scatter.
- Photon diffusion time between $10^5$ and $10^7$ years.

## Pitfalls

- Mixing SI and cgs, especially in magnitudes and energy units (MeV, erg).
- Interpolating tables in linear rather than logarithmic variables.
- Forgetting that Gaia absolute magnitudes need the extinction and parallax-zero-point corrections for faint or distant stars.

## Gate questions

1. Why can a star be treated as in hydrostatic equilibrium throughout most of its life while it changes on the nuclear timescale?
2. Why is the Kelvin-Helmholtz timescale important for pre-main-sequence stars but irrelevant for the main sequence?
3. What observable provides the only direct dynamical stellar masses, and what limits its precision?

## Deliverable

`const.py`, `math/` with tests, `notebooks/01_foundations.ipynb`, `notes/reading_map.md`, and solutions for problems 1 to 8.
