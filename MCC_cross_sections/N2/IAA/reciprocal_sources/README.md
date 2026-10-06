# N₂ source constraints for reciprocal thermal rotation

These are numerical physical inputs, not runtime sampling caches or validation
records. The prepared 300 K model is in
[../reciprocal_hybrid_300K](../reciprocal_hybrid_300K/README.md).

| File | Meaning and ordering |
|---|---|
| `n2_gote.json` | Published Table 1 transfer-rank percentages: energy keys in eV; angular columns 10°, 20°, …, 160°; rows give the reported elementary ranks (0,2,…,8 or 10); the final row is the rotationally summed DCS in 10⁻²⁰ m²/sr |
| `n2_jung_digitization.json` | Approximate manual Fig. 5 readings for unchanged, +2 and +4 branches at seven angles; units, temperature and reading uncertainty are explicit |
| `n2_jung.json` | Nonnegative elementary rank-0, rank-2 and rank-4 fractions inferred from those thermal branches, at 2.22 and 2.47 eV; columns 15°, 30°, …, 105° |

Gote and Ehrhardt, *Rotational excitation of diatomic molecules at intermediate
energies: absolute differential state-to-state transition cross sections for
electron scattering from N₂, Cl₂, CO and HCl*, J. Phys. B **28**, 3957–3986
(1995), covers 10–200 eV. A value −1 denotes a reported contribution below 1%,
not a negative cross section. The nominal completion assigns 0.5%. A column
is retained only when its possible censored sum brackets 100% within 0.3
percentage points. Inconsistent columns are replaced by interpolation from
consistent neighboring angles, while the published values remain here.
The final absolute DCS row is retained for source comparison, but IAA supplies
the production total and angular marginal. Reported ranks are not a complete
bound-rotor spectrum at higher energies.

Jung, Antoni, Muller, Kochem and Ehrhardt, *Rotational excitation of N₂, CO and
H₂O by low-energy electron collisions*, J. Phys. B **15**, 3535–3555 (1982),
provides Fig. 5 and Table 1 at 500 ± 30 K. The figure readings have at least
0.02×10⁻²⁰ m²/sr manual uncertainty in addition to experimental uncertainty.
They are thermal branches, not independent state-to-state measurements.
Reverse-branch ratios were constrained in the experimental line fit.

`prepare_jung.py` in WarpX reproduces the inferred fractions. It sums thermal
Clebsch–Gordan weights for +2/+4 branches, includes their nominal threshold
factor, solves a nonnegative least-squares problem for rank-2/rank-4 strengths,
and assigns the remaining inclusive strength to rank zero. It uses the
paper's fitted reverse/upward branch ratios only to reconstruct the inclusive
source marginal. Detailed balance in the final model is constructed separately
using shifted-energy forward primitives.

The resonance completion uses the Read–Andrick angular tensors outside the
measured angular interval. Two adopted missing-angle factors, 0.52823198 and
0.84013224 for ranks 2 and 4, were calibrated against the 2.47 eV integral
branch sums; removed intensity goes to rank zero. They are model-fit values,
not constants published by Read and Andrick. The full reciprocal construction
checks the final branches against Jung rather than treating the digitization
as exact. Kutz–Meyer supplies the elementary energy dependence from the parent
`rotation_0_*.txt` files. The separate unchanged 0→0 curve is not the IAA
residual. Boltzmann averaging alone does not reconcile their normalizations.

The derivation, interpolation, missing-angle assumptions, source limitations
and source-check commands are maintained in WarpX under
`Docs/source/theory/multiphysics/rotational_scattering/sources.rst` and
`Tools/CrossSections/reciprocal_rotation/README.rst`.
