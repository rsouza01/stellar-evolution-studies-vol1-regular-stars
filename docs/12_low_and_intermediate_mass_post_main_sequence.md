# Module 12: Low and intermediate-mass stars after the main sequence

**Difficulty:** Hard. **Time:** about 3 weeks.

## Goal

Follow a star of roughly 0.8 to 8 solar masses from the end of the main sequence through the red giant branch, the helium flash, core helium burning, the asymptotic giant branch with thermal pulses, mass loss and the formation of a white dwarf. Implement the missing physics (mass loss prescriptions, helium-burning network, dredge-up) in your code.

## Prerequisites

Modules 07, 09 and 11.

## Reading

- **KWW, HKT, Iben, S&C:** shell burning, the red giant branch, the helium flash, the horizontal branch, the asymptotic giant branch and thermal pulses, dredge-up, planetary nebulae.
- **Pri, Pols, C&O:** overviews of post-main-sequence evolution.
- Habing and Olofsson, *Asymptotic Giant Branch Stars*, as an extra reference on AGB stars.

## Key concepts and equations

**Shell burning and the red giant branch.** After hydrogen exhaustion in the core, a hydrogen-burning shell sits on an inert helium core, which for $M\lesssim2.2\,M_\odot$ becomes electron degenerate. The star's envelope expands to hundreds of solar radii while the luminosity is determined almost entirely by the core mass:

$$L\approx2.3\times10^5\,L_\odot\left(\frac{M_c}{M_\odot}\right)^6\ \text{(approximate, low-mass RGB)},\qquad R\approx\frac{3.7\times10^3M_c^4}{1+M_c^3+1.75M_c^4}\,R_\odot$$

(approximate fits from the literature; verify). For $M_c\approx0.45\,M_\odot$ these give $L\approx2\times10^3\,L_\odot$ and $R\approx130\,R_\odot$, the red giant branch tip.

**First dredge-up.** The convective envelope reaches layers where hydrogen burning partly processed the material, bringing $^{13}$C and $^{14}$N to the surface: $^{12}$C/$^{13}$C drops from about 90 to about 20 to 30 and the nitrogen abundance rises (verify).

**The helium flash.** In a degenerate core the temperature rises without the pressure responding, so helium ignition is a thermonuclear runaway ($L_{\rm He}$ reaches $10^9$ to $10^{10}\,L_\odot$ for a few seconds but nearly all the energy lifts the degeneracy of the core and does not reach the surface). The helium-burning region is convective. Afterwards the star settles on the horizontal branch (HB, or red clump for solar metallicity) with a core mass of about $0.5\,M_\odot$ regardless of the initial mass (for low-mass stars).

**Core helium burning.** Triple alpha ($\epsilon\propto T^{40}$) followed by $^{12}$C$(\alpha,\gamma)^{16}$O. Lifetime of order $10^8$ yr. The C/O ratio at the end depends on the uncertain $^{12}$C$(\alpha,\gamma)^{16}$O rate.

**The AGB.** After core helium exhaustion, two shells (He and H) surround a C/O core. The helium shell is thermally unstable (the thin-shell instability): it flashes periodically (**thermal pulses**), separated by interpulse periods of $10^4$ to $10^5$ yr depending on the core mass. The third dredge-up brings carbon (and *s*-process elements) to the surface and can make carbon stars. For $M\gtrsim4$ to $5\,M_\odot$ the base of the envelope is hot enough for **hot bottom burning**. An approximate core-mass luminosity relation for the AGB (Paczynski) is $L\approx5.9\times10^4(M_c/M_\odot-0.52)\,L_\odot$.

**Mass loss.** On the RGB, Reimers' law is $\dot M=4\times10^{-13}\eta\,LR/M\ M_\odot/{\rm yr}$ (solar units) with $\eta\approx0.3$ to $0.6$. On the AGB the rate rises with the pulsation period, up to a *superwind* of $10^{-5}$ to $10^{-4}\,M_\odot$/yr (Vassiliadis and Wood, Bloecker). The envelope is ejected, leaving a hot core: a planetary nebula, then a white dwarf (the Sun ends as a $\approx0.54\,M_\odot$ carbon-oxygen white dwarf; verify).

## Hands-on problems

**1. [E] Core-mass relations.** Compute $L(M_c)$ and $R(M_c)$ from the approximate relations above for $M_c=0.25$ to $0.5\,M_\odot$ on the RGB, and the AGB relation for $M_c=0.55$ to $1.0\,M_\odot$. Compare your results with the actual luminosities and radii from your models in problem 3.

**2. [M] Helium burning network.** Extend the network of Module 07 with the reactions and isotopes needed for helium burning: $3\alpha$, $^{12}$C$(\alpha,\gamma)^{16}$O, $^{14}$N$(\alpha,\gamma)^{18}$F to $^{18}$O, $^{18}$O$(\alpha,\gamma)^{22}$Ne, $^{13}$C$(\alpha,n)^{16}$O and $^{22}$Ne$(\alpha,n)^{25}$Mg. Integrate helium burning at fixed $\rho$ and $T$ and study the final C/O ratio as a function of the $^{12}$C$(\alpha,\gamma)^{16}$O rate (vary it by a factor of 2).

