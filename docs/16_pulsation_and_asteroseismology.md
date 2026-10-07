# Module 16: Pulsation and asteroseismology

**Difficulty:** Hard. **Time:** about 2 weeks.

## Goal

Compute stellar oscillation modes from your stellar models, interpret them with asymptotic theory, understand the excitation mechanisms that populate the instability strip, and use seismic observables (large separation, frequency of maximum power, period spacings) as a precision test of the structure code.

## Prerequisites

Modules 02, 08 and 11.

## Reading

- **ACK, Unno, ChD (*Lecture Notes on Stellar Oscillations*):** linear adiabatic oscillations, p and g modes, asymptotic theory, helio- and asteroseismology.
- **HKT, KWW, C&O:** radial pulsation, the instability strip, excitation mechanisms ($\kappa$, $\gamma$, convective driving).
- Cox, *Theory of Stellar Pulsation* (extra reference).

## Key equations (derive)

**Radial adiabatic pulsation.** With $\xi=\delta r/r$ and eigenfrequency $\omega$ (derive; verify against your books) the equation is a Sturm-Liouville problem

$$\frac{d}{dr}\left(\Gamma_1Pr^4\frac{d\xi}{dr}\right)+\left[\omega^2\rho r^4+r^3\frac{d}{dr}\left((3\Gamma_1-4)P\right)\right]\xi=0$$

with regularity at the centre ($\xi$ finite) and $\delta P=0$ at the surface. For a homogeneous sphere the fundamental mode is $\omega^2=\frac{4\pi}{3}G\bar\rho\,(3\Gamma_1-4)$, which gives the stability limit $\Gamma_1=4/3$.

**Period-density relation.** $\Pi\sqrt{\bar\rho/\bar\rho_\odot}=Q$ with $Q\approx0.03$ to $0.04$ d for classical pulsators (verify).

**Non-radial modes.** Degree $\ell$, radial order $n$, azimuthal order $m$. Two characteristic frequencies govern propagation:

$$L_\ell^2=\frac{\ell(\ell+1)c_s^2}{r^2},\qquad N^2=g\left(\frac1{\Gamma_1}\frac{d\ln P}{dr}-\frac{d\ln\rho}{dr}\right)$$

p modes propagate where $\omega>L_\ell,N$; g modes where $\omega<L_\ell,N$. In the *Cowling approximation* (neglecting the perturbation of the gravitational potential) the system is two first-order ODEs for $\xi_r$ and $\delta P$.

**Asymptotic relations.** For high-order p modes:

$$\nu_{n\ell}\approx\Delta\nu\left(n+\frac\ell2+\epsilon\right),\qquad\Delta\nu=\left(2\int_0^R\frac{dr}{c_s}\right)^{-1}\propto\sqrt{\frac{M}{R^3}}$$

For the Sun $\Delta\nu\approx135\ \mu$Hz. The frequency of maximum power scales as $\nu_{\max}\propto MR^{-2}T_{\rm eff}^{-1/2}$ ($\approx3090\ \mu$Hz for the Sun). For high-order g modes in the asymptotic regime, the periods are equally spaced:

$$\Pi_{n\ell}\approx\frac{\Pi_0}{\sqrt{\ell(\ell+1)}}(n+\epsilon_g),\qquad\Pi_0=2\pi^2\left(\int\frac{N}{r}dr\right)^{-1}$$

The g-mode period spacing of red giants ($\ell=1$ mixed modes) distinguishes hydrogen-shell-burning giants ($\Delta\Pi_1\sim60$ to 100 s) from helium-burning clump stars ($\sim250$ to 300 s; verify).

**Excitation.** The $\kappa$ mechanism: in a partial ionization zone where opacity increases upon compression, the layer traps heat and drives the oscillation ("heat engine"); Cepheids, RR Lyrae, $\delta$ Scuti and $\beta$ Cephei stars are examples. The *Cepheid period-luminosity relation* ($M_V\approx-2.43(\log P-1)-4.05$; verify) follows from the period-density relation and the mass-luminosity relation.

## Hands-on problems

**1. [E] Homogeneous sphere.** Solve the radial pulsation equation numerically for a homogeneous sphere and verify $\omega^2=\frac{4\pi}{3}G\bar\rho(3\Gamma_1-4)$ for $\Gamma_1=5/3$ and $1.4$, and the stability limit.

