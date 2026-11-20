# Changelog

All notable changes to this project are documented here. The format follows [Keep a Changelog](https://keepachangelog.com/) and the project uses [Semantic Versioning](https://semver.org/).

## [Unreleased]
### Planned
- Fast auto-fit by pole tracking: identify states once with the spectral solver, then follow each pole with the Numerov ECS secant solver while parameters change (prototype ran ~20× faster on ¹²C).

## [1.0.0] — 2026-11-20
### Added
- Five solvers: finite differences + Richardson, sinc-DVR, complex scaling + Numerov, spectral complex scaling, GAMOW 1982.
- Results, Stability, Wave functions and Theory tabs; CSV/DAT/PNG export.
- About 45 presets refitted to experimental levels, with spectroscopic factors S = Γ_exp/Γ_sp.
- Experimental data tab: IAEA LiveChart (ENSDF) retrieval, ENSDF/CSV/JSON import, harmonizer, threshold analyser, embedded AME2020 mass table.
- Two-body setup from a nuclide (default ¹²C → α + ⁸Be) with Pauli node count N₀.
- Tune V₀ to the ground state; automatic computation of bound and quasi-bound states into the *Experimental and theoretical level table* and spectrum.
- Automatic model selection and fit (Woods–Saxon, WS², Gaussian, ℓ-dependent depth) with AIC/BIC.
- Experimental spectrum tab with γ arrows, B(XL), match lines, Inspect / Measure / Pin tools and mini-map.
- Day/night theme.
