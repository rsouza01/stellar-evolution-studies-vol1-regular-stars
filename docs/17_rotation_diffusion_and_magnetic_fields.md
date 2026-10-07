# Module 17: Rotation, atomic diffusion, mixing and magnetic fields

**Difficulty:** Hard. **Time:** about 2 weeks.

## Goal

Add the physics that goes beyond the "standard" non-rotating, non-magnetic model: atomic diffusion and gravitational settling, rotation and angular momentum transport, rotational mixing, magnetic braking and the main uncertainties in internal mixing. Implement them as options in the evolution code.

## Prerequisites

Modules 05, 09 and 11.

## Reading

- **Mae:** rotating stars, the shellular rotation approximation, meridional circulation, shear instabilities, angular momentum transport, effects on massive-star evolution.
- **Tas:** stellar rotation, braking, observed rotation of stars of various types.
- **MdA:** atomic diffusion: settling, radiative accelerations, thermal diffusion.
- **KWW, HKT:** rotation and mixing processes in stellar interiors.

## Key equations and concepts (derive)

**Gravitational settling.** Heavier species sink and helium diffuses inward relative to hydrogen. The diffusion velocity of an element in a plasma follows from the Burgers equations (Thoul, Bahcall and Loeb 1994 give a practical solution; verify). The abundance equation becomes

$$\frac{\partial X_i}{\partial t}=\frac{\partial}{\partial m}\left[(4\pi r^2\rho)^2D\frac{\partial X_i}{\partial m}\right]-\frac{\partial}{\partial m}\left[4\pi r^2\rho\,X_iv_i\right]+\text{reactions}$$

with $v_i$ the diffusion velocity. In the Sun, settling lowers the surface helium from $Y_0\approx0.27$ to $\approx0.245$ and metals by about 10 percent, and deepens the convection zone for a given composition. It is essential to match helioseismology (Module 11).

**Rotation basics.** Critical angular velocity $\Omega_c=\sqrt{GM(1-\Gamma)/R_{\rm eq}^3}$. In a rotating star, the surface flux follows von Zeipel's law $F\propto g_{\rm eff}$, so poles are hotter than the equator (gravity darkening), and the equatorial radius is up to $1.5$ times the polar one at critical rotation.

**Centrifugal corrections in 1D.** In the shellular approximation (rotation is constant on isobars), the structure equations are modified with factors $f_P$ and $f_T$ (Kippenhahn and Thomas 1970; Endal and Sofia 1976):

$$\frac{dP}{dm}=-\frac{Gm}{4\pi r^4}f_P,\qquad\frac{dT}{dm}=-\frac{GmT}{4\pi r^4P}\nabla\,\frac{f_T}{f_P}$$

with $f_P$, $f_T\to1$ as $\Omega\to0$.

**Circulation and shear.** Rotation drives large-scale meridional circulation (Eddington-Sweet) on a timescale $t_{\rm ES}\approx t_{\rm KH}\,\dfrac{GM}{\Omega^2R^3}$, and shear instabilities (the Richardson criterion). Their transport of angular momentum and chemicals is modeled in 1D by

$$\rho\frac{d(r^2\Omega)}{dt}=\frac{1}{5r^2}\frac{\partial}{\partial r}\left(\rho r^4\Omega U\right)+\frac1{r^2}\frac{\partial}{\partial r}\left(\rho Dr^4\frac{\partial\Omega}{\partial r}\right)$$

(advection by the meridional circulation velocity $U$ plus diffusion by turbulence; see Mae) or in the simpler purely diffusive approach with $D_{\rm ES}$, $D_{\rm shear}$ etc. (Heger, Langer and Woosley). **Magnetic transport** (the Tayler-Spruit dynamo) strongly couples the core to the envelope. Observations (the Sun, red giants, white dwarfs and neutron-star spin rates) require a much more efficient transport than purely hydrodynamic processes.

**Magnetic braking.** Winds anchored in the magnetic field carry away angular momentum. The Skumanich law gives $\Omega\propto t^{-1/2}$ for solar-type stars on the main sequence (gyrochronology), and the Sun with $P=25$ d at $4.57$ Gyr is the anchor.

## Hands-on problems

**1. [E] Rotation and gravity darkening.** For a Roche model compute the surface shape and the effective gravity as a function of $\Omega/\Omega_c$. Verify $R_{\rm eq}/R_{\rm pol}=1.5$ at critical rotation. Using von Zeipel's law, compute the temperature difference between pole and equator and the resulting inclination-dependent observed luminosity and colour.

