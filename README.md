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
