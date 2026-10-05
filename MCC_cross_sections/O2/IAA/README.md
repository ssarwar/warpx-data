## Reciprocal thermal model

`reciprocal_hybrid_300K/thermal_rotation.rot` is the prepared, combined elastic
and rotational family for the reciprocal N2/O2 construction. It covers 0–1 GeV
at a fixed 300 K rotational temperature; see its README for configuration and
continuations. The short elementary rotational curves below remain source
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
