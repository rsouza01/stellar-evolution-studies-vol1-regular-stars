# Module 05: Opacity, energy transport and convection

**Difficulty:** Medium. **Time:** about 3 weeks.

## Goal

Understand which processes dominate the opacity in different regimes, how energy is transported by radiation, conduction and convection, and implement tabulated opacities and a mixing-length convection routine that returns the actual temperature gradient. This is where "what is the temperature gradient $\nabla$?" gets answered in the structure code.

## Prerequisites

Modules 02, 03 and 04.

## Reading

- **KWW, HKT, CG:** opacity (bound-bound, bound-free, free-free, electron scattering, H$^-$, conduction), energy transport, the criteria for convection (Schwarzschild and Ledoux), mixing-length theory, overshooting, semiconvection, thermohaline mixing.
- **BV:** convection and the mixing-length treatment.
- **RL:** electron scattering (Thomson), bremsstrahlung, conduction.

## Key equations (derive)

**Electron scattering and Kramers opacities** (order of magnitude; verify the coefficients in your books):

$$\kappa_{\rm es}=0.2(1+X)\ {\rm cm^2/g},\qquad\kappa_{\rm ff}\approx3.7\times10^{22}(1+X)(1-Z)\rho T^{-3.5},\qquad\kappa_{\rm bf}\propto Z(1+X)\rho T^{-3.5}$$

**Total opacity with conduction.** Radiative and conductive opacities combine as

$$\frac1\kappa=\frac1{\kappa_{\rm rad}}+\frac1{\kappa_{\rm cond}}$$

(conduction dominates in degenerate matter).

**Radiative gradient and the Eddington luminosity:**

$$\nabla_{\rm rad}=\frac{3}{16\pi acG}\frac{\kappa LP}{mT^4},\qquad L_{\rm Edd}=\frac{4\pi cGM}{\kappa}$$

**Convection criteria.** The layer is convectively unstable if

$$\nabla_{\rm rad}>\nabla_{\rm ad}\quad\text{(Schwarzschild)},\qquad\nabla_{\rm rad}>\nabla_{\rm ad}+\frac{\varphi}{\delta}\nabla_\mu\quad\text{(Ledoux)}$$

with $\nabla_\mu=d\ln\mu/d\ln P$, $\varphi=(\partial\ln\rho/\partial\ln\mu)_{P,T}$ and $\delta=-(\partial\ln\rho/\partial\ln T)_{P,\mu}$.

**Mixing-length theory (MLT).** Convective blobs travel a mixing length $\ell=\alpha_{\rm MLT}H_P$ with $H_P=-dr/d\ln P$. The convective flux and velocity are

$$F_{\rm conv}=\rho c_PT\sqrt{g\delta}\,\frac{\ell^2}{4\sqrt2\,H_P^{3/2}}(\nabla-\nabla_e)^{3/2},\qquad v_{\rm conv}\propto\ell\sqrt{g\delta/H_P}\,(\nabla-\nabla_e)^{1/2}$$

where $\nabla_e$ is the gradient of the rising element. The total flux condition $F_{\rm rad}+F_{\rm conv}=F_{\rm tot}$ together with the energy loss of the blob by radiation closes the system as a **cubic equation** for $\sqrt{\nabla-\nabla_e}$. *Derive* the cubic from the bubble's energy balance (the details of the radiative loss of a bubble in Böhm-Vitense's version) and check it against your books before you implement it. Variants (Henyey, Cox-Giuli, Ledoux) differ by order unity coefficients.

**Overshooting.** Convective boundaries are not sharp. Two common parameterizations: *step* overshoot of length $\ell_{\rm ov}=\alpha_{\rm ov}H_P$, and *exponential* diffusive overshoot with $D_{\rm ov}=D_0\exp(-2z/f_{\rm ov}H_P)$ (Herwig 2000 style).

## Hands-on problems

**1. [E] Opacity regimes.** Implement $\kappa_{\rm es}$, Kramers $\kappa_{\rm ff}$ and $\kappa_{\rm bf}$ for a solar mixture and plot $\log\kappa$ over $(\log\rho,\log T)$, marking the regions dominated by each. Compute the Eddington luminosity of $1$, $10$ and $100\,M_\odot$ stars with $\kappa_{\rm es}$. You should find about $3.8\times10^4\,L_\odot(M/M_\odot)$ for $X=0.7$ ($\kappa\approx0.34$ cm$^2$/g).

**2. [E] The radiative gradient in polytropes.** For the polytropic Sun of Module 02 with Kramers opacity, compute $\nabla_{\rm rad}(m)$ and compare with $\nabla_{\rm ad}=0.4$. Where is the star convectively unstable? How does the answer depend on the nuclear energy generation concentration (compare $\epsilon\propto T^4$ and $\epsilon\propto T^{16}$)?

