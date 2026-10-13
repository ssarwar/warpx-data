## Reciprocal thermal model

`reciprocal_hybrid_300K/thermal_rotation.rot` is the readable combined elastic
and rotational family for the reciprocal N2/O2 construction. It covers 0–1 GeV
at a fixed 300 K rotational temperature; see [its README](reciprocal_hybrid_300K/README.md) for configuration, units,
file layout, initialization and continuations. The full derivation is in the
[WarpX multiphysics theory manual](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering.rst). The gas-specific [rotational DCS construction](reciprocal_hybrid_300K/rotational_dcs.md)
gives formulas, energy ranges and interpolation rules. The short elementary
rotational curves below remain source
inputs, and must not be added to the inclusive family as separate processes.

New ordinary tables declare `energy_min_eV`, `energy_max_eV`, and
`outside_energy_range = error`. The matching WarpX reader enforces these bounds
before either MCC selector path. Their last values do not authorize an
unlimited constant extrapolation.

Electron–O₂ cross sections from the supplied **elmolcs IAA** data, described in
A. Schmalzried, *Electron Thermal Runaway in Atmospheric Electrified Gases:
a microscopic approach* (2023), chapters 11–12.

| File | Meaning | Energy range |
|---|---|---|
| `elastic.txt` | Residual vibrationally elastic integral, including rotation | 0–1 GeV |
| `elastic_dcs.txt` | Vibrationally elastic angular distribution | 0.001 eV–1 GeV |
| `rotation_1_3.txt` | Ground-rotor J=1→3 excitation | threshold–20 eV |

Integral files contain energy in eV and cross section in m². Positive source
knots are preserved, with adaptive points for WarpX's linear interpolation
(target relative error 0.05%, with a 10⁻⁶ peak floor near zero). Above the last
residual datum at 6 keV, the rate uses elmolcs's fitted `Elastic.gen_Born`
model matched continuously at the join.

The DCS file has one energy followed by 361 values in m²/sr at angles
0°, 0.5°, …, 180°. Original source rows are retained. WarpX's IAA sampler uses
linear interpolation in sin(θ/2) and log(E), and screened Rutherford sampling
above 10 keV. Its normalization is separate from the residual integral.

The rotational table is elmolcs's **numerical integral Born approximation**,
attributed to [Takayanagi and Itikawa (1970), Eq. 38](https://doi.org/10.1016/S0065-2199(08)60204-3),
not a measured O₂ cross section. The canonical threshold uses B=1.438 cm⁻¹;
even rotational states are forbidden. Interpolation uses σ p_in/p_out, held
at its first positive value between threshold and the first datum. σ=0 below
threshold. No rotational extrapolation above 20 eV is supplied, and the table's
energy extent does not establish the Born model's physical accuracy throughout it.

The elastic integral already includes unresolved rotation. Do not add this
excitation as an extra channel without a consistent unchanged component and
thermal de-excitation rates.

## Imported elastic DCS provenance

The following descriptions are the supplied elmolcs DCS source header. These
are the source's fitting/potential labels, not WarpX input options. WarpX
imports the resulting angular grid. The reciprocal hybrid subsequently uses
its documented smooth 8–10 keV transition to screened Rutherford.

```text
COMMENT: Least-squares Fits to DCS Database : combined use of MPSA (when it works) and LS-Legendre fit
COMMENT: Below 1 eV : MERT from A = 0.3 polarisability and permanent quadrupole
COMMENT: Below 10 eV : Sullivan et al 1995, Green et al 1997, Linert et al 2004 exclusively
COMMENT: From 15 eV to : + Wöste et al 1995, Trajmar et al 1971 and Shyn&Sharp 1982
COMMENT: From 30 eV : Calculations angular-coupling V=seopa (Eth=4.262 eV)
COMMENT: from 200 eV (s=0.85) 500 eV (s=0.92) : IAM V=seca (Vb, cross=outer)
COMMENT: from 1000 eV (s=0.95) : IAM V=seca (Vb, cross = outer)
COMMENT: from 4000 eV (s=1) : IAM V=sepa Vb
COMMENT: from 8000 eV : V=sepa Born only
UPDATED: 12/12/2022
```

The thesis discusses the physical cross sections in Chapter 11/Section 12.1,
the fits in Chapter 13, and database construction/comparison in Chapters 15–16.
Printed p. 567 identifies limitations in the sub-eV elastic data: the residual
is inferred from total scattering, the modified effective-range angular model
is basic, and only 1, 10 and 100 meV source knots describe the lowest energies.
Dense exported grids improve numerical interpolation, not the physical evidence.
