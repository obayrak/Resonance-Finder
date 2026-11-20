# Resonance Finder

*by Orhan Bayrak*

**Bound states, resonances and virtual states of two-body nuclear systems, computed in the browser with five independent methods and compared directly with experimental level schemes.**

Resonance Finder is a single-file web application (`index.html`, no build step, no server) for solving the radial Schrödinger equation of a two-body system — nucleon + core, α + core, light-ion clusters — and locating its poles in the complex energy plane. Calculated levels can be laid next to ENSDF experimental data, and a model potential can be fitted to the measured spectrum automatically.


---

## Features

### Solvers
| Method | Idea | Best for |
|---|---|---|
| **Finite differences + Richardson** | 3-point tridiagonal matrix, each state re-solved at h, h/2, h/4 and extrapolated to O(h⁶) | Robust default, any potential |
| **Sinc-DVR (Colbert–Miller)** | Discrete variable representation with spectral accuracy | Smooth potentials, small grids |
| **Complex scaling + Numerov** | Exterior complex scaling, outward/inward Numerov with secant iteration on the pole | Fast, accurate single poles |
| **Spectral complex scaling** | Chebyshev collocation on a rotated contour; θ-trajectory test separates physical poles from the rotated continuum | Whole spectrum at once |
| **GAMOW 1982** | Re-implementation of the Vertse–Pál–Balogh code: direct integration with Coulomb–Hankel boundary conditions | Narrow resonances, Coulomb barriers |

Each state is reported with E, Γ = −2 Im E, an accuracy estimate and its θ-shift (stability under a change of the rotation angle). The **Stability** tab sweeps θ and grid parameters; the **Wave functions** tab plots the complex radial functions on the real axis and contour (c-normalised).

### Potentials
Woods–Saxon, Gaussian, Yukawa, exponential, sums of Gaussians, inverted oscillator; spin–orbit term (Woods–Saxon / Gaussian / Yukawa derivative); Coulomb as a uniformly charged sphere, Gaussian cloud (erf) or point charge; optional external tabulated potential (spline/linear). Physical units (MeV, fm, masses in u) or dimensionless ħ²/μ = 1.

### Presets (refitted to experiment)
Around 45 ready-made systems, grouped as:
- **Benchmarks** — Gaussian barrier, inverted oscillator (analytic)
- **Deuteron** — Yukawa, Gaussian, exponential, square-well-like
- **Unbound light nuclei** — ⁵He, ⁵Li, ⁷He, ¹³N
- **Halo nucleus ¹¹Be** — 1/2⁺ halo, 1/2⁻ parity inversion, 5/2⁺ resonance
- **p- and sd-shell** — ¹³C, ¹⁷O, ¹⁷F (Thomas–Ehrman shift)
- **α-cluster models** — ⁸Be (Buck–Friedrich–Wheatley, Ali–Bodmer), ⁶Li, ⁷Li, ⁷Be
- **Stellar helium burning** — ¹²C Hoyle state, ¹⁶O subthreshold 1⁻/2⁺/3⁻, ²⁰Ne resonances
- **Shell-model single-particle levels** — ⁴¹Ca, ²⁰⁹Pb, ²⁰⁹Bi

Widths are compared with experiment through a spectroscopic factor S = Γ_exp / Γ_sp.

### Experimental data
![Experimental data tab](docs/screenshots/experimental-data.png)

- **Retrieve** levels and γ transitions for any nuclide from the IAEA LiveChart (ENSDF) endpoint, or **import** ENSDF / CSV / JSON files or pasted text (a CSV template is provided).
- Harmonizer for loaded levels, threshold & channel analyser (n, p, d, t, ³He, α, nα …) using the **AME2020** mass table (3558 nuclides, embedded).
- **Two-body setup from a nucleus**: type a nuclide (default ¹²C) and the app proposes partitions (e.g. α + ⁸Be → ¹²C, S = 7.367 MeV) with the Pauli-forbidden node count N₀ from oscillator-quanta counting.
- **Tune V₀ to the ground state** (bisection), then all bound and quasi-bound states for ℓ = 0…ℓ_max are computed and inserted into the *Experimental and theoretical level table* and the spectrum.
- **Automatic model selection and fit**: Woods–Saxon, WS², Gaussian and an ℓ-dependent depth
  V₀(ℓ) = V₀ [1 + a_L ℓ(ℓ+1)] [1 + k_π (odd ℓ)]
  are each fitted (staged Levenberg–Marquardt: V₀ → a_L, k_π → r₀ → a) to the matched levels; the best model is chosen by AIC/BIC. Matching pairs states with the same J^π, nearest in energy within a tolerance.

### Experimental spectrum
Level-scheme plot with experimental levels (left of each J column) and calculated levels (right, green), match lines, thresholds, S_n/S_p lines, γ-ray arrows with B(XL), uncertainty bars, filters, mini-map, and Inspect / Measure (ΔE, Γ ratio) / Pin tools. Exports to PNG and CSV.

