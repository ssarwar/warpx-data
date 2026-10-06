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

Electron–N₂ cross sections from the supplied **elmolcs IAA** data, described in
A. Schmalzried, *Electron Thermal Runaway in Atmospheric Electrified Gases:
a microscopic approach* (2023), chapters 11–12.

| File | Meaning | Energy range |
|---|---|---|
| `elastic.txt` | Residual vibrationally elastic integral, including rotation | 0–1 GeV |
| `elastic_dcs.txt` | Vibrationally elastic angular distribution | 0.001 eV–1 GeV |
| `rotation_0_0.txt` | Rotationally unchanged J=0→0 calculation | 0–1000 eV |
| `rotation_0_2.txt` | Elementary J=0→2 excitation | threshold–1000 eV |
| `rotation_0_4.txt` | Elementary J=0→4 excitation | threshold–1000 eV |
| `rotation_0_6.txt` | Elementary J=0→6 excitation | threshold–1000 eV |
| `thermal_spectator.rot` | Discrete IAA spectator differential weights, spectator format | 0–1000 eV |

Integral files have two columns: energy in eV and cross section in m².
They preserve positive source knots and adapt log–log interpolation to WarpX's
linear reader (target relative error 0.05%, with a 10⁻⁶ peak floor near zero).
The residual table ends at 6 keV; its continuation uses elmolcs's fitted
`Elastic.gen_Born` model, matched continuously at that endpoint.

The DCS file has one energy followed by 361 values in m²/sr at angles
0°, 0.5°, …, 180°. Original source rows are retained. WarpX's IAA sampler uses
linear interpolation in sin(θ/2) and log(E), with screened Rutherford sampling
above 10 keV. Its angular normalization is separate from `elastic.txt`.

Rotational sources are Itikawa (2006), Table 5, below 1.25 eV for J=0→2,
and [Kutz and Meyer (1995), Fig. 7a](https://doi.org/10.1103/PhysRevA.51.3819)
for the remaining entries. These are the elementary `iaa*` tables, not the
generated `iaa` tables with the displaced resonance. Thresholds use
B=1.998 cm⁻¹ and Δ=B[J′(J′+1)−J(J+1)]; this corrects the 0→6 header.
Interpolation uses the reduced cross section σ p_in/p_out, held at its first
positive value between threshold and the first datum. Below threshold σ=0.
No high-energy rotational extrapolation is supplied.

**These source sets cannot be combined unchanged.** At 2.3 eV the listed
rotational excitations sum to 2.40×10⁻¹⁹ m², while the residual integral is
1.73×10⁻¹⁹ m². `rotation_0_0.txt` is a separate theoretical J=0→0 result,
not the residual minus rotation and not a thermal average. Do not add the
rotational rates to `elastic.txt` or use a negative unchanged remainder.
The two-column source files retain those values. The conditional model below
uses them as relative differential weights and therefore defines different
effective integral rotational rates.

`thermal_spectator.rot` is the kinetic counterpart of the IAA mean-loss
prescription (thesis Eq. 2.48). Use it on the same elastic process as
`elastic.txt` and `elastic_dcs.txt`, with `rotation_model = iaa_spectator`.
After the elastic angle is drawn, normalized sudden/spectator weights select
unchanged, excitation or de-excitation outcomes. Angular averaging under the
elastic DCS determines their effective integral rates. No additional rotational
processes should be added. Discrete energy changes retain the transfer variance
and angle–energy correlation rather than applying a mean loss.

The little-endian spectator bundle stores energy and angular grids, four tabulated
angular basis functions, and finite-threshold transition coefficients. It is
generated offline from the elementary elmolcs data by WarpX's
`Tools/CrossSections/thermiaa_spectator.py`; WarpX evaluates no Bessel functions
and generates no physical data files during a simulation. Coefficients have
a common scale removed at each energy because it cancels in the conditional
probabilities. They are relative differential weights, not cross sections in m².
The ordinary elastic file continues to supply the absolute event rate.
Energy-row probabilities use square-root mixing immediately above zero and
the first excitation threshold, and linear mixing on the other intervals.
Energy knots and canonical level spacings remain in double precision.

The file resolves initial populations through J=96 for temperatures through
1000 K, uses N₂ even:odd nuclear-spin weights 6:3, and includes final states
through J=102. Runtime initialization prepares the chosen fixed Boltzmann
distribution; it does not evolve neutral states. The angular functions
use the Kutz–Meyer internuclear separation R=2.068 Bohr radii. The included ranks
are L=0, 2, 4 and 6. This truncation does not establish convergence of higher-rank
rainbows at high energies. No continuation above 1000 eV is supplied.

The spectator approximation is known to be inaccurate in the thermal and
resonance regimes. Its use there is an explicit part of this IAA-style model.
Normalizing its transition probabilities does not enforce detailed balance
or guarantee Maxwellian electron equilibrium. These are physical model
limitations, distinct from interpolation or sampling error.

## Imported elastic DCS provenance

The following descriptions are the supplied elmolcs DCS source header. These
are the source's fitting/potential labels, not WarpX input options. WarpX
imports the resulting angular grid. The reciprocal hybrid subsequently uses
its documented smooth 8–10 keV transition to screened Rutherford.

```text
COMMENT: Least-squares Fits to DCS Database (below 30 eV)
COMMENT: Below 0.1 eV : MERT estimate with A = 0.44
COMMENT: Between 4 and 8 eV, assistance from Sun et al. (1995) table VII. predicted scattering at 180° (only 1 point added with a 15% uncertainty (~ 2x higher than exp data))
COMMENT: Above 30 eV to 150 eV : V=seopa with angular-momentum coupling, Vex(fermi=True) and Vpol(pola2=0.5,exp=True)
COMMENT: 200 (s=0.85) to 850(s=0.86),900(s=0.87),950(s=0.885), 1000 (s=0.9) eV: IAM with V=seca, Vex(fermi=False), Vcop(cross=outer)
COMMENT: Above 1000 eV : Removal of correlation potential and use of Buckingham polarisation (pola2=0.5),
COMMENT: Above 8000 eV: Removal of absorption potential and use of Born approximation only
UPDATED: 22/09/2022
```

The thesis discusses the physical cross sections in Chapter 11/Section 12.1,
the fits in Chapter 13, and database construction/comparison in Chapters 15–16.
Printed p. 567 identifies limitations in the sub-eV elastic data: the residual
is inferred from total scattering, the modified effective-range angular model
is basic, and only 1, 10 and 100 meV source knots describe the lowest energies.
Dense exported grids improve numerical interpolation, not the physical evidence.
