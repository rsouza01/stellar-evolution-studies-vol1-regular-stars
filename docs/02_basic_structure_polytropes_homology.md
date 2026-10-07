# Module 02: Basic structure, the virial theorem, polytropes and homology

**Difficulty:** Easy to medium. **Time:** about 2 weeks.

## Goal

Understand the structure equations, the virial theorem and its consequences (negative heat capacity), and solve the first complete stellar models: polytropes and the Eddington standard model. Derive the scaling relations that explain the mass-luminosity relation.

## Prerequisites

Module 01.

## Reading

- **KWW, HKT, Pols, Pri:** the basic equations (mass conservation, hydrostatic equilibrium, energy conservation, energy transport), the virial theorem, timescales, homology relations.
- **CG, Cha, KWW:** polytropes, the Lane-Emden equation, the Eddington standard model.
- **HKT:** scaling relations and simple stellar models.

## Key equations (derive)

**Lagrangian form** (mass coordinate $m$, the natural variable for evolution):

$$\frac{\partial r}{\partial m}=\frac{1}{4\pi r^2\rho},\qquad \frac{\partial P}{\partial m}=-\frac{Gm}{4\pi r^4}-\frac{1}{4\pi r^2}\frac{\partial^2r}{\partial t^2}$$

$$\frac{\partial L}{\partial m}=\epsilon_{\rm nuc}-\epsilon_\nu-T\frac{\partial s}{\partial t},\qquad \frac{\partial T}{\partial m}=-\frac{GmT}{4\pi r^4P}\nabla,\quad \nabla\equiv\frac{d\ln T}{d\ln P}$$

**Virial theorem** for hydrostatic equilibrium, with the gravitational energy $\Omega$ and the integral of $3P/\rho$ over the mass:

$$\int_0^M\frac{3P}{\rho}\,dm=-\Omega$$

For an ideal monatomic gas, $2E_{\rm int}+\Omega=0$ so the total energy is $E=-E_{\rm int}=\Omega/2<0$: a star that radiates energy contracts and heats up (negative heat capacity). Show also that if the gas is radiation dominated the total energy tends to zero.

**Lane-Emden equation.** With $P=K\rho^{1+1/n}$, $\rho=\rho_c\theta^n$, $r=a\xi$ and $a=\left[(n+1)P_c/(4\pi G\rho_c^2)\right]^{1/2}$,

$$\frac{1}{\xi^2}\frac{d}{d\xi}\left(\xi^2\frac{d\theta}{d\xi}\right)=-\theta^n,\qquad \theta(0)=1,\ \theta'(0)=0$$

