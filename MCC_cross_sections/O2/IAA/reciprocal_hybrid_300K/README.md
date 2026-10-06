# Reciprocal electron–O2 rotational scattering at 300 K

`thermal_rotation.rot` and its binary parts are one prepared V6 collision
family for `rotation_model = reciprocal_hybrid`. The supported electron-energy
range is 0–1 GeV. The bundle contains the inclusive vibrationally elastic rate,
its angular distribution, and discrete unchanged/excitation/de-excitation
outcomes. Do not add the elementary rotational files as extra MCC processes.

Use the parent `elastic.txt` as the cross-section consistency input,
`scattering_angle_model = IAA`, and this index as `rotation_file`. The index
already contains the elastic angular sampling data. Set rotational temperature
to 300 K; the translational temperature can be specified independently.

Rates K are in m³/s, energies and signed electron losses in eV, and neutral
mass in kg. Positive loss denotes rotational excitation. The finite zero-energy
superelastic rate is included. Below 1 meV the unchanged background uses the
explicit cold continuation; above that join the inclusive rate follows the IAA
residual and its matched high-energy Born continuation. The angular model joins
screened Rutherford smoothly from 8 to 10 keV. Queries beyond the advertised
range are errors, not constant-rate or zero extrapolations.

All state sums, reciprocal normalization, CDF inversion, and alias construction
were performed offline. The format and source qualifications are documented in
WarpX `Tools/CrossSections/reciprocal_rotation`. The continuous reference uses
paired forward/reverse kernels; finite sampling tables approximate it within
the documented numerical budgets. Numerical precision does not establish the
physical accuracy of the low-energy O2 resonance or high-J continuation.

The accompanying `.bin` parts contain only scientific sampling arrays. These
production files use aliases; cumulative comparison tables and validation
records are kept with the tests, outside warpx-data.

The index also supplies compact quantile search bounds. They shorten exact
searches of the stored CDF grids and do not change any cross section,
probability, angle, or discrete energy change.
