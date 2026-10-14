# N2 vibrational angular distributions

Pass a `.angular` descriptor as `differential_cross_section`. Each descriptor
references its normalized `_pdf.txt` profile and the shared elastic fallback.
The profile contains incident energy followed by values in sr⁻¹ at 0°, 1°, …,
180°. It supplies only angular probability; the integral collision rate and
energy loss come from the parent directory and gas manifest.

[sources.json](sources.json) records measured rows, canonical mappings, endpoint
completion, transitions and source hashes.

| Descriptor | Profile |
|---|---|
| [v0_to_v01.angular](v0_to_v01.angular) | [v0_to_v01_pdf.txt](v0_to_v01_pdf.txt) |
| [shared_bound_states.angular](shared_bound_states.angular) | [shared_bound_states_pdf.txt](shared_bound_states_pdf.txt) |