**2. [M] Radial modes of polytropes.** Solve the radial equation for $n=3$ and $n=3/2$ polytropes with $\Gamma_1=5/3$ using shooting or a relaxation method. Compute the dimensionless frequencies $\omega^2R^3/GM$ of the first five radial modes and compare with tabulated values in the literature. Convert to $Q$ for a Cepheid model and compare with the observed value.

**3. [M] Characteristic frequencies in the Sun.** From your solar model compute $L_\ell(r)$ for $\ell=0$ to $3$, $N(r)$ and make the propagation diagram. Identify the p-mode and g-mode cavities and the convective zone where $N^2<0$.

**4. [M] Asymptotic quantities.** Compute $\Delta\nu=\left[2\int dr/c_s\right]^{-1}$ for your solar model (about 135 $\mu$Hz), and for models of $1$, $1.5$ and $2\,M_\odot$ along the main sequence. Verify the scaling $\Delta\nu\propto\sqrt{\bar\rho}$. Calculate $\Pi_0$ for the radiative core of the Sun and for a red clump star.

**5. [H] Non-radial adiabatic solver.** Implement the full non-radial adiabatic equations (the fourth-order system without the Cowling approximation) for the structure of your solar model, using either shooting with node counting or a finite-difference relaxation method with a block-tridiagonal solver (reuse Module 01). Compute the frequencies for $\ell=0$ to $3$ and $n=1$ to $30$. Compare with the observed solar frequencies (BiSON or GOLF/MDI tables; verify availability) and with the published results for Model S. Frequencies should agree to within about $0.5$ percent at low frequency; at high frequency the discrepancy grows (the "surface effect"). Correct it with a power-law surface term and report the residuals.

**6. [M] Period spacings of red giants.** Take your $1.5\,M_\odot$ model on the RGB and in the red clump. Compute $N(r)$, the asymptotic $\Delta\Pi_1$, and the observable characteristics. Verify that the clump has a larger period spacing than the RGB; find the physical origin (the convective core and the different $N$ profile).

**7. [M] Scaling relations.** Compute $\Delta\nu$ and $\nu_{\max}$ across your grid of models, from the main sequence to the red giant branch, and compare with the scaling relations from the solar values. Examine the deviations (corrections to the $\Delta\nu$ scaling of a few percent for giants), and discuss how they affect mass and radius estimates.

**8. [H] The $\kappa$ mechanism.** Implement the quasi-adiabatic work integral to compute the growth or damping rates of radial modes. Compute the opacity profile of a Cepheid model ($5\,M_\odot$, $L\sim2000\,L_\odot$, $T_{\rm eff}\sim5500$ to $6500$ K) and show that the fundamental mode is driven by the helium II ionization zone. Map the instability strip in the HR diagram by repeating for a grid in $T_{\rm eff}$; compare with the observed strip.

**9. [H] Period-luminosity relation.** Build a set of models along the blue loop of intermediate-mass stars (Module 12) and compute their fundamental period. Derive the theoretical period-luminosity-colour relation, and compare its slope with the Leavitt law.

## Software component

`starlite/pulse/`: reads a stellar profile (the same HDF5 format as `star/io.py`), computes characteristic frequencies, radial and non-radial adiabatic modes, asymptotic quantities, and (optionally) quasi-adiabatic growth rates. Keep an interface compatible with external oscillation codes such as GYRE, so that you can cross-check.

## Checks

- Homogeneous-sphere frequencies to $10^{-6}$.
- Solar $\Delta\nu\approx135\ \mu$Hz and $\nu_{\max}$ scaling reproduced.
- Solar low-degree frequencies within about $0.5$ percent of observed ones at low frequency, with a surface-effect pattern at high frequency.
- Red clump period spacing larger than that of the RGB.
- Instability strip at about the observed temperature range.

## Pitfalls

- Using a stellar model with too few zones (oscillation eigenfunctions need a finer mesh than the structure).
- Confusing the radial order conventions between codes.
- Applying the scaling relations outside their calibrated range.

## Gate questions

1. What physical information does the large frequency separation carry, and why?
2. Why does the $\kappa$ mechanism work only in particular layers of particular stars?
3. Why do the g modes of red giants probe the core while the p modes mostly probe the envelope?

## Deliverable

`starlite/pulse/`, `notebooks/16_pulsation.ipynb`, solutions for problems 1 to 9.