**3. [M] The red giant branch.** Evolve $1\,M_\odot$ and $2\,M_\odot$ models from the ZAMS (Module 11) through the subgiant branch and up the RGB with Reimers mass loss. Record the core mass, $L$, $R$, $T_{\rm eff}$ and the position in the HR diagram. Check the core-mass luminosity relation, the RGB bump (a pause in the luminosity when the convective envelope reaches the former extent of the hydrogen shell), the first dredge-up (surface $^{12}$C/$^{13}$C and N/C versus time) and the RGB tip luminosity. Compare with a published track.

**4. [H] The helium flash.** Take a model near the RGB tip ($M_c\approx0.47\,M_\odot$) and follow the helium flash with your fully coupled solver (Module 09, problem 6): the runaway, the peak luminosity, the convective helium shell and its extension, the lifting of the degeneracy and the subflashes. Choose time steps carefully. Alternatively use the common shortcut: remove the star from the RGB at the flash and relax to a zero-age horizontal branch model; compare the two approaches.

**5. [M] Horizontal branch models.** Build zero-age horizontal branch models for a fixed core mass ($0.48\,M_\odot$) and different envelope masses ($0.01$ to $0.5\,M_\odot$) at Z$=0.001$ and Z$=0.02$. Plot the ZAHB in the HR diagram; explain the horizontal extension (the blue HB for small envelopes) and the red clump for solar metallicity. Evolve them to the end of core helium burning and compute the lifetime.

**6. [H] Thermal pulses.** Evolve a $2$ or $3\,M_\odot$ model through the early AGB and the first 10 to 20 thermal pulses (this will require a robust solver, small time steps, a good mesh in the burning shells and the coupled composition solution). Measure the interpulse period, the peak helium luminosity, the pulse-driven convective zone and the amount of third dredge-up ($\lambda=\Delta M_{\rm dredge}/\Delta M_{\rm core}$). Compare the interpulse period against the core mass relation and analyze the thin-shell instability criterion (derive it from the Schwarzschild-Harm analysis).

**7. [M] Mass-loss prescriptions.** Implement Reimers, Bloecker and Vassiliadis-Wood mass loss (formulas from your books or papers; verify the coefficients) and compare the final masses of $1$, $2$ and $5\,M_\odot$ stars. Compare the resulting initial-final mass relation with the white dwarf mass distribution (peak near $0.6\,M_\odot$).

**8. [M] Dredge-up and surface abundances.** Track the evolution of surface $^{12}$C/$^{13}$C, N/C and C/O through the first and third dredge-up and compare with observed values in giants and carbon stars (verify from Iben or Habing and Olofsson).

**9. [M] The post-AGB phase.** When the envelope mass falls below a threshold, the star leaves the AGB at nearly constant luminosity, crossing the HR diagram toward high $T_{\rm eff}$ in $10^3$ to $10^4$ yr (higher core masses are faster). Follow this transition with your code and plot the path to the planetary nebula nucleus and the white dwarf cooling track.

**10. [H] The Sun's future.** Evolve the calibrated solar model through the RGB, helium flash, horizontal branch and AGB. Tabulate the ages and durations of each phase, the tip luminosity, the maximum radius (about $170$ to $250\,R_\odot$; verify) and the final white dwarf mass. Estimate whether the Earth is engulfed (the question is subtle: tidal effects, mass loss and the orbital expansion matter; discuss qualitatively).

## Software component

- `starlite/winds/`: mass-loss prescriptions (Reimers, Bloecker, Vassiliadis-Wood) selectable per evolution phase.
- Helium-burning network extension (`net`), with options for $^{13}$C$(\alpha,n)$ and $^{22}$Ne$(\alpha,n)$.
- Convective boundary and dredge-up diagnostics in `star/io.py`.
- Thermohaline mixing hook from Module 05.

## Checks

- RGB core mass luminosity relation within about 20 percent of the approximate formula; RGB tip at $L\sim2\times10^3$ to $3\times10^3\,L_\odot$ for $M_c\approx0.47\,M_\odot$.
- First dredge-up: $^{12}$C/$^{13}$C falls from about 90 to about 20 to 30.
- ZAHB core mass about $0.5\,M_\odot$ for stars that undergo the helium flash.
- Thermal pulses with interpulse periods of order $10^4$ to $10^5$ yr.
- Final white dwarf mass about $0.53$ to $0.56\,M_\odot$ for the Sun.

## Pitfalls

- Treating the helium flash with time steps that are too large, resulting in unphysical energy release.
- Switching mass loss formulas between phases without smoothing.
- Taking dredge-up results at face value: they depend strongly on the treatment of convective boundaries and overshoot.
- Insufficient mesh resolution in the thin burning shells.

## Gate questions

1. Why does the luminosity of a giant depend almost only on its core mass?
2. Why does the helium flash occur in a degenerate core and not in a non-degenerate one?
3. What causes thermal pulses and why do they recur?

## Deliverable

`starlite/winds/`, extended networks, tracks for $1,2,5\,M_\odot$ from the ZAMS to the white dwarf, `notebooks/12_post_ms.ipynb`, solutions for problems 1 to 10.
