# Module 14: Nucleosynthesis

**Difficulty:** Medium to hard. **Time:** about 3 weeks.

## Goal

Understand where the chemical elements come from: Big Bang nucleosynthesis, hydrostatic burning, the slow and rapid neutron-capture processes, explosive nucleosynthesis in supernovae, and the chemical enrichment of galaxies. Build post-processing tools that compute abundances along stellar evolution tracks and a simple galactic chemical evolution model.

## Prerequisites

Modules 07, 11, 12 and 13.

## Reading

- **Cla, R&R, Ili:** the burning stages, the $s$-, $r$- and $p$-processes, explosive burning, nuclear statistical equilibrium, abundance curve.
- **Arn:** explosive nucleosynthesis in supernovae.
- **Pagel:** chemical evolution of galaxies, yields, the closed-box model, abundance patterns.
- **KWW, HKT, Iben:** the nuclear processes inside stars and the yields of stellar evolution.

## Key concepts and equations

**Solar abundances.** The abundance curve ($\log\epsilon$ against mass number $A$) shows the high abundance of H and He (primordial), the dip of Li, Be, B (destroyed in stars), the iron peak at $A\approx56$, double peaks from neutron-capture processes at the magic neutron numbers $N=50$, 82, 126: *$s$-process peaks* at $A\approx88$, $138$ and $208$ and *$r$-process peaks* at $A\approx80$, $130$ and $195$. Mass fractions of the Sun: $X\approx0.74$, $Y\approx0.25$, $Z\approx0.014$ (depends on the composition table; verify).

**Big Bang nucleosynthesis.** Primordial helium mass fraction $Y_p\approx0.247$; D/H $\approx2.5\times10^{-5}$; the lithium problem (predicted $^7$Li exceeds the observed abundance; verify).

**Hydrostatic burning cycles.** pp, CNO (equilibrium with $^{14}$N dominating and $^{12}$C/$^{13}$C$\approx3$ to 4), NeNa and MgAl cycles (relevant to the abundance anomalies of globular cluster stars), He burning ($3\alpha$, $^{12}$C$(\alpha,\gamma)^{16}$O, $^{14}$N$\to^{18}$O$\to^{22}$Ne), carbon, neon, oxygen and silicon burning.

**The $s$-process.** Slow neutron capture (neutron density $10^7$ to $10^{12}$ cm$^{-3}$) along the valley of stability. Neutron sources: $^{13}$C$(\alpha,n)^{16}$O (about $10^8$ K, in the intershell of low-mass AGB stars) and $^{22}$Ne$(\alpha,n)^{25}$Mg (about $3\times10^8$ K, in massive stars and in thermal pulses of massive AGB stars). The neutron exposure is $\tau_n=\int n_nv_T\,dt$. In the steady-flow regime $\langle\sigma\rangle N_s\approx{\rm const}$ between magic numbers (so the abundance is inversely proportional to the capture cross-section). The "main" $s$-process occurs in AGB stars and the "weak" $s$-process in massive stars for $A\lesssim90$.

**The $r$-process.** Rapid neutron capture (neutron density above $10^{20}$ cm$^{-3}$) in neutron-rich environments (neutron star mergers, certain supernova ejecta), with the path set by the $(n,\gamma)\leftrightarrow(\gamma,n)$ equilibrium (the *waiting-point approximation*) and followed by $\beta$-decays back to the stability valley.

**The $p$-process** ($\gamma$-process): photodisintegration of heavy seeds in supernova shells; the **$i$-process** (intermediate neutron density) and the **rp-process** (rapid proton capture in X-ray bursts) are other regimes.

**Explosive burning.** Shocked layers burn on timescales shorter than the hydrostatic ones, at $T_9\gtrsim4$ reaching NSE with alpha-rich freeze-out. The yield of $^{56}$Ni (which decays to $^{56}$Fe through $^{56}$Co) is about $0.07\,M_\odot$ in SN 1987A and about $0.6\,M_\odot$ in a typical Type Ia supernova. Core-collapse supernovae produce most of the $\alpha$-elements (O, Mg, Si, Ca) and Type Ia supernovae most of the iron-group nuclei.

**Chemical evolution.** In the closed-box model with instantaneous recycling the metallicity grows as $Z=y\ln(1/\mu_g)$ with $\mu_g$ the gas fraction and $y$ the yield. The $[\alpha/{\rm Fe}]$ ratio shows a plateau of about $+0.3$ to $+0.4$ at low metallicity (core-collapse supernovae) and a decline once Type Ia supernovae (with delay times of order $10^8$ to $10^{10}$ yr) contribute iron.

## Hands-on problems

**1. [E] Solar abundances.** Parse a published solar composition table (Asplund et al. 2009 or the Lodders compilations; verify). Compute $X$, $Y$, $Z$, plot $\log\epsilon$ versus $A$ and label the iron peak, the $s$- and $r$-process peaks and the light-element dip. Compare two compositions (older versus newer) and discuss the difference in $Z$.