**2. [E] Gyrochronology.** Use the Skumanich law and the Sun's period to estimate the ages of stars with rotation periods of 5 and 10 days (about 0.4 and 1.6 Gyr from $P\propto t^{1/2}$ at fixed colour). Discuss its limitations (the saturation of fast rotators, the observed stalling of spin-down).

**3. [M] Atomic diffusion in the Sun.** Implement helium and heavy-element settling in your solar model using either the Burgers solution (coefficients from the literature) or the Thoul-Bahcall-Loeb approximation. Evolve the calibrated Sun with diffusion and verify: (a) the surface helium falls by about $0.025$ over 4.57 Gyr, (b) the convection zone base moves toward $r\approx0.713\,R_\odot$ and (c) the sound speed profile improves relative to a model without diffusion. Report the effect on the calibrated $Y_0$.

**4. [M] Rotational mixing in massive stars.** Add angular momentum transport and chemical mixing in the diffusive approach (Heger-Langer-Woosley coefficients; verify the prescriptions in Mae). Evolve $15$ and $25\,M_\odot$ stars with initial rotation speeds of $0$, $150$ and $300$ km/s and compute: (a) the surface N/C and N/O enhancements, (b) the extended main-sequence lifetimes, (c) the final core masses. Compare with the observed nitrogen enrichment in rotating OB stars.

**5. [M] Angular momentum conservation and coupling.** Test your angular momentum transport implementation: total angular momentum is conserved without mass loss to $10^{-8}$; strongly coupled models relax toward solid-body rotation; a decoupled core spins up as it contracts. Compare the evolution of a $1.5\,M_\odot$ star's core rotation in the subgiant and red giant phase with and without extra magnetic coupling and with the observed core rotation rates from asteroseismology (which require much stronger coupling than hydrodynamics provides; discuss).

**6. [H] Centrifugal corrections.** Implement $f_P$ and $f_T$ in your structure equations and test: in the limit $\Omega\to0$ you recover the non-rotating models, and for $\Omega/\Omega_c\lesssim0.3$ the changes in $L$, $R$ and $T_{\rm eff}$ are of a few percent. Include a pre-factor for how the effective gravity enters the surface boundary condition. Compare the effect on the main sequence with the effect of rotational mixing.

**7. [H] The solar rotation profile.** Set up angular momentum transport for the Sun with magnetic braking at the surface and evolve from the ZAMS: find whether purely hydrodynamic transport reproduces the helioseismically measured near-solid-body rotation of the radiative zone (it does not; the core would spin too fast). Add a simple extra viscosity or the Tayler-Spruit prescription and calibrate it. Report the viscosity needed.

**8. [H] Thermohaline mixing in giants.** Apply the thermohaline diffusion coefficient of Module 05 to your red giant models. Show how it modifies the surface $^3$He, Li and $^{12}$C/$^{13}$C after the RGB bump compared with the observations of low-mass giants.

## Software component

- `starlite/diff/`: diffusion velocities and coefficients; settling operator integrated in `star/mix.py`.
- `starlite/rot/`: Roche geometry, centrifugal correction factors, angular momentum transport and mixing coefficients; a magnetic braking law.
- Options in the TOML configuration files to switch each physics on and off.

## Checks

- Roche geometry: $R_{\rm eq}/R_{\rm pol}=1.5$ at critical rotation.
- Solar helium settling: surface $Y$ lower by about $0.02$ to $0.03$ than $Y_0$; improved agreement of $c_s$ with helioseismology.
- Angular momentum conserved to $10^{-8}$ in the absence of mass loss.
- The rotating massive star models show surface nitrogen enrichment increasing with the rotation speed.

## Pitfalls

- Doing atomic diffusion with a time step so large that the Burgers solution becomes inconsistent.
- Treating rotation as small without checking $\Omega/\Omega_c$ after contraction (spin-up).
- Forgetting that the transport of angular momentum and the chemical mixing are coupled.

## Gate questions

1. Why does helioseismology require both atomic diffusion and a particular opacity?
2. Why is angular momentum transport in stellar interiors an open problem?
3. Why do rotational mixing and mass loss interact in massive stars?

## Deliverable

`starlite/diff/`, `starlite/rot/`, `notebooks/17_rotation_diffusion.ipynb`, solutions for problems 1 to 8.