**3. [M] Tabulated opacities.** Obtain radiative opacity tables (the OPAL and Opacity Project tables for high temperatures and the Ferguson et al. tables for low temperatures are the standard sources; verify availability and licensing). Write a parser, and interpolate $\log\kappa$ in $(\log R,\log T)$ with $R=\rho/T_6^3$ and in $(X,Z)$ with bicubic splines. Blend smoothly between the high- and low-$T$ tables and add electron conduction (use a published fit). Verify smoothness: no kinks in $\partial\ln\kappa/\partial\ln\rho$ and $\partial\ln\kappa/\partial\ln T$, which would wreck the Newton solver.

**4. [M] Implementing MLT.** Derive and solve the MLT cubic. Return $\nabla$, $\nabla_e$, the convective velocity, the flux fraction carried by convection and the derivatives needed for the Jacobian. Test: (a) deep in a convective zone $\nabla\to\nabla_{\rm ad}$ and $F_{\rm conv}\approx F_{\rm tot}$; (b) in the outer superadiabatic layer $\nabla>\nabla_{\rm ad}$ substantially and $v_{\rm conv}$ is of order 1 to 2 km/s in the solar case; (c) smooth transition across the Schwarzschild boundary.

**5. [M] Solar envelope.** Integrate the structure equations inwards from the photosphere through the solar convective envelope (with your EOS, opacity and MLT) at given $L_\odot$, $M_\odot$, $T_{\rm eff}$ and see how the depth of the convective zone depends on $\alpha_{\rm MLT}$ and the surface radius depends on it. The observed base of the solar convection zone is at about $0.713\,R_\odot$ (helioseismology); the radius is the quantity to match by adjusting $\alpha_{\rm MLT}$ later (Module 11).

**6. [M] Ledoux versus Schwarzschild and semiconvection.** In a model with a composition gradient (for example the edge of a convective core), apply both criteria and identify the semiconvective region where Ledoux says stable and Schwarzschild unstable. Implement a simple semiconvective diffusion coefficient (Langer et al. 1983 style, with efficiency parameter $\alpha_{\rm sc}$) and describe its effect on core growth.

**7. [M] Overshooting.** Implement step and exponential overshoot as an extra mixing coefficient and compute, for a $2\,M_\odot$ ZAMS model (once Module 08 is done), how the core size and the main-sequence width change with $\alpha_{\rm ov}=0,0.1,0.2$.

**8. [H] Thermohaline mixing.** Implement the thermohaline diffusion coefficient of Ulrich and Kippenhahn, $D_{\rm th}\propto K_T(-\nabla_\mu)/(\nabla-\nabla_{\rm ad})$, with $K_T=4acT^3/(3\kappa\rho^2c_P)$ the thermal diffusivity. Verify the form from KWW or Maeder and observe where $\nabla_\mu<0$ occurs (above the hydrogen shell in red giants). It will be applied in Module 12.

**9. [H] Convection beyond the local theory.** Read about time-dependent and non-local convection. Implement the "MLT++" idea (reducing the superadiabaticity in radiation-dominated envelopes of massive stars) as an option, and explain why it is needed when $L$ approaches $L_{\rm Edd}$.

## Software component

- `starlite/kap/`: tabulated radiative opacity, conductive opacity, combination, and derivatives.
- `starlite/mlt/`: MLT with Schwarzschild and Ledoux criteria, overshoot, semiconvection and thermohaline diffusion coefficients, returning $\nabla$, $v_{\rm conv}$ and diffusion coefficients with derivatives.

## Checks

- $L_{\rm Edd}\approx3.8\times10^4\,L_\odot(M/M_\odot)$ for $\kappa=0.34$ cm$^2$/g.
- Opacity interpolation $C^1$ continuous across table boundaries.
- MLT: $\nabla\to\nabla_{\rm ad}$ in the deep interior and $v_{\rm conv}$ of order a km/s in the solar superadiabatic layer.
- Smooth switching at the convective boundary.

## Pitfalls

- Using $\nabla_{\rm rad}$ that includes only radiation when conduction is important.
- Noisy opacity derivatives that stall the Newton iteration.
- Treating the MLT parameter $\alpha_{\rm MLT}$ as a universal constant; it is calibrated and depends on the input physics.
- Applying the Ledoux criterion without a consistent $\nabla_\mu$ from the composition profile.

## Gate questions

1. Why does the opacity peak near $10^5$ K at low density and why does this matter for envelopes?
2. In which stars and in which layers do you expect convective cores and convective envelopes, and why?
3. What physical processes the mixing-length theory ignores, and when are those important?

## Deliverable

`starlite/kap/`, `starlite/mlt/` with tests, `notebooks/05_opacity_convection.ipynb`, solutions for problems 1 to 9.
