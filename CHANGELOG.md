# Changelog

## 0.1.1 - 2026-09-11

- Added evaluate_observed_footprint_source, which converts a declared
  circular-equivalent LH2 pool footprint into a heat-limited, conditional
  evaporation source term.
- The new route intentionally has no release rate or deposition_fraction
  input. It does not infer jet impact, rainout, deposition, inventory or a
  time-resolved pool trajectory.
- When a published experiment reports a footprint range, users should run
  each reported bound; the package does not select a midpoint or fit a source
  fraction.

## 0.1.0 — 2026-09-09

- First public release.
- Isenthalpic LH2 flash with CoolProp.
- Explicit deposition fraction and quasi-steady ground-conduction source term.
- Confined-pool accumulation status to prevent use as an unqualified steady source.
- Optional adapters supplied by SLABx and DEGADISx; no dispersion model is a dependency.
