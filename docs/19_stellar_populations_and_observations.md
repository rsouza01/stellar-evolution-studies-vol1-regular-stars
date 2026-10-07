# Module 19: Stellar populations and comparison with observations

**Difficulty:** Medium. **Time:** about 2 weeks.

## Goal

Turn your stellar tracks into observables: isochrones, colour-magnitude diagrams, bolometric corrections, luminosity functions and simple stellar populations; then confront them with real star clusters and field stars, including parameter estimation with uncertainties. This is the validation layer for the whole code.

## Prerequisites

Modules 06, 10, 11, 12, 13.

## Reading

- **S&C:** isochrones, colour-magnitude diagrams, cluster ages, population synthesis, evolutionary sequences in clusters.
- **Pols, C&O, Gray:** photometric systems, colours and bolometric corrections.
- Greggio and Renzini, *Stellar Populations*, as an extra reference.
- **KWW, HKT:** comparison of models with clusters.

## Key concepts

**Tracks and isochrones.** An evolutionary *track* follows one mass in time. An *isochrone* is the locus at a given age of a set of masses. To build smooth isochrones, interpolate between tracks using **equivalent evolutionary points** (EEPs): physically defined phases (ZAMS, turn-off, base of the RGB, RGB tip, ZAHB, etc.) that map tracks of different masses to a common "primary" coordinate before interpolating in mass.

**Observables.** $M_{\rm bol}=-2.5\log(L/L_\odot)+4.74$; the *bolometric correction* $BC_X=M_{\rm bol}-M_X$ in filter $X$ from model atmospheres (Module 06); colours; the distance modulus $\mu=5\log_{10}(d/10\,{\rm pc})$; the extinction $A_V$ and reddening $E(B-V)$ with $A_V\approx3.1E(B-V)$.

**Cluster morphology.** Main sequence, turn-off (TO), subgiant branch, red giant branch, horizontal branch (HB, red clump), asymptotic giant branch, blue stragglers, white dwarf sequence. The age is set mainly by the luminosity of the TO and by the colour difference between the TO and the RGB. The turn-off mass of a globular cluster at 12 to 13 Gyr is $\approx0.8\,M_\odot$ for low metallicity. Example ages: Pleiades about 125 Myr; Hyades about 650 to 750 Myr; old globular clusters 11 to 13 Gyr (verify).

**IMF and luminosity functions.** Salpeter, Kroupa and Chabrier IMFs (Module 10); the stellar luminosity function from the IMF, tracks and star formation history; simple stellar populations (SSP): integrated luminosity, colours and mass-to-light ratio as a function of age and metallicity.

## Hands-on problems

**1. [E] Photometry from spectra.** Take synthetic spectra from a public grid or your own atmosphere models (Module 06) and compute magnitudes in several filters (Johnson-Cousins $UBVRI$, 2MASS $JHK$, Gaia $G$, $G_{BP}$, $G_{RP}$) using the filter curves (from the SVO filter service; verify). Compute the Sun's magnitudes and colours ($BP-RP\approx0.82$, $G\approx4.67$) and build $BC(T_{\rm eff},\log g,{\rm [Fe/H]})$ tables. Check a few against published bolometric corrections.

**2. [M] EEP-based isochrones.** Choose and implement EEPs for low- and intermediate-mass stars (and massive stars). Construct isochrones from your track grid for ages $10^8$ to $1.3\times10^{10}$ yr, for several metallicities. Check smoothness at the turn-off, the hook, the RGB base and the HB. Verify that the turn-off mass at 12.5 Gyr and low metallicity is about $0.8\,M_\odot$.

**3. [M] An open cluster.** Take Gaia data for the Pleiades, Hyades or NGC 188 with membership from a published catalogue (verify the source), correct for distance and extinction, and fit a single isochrone from your grid. Use MCMC (`emcee`) to infer age, metallicity, distance modulus and extinction with uncertainties, using the main-sequence turn-off and the cluster sequence. Compare with the literature ages above. Examine the effect of overshoot, rotation and binaries on the age.

**4. [M] A globular cluster.** Fit a globular cluster CMD (for example M3 or NGC 6397 from Hubble photometry or Gaia, verify availability) including the red giant branch and horizontal branch. Determine the age (using the TO and subgiant branch, which are least sensitive to mixing-length uncertainties) and discuss the HB morphology and its connection to the red giant mass loss from Module 12. Report the age with its systematic uncertainty.

**5. [M] Luminosity function.** Generate a synthetic population from a Kroupa IMF and a constant star formation rate over 10 Gyr using your isochrones. Compare the luminosity function with that of nearby stars (the Gaia 100 pc sample of Module 01). Add binaries with a random mass ratio and see how they widen the main sequence.

**6. [M] Simple stellar populations.** Compute integrated colours and mass-to-light ratios of SSPs from 100 Myr to 13 Gyr and $Z$ from $0.0002$ to $0.03$ with your isochrones and atmospheres. Examine the age-metallicity degeneracy and the contribution of AGB stars to the near-infrared light. Compare with published SSP models for a few ages (verify).

**7. [H] Bayesian parameter estimation of field stars.** Implement a probabilistic estimator for single stars: given $T_{\rm eff}$, $\log g$ (or luminosity from parallax), [Fe/H], apparent magnitudes, parallax and optional asteroseismic constraints ($\Delta\nu$, $\nu_{\max}$), infer mass, age, radius and extinction, using your tracks as the forward model, with priors from the IMF and star formation history. Apply it to several Gaia stars with TESS or Kepler asteroseismology (verify data access) and show how the age uncertainty shrinks when seismology is added.

**8. [H] A mock Gaia HR diagram.** Build a mock volume-limited stellar population within 100 pc using a star formation history, the IMF, your isochrones, white dwarf cooling tracks (Module 15) and binary fractions. Compare with the observed Gaia HR diagram: the white dwarf sequence and cooling-track features (crystallization pile-up), the width of the main sequence, the "Jao gap" in the M-dwarf main sequence near $M_G\approx10$ (the signature of the convective transition; verify). Which features does your code reproduce and which not?

## Software component

- `tools/isochrones.py`: EEP definition, track interpolation and isochrone generation.
- `tools/synphot.py`: bolometric corrections, filter convolution and tables.
- `tools/cmdfit.py`: MCMC cluster and star fitting.
- `tools/ssp.py`: simple stellar populations.

## Checks

- Sun: $G\approx4.67$, $BP-RP\approx0.82$.
- Smooth isochrones with a turn-off mass of about $0.8\,M_\odot$ at 12.5 Gyr (low $Z$).
- Pleiades age near 125 Myr, Hyades near 650 to 750 Myr (the order of magnitude and the relative ordering).
- Age of a globular cluster in the 11 to 13 Gyr range with an uncertainty of order 1 Gyr.

## Pitfalls

- Mixing photometric systems (Vega versus AB magnitudes).
- Forgetting extinction and its dependence on colour.
- Interpolating tracks in mass without EEPs, which blurs sharp features.
- Quoting ages without systematic uncertainties from mixing, rotation, opacities, composition and bolometric corrections.

## Gate questions

1. Why are the turn-off and subgiant branch the best age indicators of a star cluster?
2. Why is the age of a globular cluster uncertain at the 1 Gyr level even with perfect photometry?
3. What can asteroseismology add to the age of a single star?

## Deliverable

`tools/isochrones.py`, `tools/synphot.py`, `tools/cmdfit.py`, `tools/ssp.py`, `notebooks/19_populations.ipynb`, solutions for problems 1 to 8.
