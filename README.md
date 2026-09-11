# LH2PoolX

LH2PoolX calculates an **evidence-qualified, quasi-steady liquid-hydrogen
pool source term** for a declared time window.  It is an upstream input module
for dispersion models, not a dispersion model and not a general jet-impact or
pool-spreading solver.

## What it calculates

For a specified release rate, storage pressure, elapsed time, substrate and
ground-deposition fraction, LH2PoolX calculates:

1. isenthalpic flash fraction using CoolProp;
2. liquid mass reaching the ground;
3. heat-limited evaporation flux using a semi-infinite ground-conduction
   relation with an LH2 critical-heat-flux cap; and
4. the unconfined equilibrium pool area and evaporation rate.

`deposition_fraction` is deliberately explicit.  The package does **not**
predict impingement, splashing, droplet transport, drainage, barriers, or
transient pool spreading.  A confined result reports liquid accumulation and
must not be used as a steady source without a separate inventory model.

## Observed-footprint route

Some records constrain a ground footprint more reliably than they constrain
the liquid fraction transferred from the release to the ground. In this case,
evaluate_observed_footprint_source converts a declared circular-equivalent
radius into a heat-limited evaporation source on the declared solid substrate:

    from lh2poolx import evaluate_observed_footprint_source

    lower = evaluate_observed_footprint_source(
        equivalent_radius_m=0.5, elapsed_s=300.0
    )
    upper = evaluate_observed_footprint_source(
        equivalent_radius_m=1.0, elapsed_s=300.0
    )
    print(lower.area_m2, lower.evaporation_rate_kg_s)
    print(upper.area_m2, upper.evaporation_rate_kg_s)

This is a conditional source route, not an inverse impact model. It does not
accept a release rate or deposition_fraction, and does not reconstruct pool
growth, inventory, liquid deposition, splash/droplet transport, drainage or
the footprint history. If a record gives an extent range, calculate both
bounds and retain the resulting source range; do not select a midpoint or fit
an unobserved deposition fraction.

For example, the FFI large-scale LH2 report describes vertical-release pools
as remaining within roughly 0.5-1.0 m of the release point, while also noting
that fog prevented a precise visual verification. That record can support a
0.5-1.0 m conditional radial-extent envelope, not a single observed pool
area, evaporation history or deposition fraction. See the
[FFI report](https://www.ffi.no/publikasjoner/arkiv/large-scale-leakage-of-liquid-hydrogen-lh2-tests-related-to-bunkering-and-maritime-use-of-liquid-hydrogen/21-03101.pdf).

## Install

```bash
pip install lh2poolx
```

## Minimal use

```python
from lh2poolx import LH2Release, evaluate_pool_source

release = LH2Release(rate_kg_s=0.1055, storage_pressure_barg=1.0)
source = evaluate_pool_source(release, elapsed_s=300.0)
print(source.area_m2, source.evaporation_rate_kg_s)
```

## Evidence boundary

The concrete heat-transfer implementation is checked against the four
published PRESLHY E3.4 Concrete02 mass-loss windows.  That comparison is a
restricted substrate-and-condition check, not validation for other substrates
or for jet-to-ground deposition.  Literature supports the physical form of
the source chain; it does not turn unknown deposition into a fitted value.

## License

MIT.  Cite the accompanying software record when one is released, plus the
underlying experimental and heat-transfer sources used in an analysis.