The radius is $R=a\xi_1$ with $\theta(\xi_1)=0$ and the mass is $M=4\pi a^3\rho_c(-\xi^2\theta')_{\xi_1}$. Analytic solutions: $n=0$ ($\theta=1-\xi^2/6$, $\xi_1=\sqrt6$), $n=1$ ($\theta=\sin\xi/\xi$, $\xi_1=\pi$), $n=5$ ($\theta=(1+\xi^2/3)^{-1/2}$, $\xi_1=\infty$).

Selected numerical values to reproduce (from memory; verify with Chandrasekhar's tables):

| $n$ | $\xi_1$ | $(-\xi^2\theta')_{\xi_1}$ | $\rho_c/\bar\rho$ |
| --- | --- | --- | --- |
| 1.5 | 3.65375 | 2.71406 | 5.99 |
| 3 | 6.89685 | 2.01824 | 54.18 |

with $\rho_c/\bar\rho=\xi_1/\left(3(-\theta'_{\xi_1})\right)$. The gravitational energy of a polytrope is

$$\Omega=-\frac{3}{5-n}\frac{GM^2}{R}$$

**Eddington standard model.** Take $P_{\rm gas}=\beta P$ with $\beta$ constant through the star. Then it is an $n=3$ polytrope and $\beta$ satisfies the quartic (derive and check against CG)

$$1-\beta\approx0.003\left(\frac{M}{M_\odot}\right)^2\mu^4\beta^4$$

**Homology.** If two stars are homologous, all structure variables at the same relative mass scale by power laws in $M$ and $R$. For an ideal gas, $P\propto M^2R^{-4}$, $T\propto\mu MR^{-1}$, $\rho\propto MR^{-3}$. Using radiative diffusion, $L\propto T^4R/(\kappa\rho)$:
- electron scattering: $L\propto\mu^4M^3$;
- Kramers opacity ($\kappa\propto\rho T^{-3.5}$): $L\propto\mu^{7.5}M^{5.5}R^{-0.5}$.

## Hands-on problems

**1. [E] Lane-Emden solver.** Integrate the equation with `solve_ivp`, starting at a small $\xi_0$ with a series expansion $\theta\approx1-\xi_0^2/6+n\xi_0^4/120$. Detect $\xi_1$ with an event. Reproduce $\xi_1$ for $n=0,1,1.5,3$ and the table above to five digits. For $n=5$ show that the solution is analytic and that numerical integration approaches the analytic curve.

**2. [E] Polytropic Sun.** Build the $n=3$ polytrope for $M=M_\odot$, $R=R_\odot$ and compute $\rho_c$, $P_c$ (it should be about $11\,GM^2/R^4$), and the central temperature with an ideal-gas EOS ($\mu\approx0.6$). You should get $T_c\approx1.2\times10^7$ K versus the true $1.57\times10^7$ K, and $\rho_c\approx76$ g/cm$^3$ versus about 150. Discuss the differences (the Sun is not an exact polytrope).

**3. [M] Energies and the virial theorem.** For $n=1.5$ and $n=3$ compute the gravitational energy numerically by integrating $-\int Gm/r\,dm$, check the analytic formula, and verify the virial theorem $2E_{\rm int}+\Omega=0$ for an ideal gas.

**4. [M] Eddington standard model.** Solve the quartic for $\beta$ over $M=0.1$ to $100\,M_\odot$ with $\mu=0.6$. At $100\,M_\odot$ you should find $\beta\approx0.57$. Plot $1-\beta$ against $M$ and find the mass where radiation pressure is 10, 25 and 50 percent of the total. Explain why this signals the transition to massive-star physics.

**5. [M] Homology exercises.** Derive the mass-radius relation for hydrogen burning ($\epsilon\propto\rho T^\nu$) with (a) electron scattering opacity and (b) Kramers opacity. You should find $R\propto M^{(\nu-1)/(\nu+3)}$ and $R\propto M^{(\nu-3.5)/(\nu+2.5)}$. Evaluate for the pp chain ($\nu\approx4$) and CNO ($\nu\approx16$) and discuss. Compare the resulting $L(M)$ slopes with the observed ones from Module 01.

**6. [M] The Chandrasekhar mass from a polytrope.** The degenerate relativistic electron gas has $P=K\rho^{4/3}$ with $K=1.2435\times10^{15}\mu_e^{-4/3}$ (cgs). Using the $n=3$ polytrope, show that the mass is independent of the central density and equal to $M_{\rm Ch}\approx5.83\,\mu_e^{-2}\,M_\odot$ ($1.456\,M_\odot$ for $\mu_e=2$). Check your own number with the formula $M=4\pi\left(K/\pi G\right)^{3/2}(-\xi^2\theta')_{\xi_1}$.

**7. [H] Stability.** For a homogeneous sphere derive the condition for dynamical stability against homologous radial perturbations, $\Gamma_1>4/3$, using an energy argument: write $E(R)$ and find where $d^2E/dR^2$ changes sign for a polytrope with adiabatic index $\Gamma$. Hold the result for Module 16, where you will recover it from the pulsation equation.

**8. [H] Composite polytropes.** Join an $n=3$ core to an $n=3/2$ envelope with continuity of $P$, $m$ and $r$ at an interface and different mean molecular weights. Find the family of solutions and relate it to a convective-core plus radiative-envelope star and a radiative-core plus convective-envelope star.

## Software component

`starlite/polytrope.py`: Lane-Emden solver returning $(\xi,\theta,\theta')$ and the physical profiles for given $(n,M,R)$, and the Eddington-$\beta$ function. These provide the first guess for the Henyey solver in Module 08 and the analytical tests of the EOS and structure code.

## Checks

- $\xi_1$ and $(-\xi^2\theta')$ for $n=1.5$ and $n=3$ to five digits; $\rho_c/\bar\rho=5.99$ and $54.18$.
- Polytropic Sun: $T_c\approx1.2\times10^7$ K, $P_c\approx1.2\times10^{17}$ dyn/cm$^2$.
- Virial theorem verified numerically for two values of $n$.
- Eddington model: $\beta\approx0.57$ at $100\,M_\odot$ with $\mu=0.6$.
- $M_{\rm Ch}\approx1.456\,M_\odot$ for $\mu_e=2$.

## Pitfalls

- Starting the Lane-Emden integration exactly at $\xi=0$ (singular). Use the series start.
- Using $\mu$ for the ions (nuclei) in one place and $\mu_e$ in another.
- Expecting the polytrope to reproduce the solar central values; it only reproduces the order of magnitude.

## Gate questions

1. Why does a star have negative heat capacity, and what does this imply for its stability and for the pace of its evolution?
2. Why is the mass of an $n=3$ polytrope independent of its radius?
3. Why does the mass-luminosity exponent decrease at high mass?

## Deliverable

`polytrope.py` with tests, `notebooks/02_structure.ipynb`, `notes/derivations/homology.tex`, solutions for problems 1 to 8.