**2. [E] CNO equilibrium.** Using your REACLIB network, run the CNO cycle at $T=2\times10^7$ K until it reaches equilibrium. Compute the equilibrium ratios $^{12}$C/$^{13}$C and $^{14}$N/$^{12}$C and compare with the textbook values ($^{12}$C/$^{13}$C$\approx3.5$). Repeat at several temperatures and compute the timescale to equilibrium.

**3. [M] NeNa and MgAl cycles.** Extend the network to include the NeNa and MgAl chains and run it at $T=(5$ to $10)\times10^7$ K. Reproduce the O-Na and Mg-Al anticorrelations qualitatively, which are the hallmark of multiple stellar populations in globular clusters.

**4. [M] The $s$-process.** Build a network along the valley of stability for Fe to Bi with neutron capture cross sections (the public KADoNiS database; verify) and $\beta$-decay rates. Calculate the abundance pattern for a given neutron exposure $\tau_n$. Show the steady-flow plateaus and the peaks at $A=88$, $138$, $208$. Compare with the "$s$-only" isotopes of the solar system (those shielded from $r$-process contributions).

**5. [M] The $r$-process.** Implement the waiting-point approximation: for each isotopic chain find the equilibrium abundance distribution for given $n_n$ and $T$, then follow the $\beta$-flow along the waiting points. Use published nuclear masses (for example a mass-table fit) and $\beta$-decay half-lives. Produce the $r$-process peaks at $A\approx80$, $130$, $195$ and show how they depend on $n_n$ and on the duration. Then run a full dynamic network on a parameterized neutron-rich expansion trajectory (initial $Y_e$, entropy and expansion timescale) and compare.

**6. [M] NSE and freeze-out.** Using your NSE solver (Module 07), follow a cooling trajectory $T_9(t)=T_{9,0}\exp(-t/\tau)$, $\rho\propto T^3$ from $T_9=8$ to $2$ with $Y_e=0.5,0.49,0.47$. Compute the final yields ($^{56}$Ni, $^{57,58}$Ni, $^{44}$Ti, $^4$He) for different expansion timescales and discuss the alpha-rich freeze-out.

**7. [M] Post-processing a stellar model.** Take your massive-star model of Module 13 (the final structure and its history, or a snapshot of temperature and density per zone) and run a large network zone by zone (post-processing) through the shock passage with a parametrized shock trajectory ($T_{\rm peak}$ determined by the explosion energy and the radius). Compute the nucleosynthetic yields of each isotope, and plot the abundance pattern relative to solar ("production factors") to see which elements come from massive stars.

**8. [M] A one-zone chemical evolution model.** Write a one-zone model with infall, a star formation law, the IMF (Module 10), stellar lifetimes from your tracks (Module 11), core-collapse yields (from problem 7 or published yields) and a Type Ia delay-time distribution ($t^{-1}$). Reproduce the $[\alpha/{\rm Fe}]$ plateau and the knee near $[{\rm Fe/H}]\approx-1$. Vary the star formation efficiency and the Type Ia delay time and see how the knee moves.

**9. [H] Large networks and performance.** Run a network of 500 to 2000 isotopes (the public REACLIB set) with a sparse Jacobian and an implicit solver, for the explosive nucleosynthesis trajectories of problem 6. Benchmark different linear algebra approaches (sparse LU, iterative solvers), and consider GPU or batched approaches to post-process hundreds of zones.

## Software component

- `starlite/net/`: large networks, sparse solvers, $s$-process and $r$-process specialized networks.
- `tools/postprocess.py`: zone-by-zone network post-processing along trajectories.
- `tools/yields.py` and `tools/gce.py`: yield tables and the one-zone chemical evolution model.

## Checks

- CNO equilibrium: $^{12}$C/$^{13}$C$\approx3.5$ and $^{14}$N dominant.
- $s$-process peaks at $A=88,138,208$ and flat $\sigma N$ plateaus; $r$-process peaks at $A\approx80,130,195$.
- $^{56}$Ni dominance at $Y_e=0.5$ and neutron-rich Fe-group nuclei at $Y_e<0.49$.
- $[\alpha/{\rm Fe}]\approx+0.3$ to $+0.4$ plateau with a knee in the one-zone model.

## Pitfalls

- Applying a small network and expecting accurate yields of rare isotopes.
- Using rate libraries without checking temperature ranges, reverse rates or partition functions.
- Ignoring that yields depend sensitively on the explosion model, mixing and rates.

## Gate questions

1. Why do neutron-capture processes produce abundance peaks at specific mass numbers?
2. Why do the $s$- and $r$-process peaks sit at different mass numbers?
3. Why does $[\alpha/{\rm Fe}]$ decrease at higher metallicity?

## Deliverable

`starlite/net/` extensions, `tools/postprocess.py`, `tools/gce.py`, `notebooks/14_nucleosynthesis.ipynb`, solutions for problems 1 to 9.
