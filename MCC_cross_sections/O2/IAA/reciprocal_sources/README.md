# O₂ source constraints for reciprocal thermal rotation

`o2_bhattacharyya.json` contains the physical source values from P. K.
Bhattacharyya and K. K. Goswami, *Elastic and rotational excitation of the
oxygen molecule by intermediate-energy electrons*, Physical Review A **28**,
713 (1983), [DOI](https://doi.org/10.1103/PhysRevA.28.713).
The prepared fixed-temperature model is in
[../reciprocal_hybrid_300K](../reciprocal_hybrid_300K/README.md).

| Field | Meaning |
|---|---|
| `energy_eV` | Incident electron energies, 20–200 eV |
| `integrals_a02` | Columns N=1→1, N=1→3 and rotationally inclusive cross section, in a₀² |
| `momentum_transfer_a02` | Same columns for ∫(1−cosθ)DCS dΩ, in a₀² |

The values are Tables II–III, potential B with a 2a₀ cutoff. They are a
Glauber/eikonal calculation using adiabatic nuclei and polarization, with no
electron exchange. They are not a measured complete O₂ rotational DCS set.
The code's common J index means nuclear rotation N; even N is excluded and
electronic-spin fine structure is unresolved.

For elementary rank integrals aL, angular-momentum recoupling gives

```text
σ11 = a0 + (2/5)a2
σ13 = (3/5)a2 + (4/9)a4
σinclusive − σ11 − σ13 = (5/9)a4 + Σ(L≥6) aL
```

The same equations hold for momentum-transfer integrals. The unresolved
higher-rank residual is distributed using a two-centre spectator prior.
The 1→3 strength and first angular moment then determine rank two. A positive
relative-entropy projection determines angular fractions under the prescribed
IAA elastic angular marginal. This is a completion assumption; the paper does
not uniquely supply those missing angular distributions. The final absolute
inclusive total is IAA's, so the paper's 1→1 value is not an additional
independent absolute constraint on the final model.

Below 1 eV the model instead uses thesis Eq. 11.21b with the actual momentum
transfer of each N→N+2 transition, Q=−0.29 and anisotropic polarizability 4.93
in atomic units. The 1–20 eV positive bridge is modeled, not measured resonance
physics. Reverse channels reuse forward primitives at shifted energies.
The short parent `rotation_1_3.txt` is the elmolcs Born integral source; its
20 eV endpoint must not be interpreted as a constant high-energy rate.

See WarpX's `Docs/source/theory/multiphysics/rotational_scattering/sources.rst`
for the equations, constrained fit and limitations, and
`Tools/CrossSections/reciprocal_rotation/README.rst` for reproducible source
checks. This directory contains source inputs only, not fit residual reports,
benchmark logs, synthetic fixtures or verification records.
