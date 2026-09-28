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
The data here retain the source values; they do not impose a normalization
to conceal that discrepancy.
