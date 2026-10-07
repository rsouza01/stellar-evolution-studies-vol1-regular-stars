# Appendix: books, software, data, notation and shortcuts

> All values and citations here are from memory. Verify before relying on them.

## 1. Which book for what (module by module)

| Module | First choice | Also useful |
| --- | --- | --- |
| 01 Foundations | KWW, C&O | HKT, Pols |
| 02 Basic structure | KWW, HKT | CG, Cha, Pri, Pols |
| 03 EOS | HKT, CG | KWW, ST, Cha |
| 04 Radiative transfer | Mih, RL | H&M, Gray, KWW |
| 05 Opacity and convection | KWW, HKT | CG, BV, RL |
| 06 Atmospheres | Gray, Mih | H&M, BV, C&O |
| 07 Nuclear physics | Cla, Ili | R&R, KWW, Arn |
| 08 Structure solver | KWW | HKT, Pri, NR, Pols |
| 09 Evolution numerics | KWW | Pri, NR, MESA papers |
| 10 Star formation | SP | KWW, C&O |
| 11 Main sequence and Sun | KWW, ChD | HKT, S&C, Pri |
| 12 Post-MS low mass | Iben, KWW | S&C, HKT, Habing and Olofsson |
| 13 Massive stars | Mae, Arn | L&C, Iben, KWW |
| 14 Nucleosynthesis | Cla, Ili | R&R, Arn, Pagel |
| 15 Remnants and explosions | ST, Arn | Cha, Gle, Branch and Wheeler |
| 16 Pulsation | ACK, Unno | ChD, HKT, Cox |
| 17 Rotation and diffusion | Mae, MdA | Tas, KWW |
| 18 Mass loss and binaries | Egg, Hil | L&C, Iben, KWW |
| 19 Populations | S&C | Greggio and Renzini, Gray |
| 20 Capstone | KWW + NR | MESA documentation |

## 2. Software and data

| Resource | Purpose | Notes |
| --- | --- | --- |
| MESA (Modules for Experiments in Stellar Astrophysics) | validation, design reference | read the instrument papers and the test suite |
| GYRE | oscillation code | cross-check for `pulse` |
| ATLAS, MARCS, PHOENIX, TLUSTY grids | model atmospheres and spectra | boundary conditions and bolometric corrections |
| OPAL, Opacity Project, Ferguson low-$T$ tables | opacities | check licensing and format |
| REACLIB (JINA) | reaction rates | public |
| Helmholtz EOS (Timmes and Swesty), SCVH, FreeEOS | equation of state | cross-checks for `eos` |
| Model S and later solar models; BiSON, GOLF, MDI frequencies | helioseismic validation | verify where to get them |
| KADoNiS | neutron-capture cross sections | $s$-process |
| DEBCat | eclipsing-binary masses and radii | Module 01 and 11 |
| Gaia archive, 2MASS, TESS and Kepler asteroseismic catalogues | observational comparison | Modules 01, 16, 19 |
| MIST, PARSEC, BaSTI, Geneva grids | published tracks and isochrones | cross-code validation |
| SVO filter profile service | filter curves | Module 19 |
| Python: NumPy, SciPy, Numba, JAX, h5py, matplotlib, emcee, astropy | tools | check current versions |

## 3. Notation used in the program

| Symbol | Meaning |
| --- | --- |
| $m,r,P,T,\rho$ | mass coordinate, radius, pressure, temperature, density |
| $L,\epsilon_{\rm nuc},\epsilon_\nu,\epsilon_{\rm grav}$ | luminosity and the energy-generation terms |
| $\nabla=d\ln T/d\ln P$ | actual gradient; $\nabla_{\rm ad}$, $\nabla_{\rm rad}$, $\nabla_\mu$ |
| $\chi_\rho,\chi_T$ | pressure derivatives; $\delta$, $\varphi$ composition derivatives |
| $\Gamma_1,\Gamma_2,\Gamma_3$ | adiabatic exponents |
| $\kappa$ | opacity (Rosseland mean unless noted) |
| $X,Y,Z$ | hydrogen, helium, metal mass fractions; $X_i$ for isotopes |
| $Y_i=X_i/A_i$ | molar abundance |
| $\mu,\mu_e$ | mean molecular weight, per free electron |
| $\alpha_{\rm MLT}$, $\alpha_{\rm ov}$ | mixing length and overshoot parameters |
| $\tau$ | optical depth |

## 4. Writing the book

Suggested conventions:

- **Problem IDs:** `SP-MM.N` (module, number). Keep the same IDs in the repository (`solutions/MM/pNN.py`) and the notebooks.
- **For each problem:** statement, expected numerical result with tolerance, short worked solution, code reference, and (for `[H]` problems) hints and common failure modes.
- **For each chapter:** learning goals, derivations (the "derive" items), worked examples, problems, a "what can go wrong" box (the pitfalls list), and conceptual questions (the gate questions).
- **Tests as exercises:** the check values in each module are natural "reproduce this table" exercises and automatic grading targets.
- **Code and text together:** every equation in the book that the code implements should cite the function and the test that verifies it.

## 5. Pitfalls that recur throughout the program

1. Mixing units (cgs versus SI; MeV versus erg; years versus seconds).
2. Inconsistent thermodynamics (EOS not obeying Maxwell relations) or non-smooth tables, which stall Newton solvers.
3. Jacobians that do not match residuals.
4. Conservation errors in remeshing, mixing and mass loss.
5. Comparing against published models without matching the input physics (opacity, composition table, convection, boundary condition).
6. Treating fitted parameters ($\alpha_{\rm MLT}$, overshoot, $\eta$ in mass-loss laws) as universal.
7. Trusting remembered numbers: all of the check values in these files should be verified against primary sources.

## 6. Minimal paths

| Goal | Modules | Rough time |
| --- | --- | --- |
| **The engine only** (a working evolution code for the main sequence) | 01 to 09, then 11 | 8 to 9 months |
| **Solar-type stars and the Sun** | 01 to 09, 11, 12, 16 | 10 months |
| **Massive stars and explosions** | 01 to 09, 11, 13, 14, 15 | 11 months |
| **Theory first, code later** (read and solve analytic problems) | 02, 03, 04, 05, 07, 10 to 15 | 6 months |
| **Everything** | 01 to 20 | 14 to 16 months |

## 7. Glossary

- **ZAMS, TAMS:** zero-age and terminal-age main sequence.
- **RGB, HB, AGB, TP-AGB:** red giant branch, horizontal branch, asymptotic giant branch, thermally pulsing AGB.
- **MLT:** mixing-length theory.
- **EEP:** equivalent evolutionary point.
- **NSE:** nuclear statistical equilibrium.
- **TOV:** Tolman-Oppenheimer-Volkoff equation.
- **CAK:** Castor-Abbott-Klein theory of line-driven winds.
- **IMF:** initial mass function.
- **GCE:** galactic chemical evolution.
- **SSP:** simple stellar population.