### Theory tab
Derivations and formulas behind each method, with references.

### Exports
Every plot exports to CSV, DAT (commented header with all parameters) and PNG; tables export to CSV/JSON.

---

## Quick start

**Online:** open the GitHub Pages site → `https://<your-user>.github.io/resonance-finder/`

**Locally:** download `index.html` and open it in any modern browser (Chrome, Firefox, Edge, Safari). Nothing to install.

Typical session:
1. Pick a **Preset** in the sidebar (e.g. *¹²C Hoyle state*) and click **Find states**.
2. Inspect poles on the **Results** tab; check them on **Stability**.
3. Go to **Experimental data**, enter a nuclide, click **Retrieve levels + γ** (or import a file).
4. Click **Set up two-body system**, then **Tune V₀ to the ground state** or **Auto-fit model**.
5. Compare on **Experimental spectrum**.

### Requirements
- An internet connection for [Plotly.js 2.35.2](https://plotly.com/javascript/) (loaded from jsDelivr) and Google Fonts. Everything else, including the AME2020 table, is inside `index.html`.
- Live ENSDF retrieval needs network access to `nds.iaea.org`. If the browser blocks the request (CORS), the app can try public CORS proxies (checkbox), or you can download the data from [IAEA LiveChart](https://www-nds.iaea.org/relnsd/vcharthtml/VChartHTML.html) / [NNDC NuDat](https://www.nndc.bnl.gov/nudat3/) and import it.

---

## Deploying on GitHub Pages

1. Create a repository (e.g. `resonance-finder`) and push these files.
2. *Settings → Pages → Build and deployment → Source: GitHub Actions* (the included workflow `.github/workflows/pages.yml` publishes the repository root), or choose *Deploy from a branch → main / (root)*.
3. The app will be served at `https://<your-user>.github.io/resonance-finder/`.

```bash
git init
git add .
git commit -m "Initial release of Resonance Finder"
git branch -M main
git remote add origin https://github.com/<your-user>/resonance-finder.git
git push -u origin main
```

---

## Repository layout

```
resonance-finder/
├── index.html              # the complete application
├── README.md
├── LICENSE
├── CITATION.cff
├── CHANGELOG.md
├── CONTRIBUTING.md
├── examples/
│   └── 12C_levels_example.csv   # sample import file (approximate values; check against ENSDF)
├── docs/screenshots/       # images used in this README
├── .github/workflows/pages.yml
├── .nojekyll
└── .gitignore
```

---

## Known limitations

- **Auto-fit is slow** (tens of seconds to a few minutes for ¹²C with all four models), because each trial potential re-runs a full spectral search. A pole-tracking speed-up is planned (see `CHANGELOG.md`).
- Single-channel, local potentials only; no coupled channels, non-local or deformed potentials.
- A single cluster potential cannot reproduce the ¹²C spectrum without ℓ-dependence; fitted parameters are phenomenological.
- Pauli-forbidden states are removed by node counting (N₀), not by orthogonality-condition projection.
- ENSDF retrieval depends on the IAEA service and on browser CORS policy.

---

## Data sources and credits

- **Masses:** AME2020 — W. J. Huang *et al.*, Chin. Phys. C 45, 030002 (2021); M. Wang *et al.*, Chin. Phys. C 45, 030003 (2021). Table taken from the `mendeleev` Python package.
- **Levels and γ rays:** ENSDF, via the IAEA Nuclear Data Section LiveChart API; see also NNDC NuDat 3.
- **Plotting:** Plotly.js (MIT licence, GPL-compatible).

## Key references

- J. Aguilar, J. M. Combes, Commun. Math. Phys. 22, 269 (1971); E. Balslev, J. M. Combes, *ibid.* 22, 280 (1971) — complex scaling.
- B. Simon, Phys. Lett. A 71, 211 (1979) — exterior complex scaling.
- N. Moiseyev, Phys. Rep. 302, 212 (1998); *Non-Hermitian Quantum Mechanics*, CUP (2011).
- T. Vertse, K. F. Pál, Z. Balogh, Comput. Phys. Commun. 27, 309 (1982) — GAMOW.
- D. T. Colbert, W. H. Miller, J. Chem. Phys. 96, 1982 (1992) — sinc-DVR.
- B. Buck, H. Friedrich, C. Wheatley, Nucl. Phys. A 275, 246 (1977) — α–α potential.
- S. Ali, A. R. Bodmer, Nucl. Phys. 80, 99 (1966) — α–α potential.

Please check the references against the original sources before citing them in publications.

---

## Citing

If you use Resonance Finder in your work, please cite it using the metadata in [`CITATION.cff`](CITATION.cff) (GitHub shows a *Cite this repository* button).

## Licence

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)

Copyright © 2026 Orhan Bayrak.

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version. It is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the [GNU General Public License v3](LICENSE) for details.
