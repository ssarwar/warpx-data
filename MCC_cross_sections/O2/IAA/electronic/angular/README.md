# O2 electronic angular distributions

Pass a `.angular` descriptor as `differential_cross_section`. Each descriptor
references its normalized `_pdf.txt` profile and the shared elastic fallback.
The profile contains incident energy followed by values in sr⁻¹ at 0°, 1°, …,
180°. It supplies only angular probability; the integral collision rate and
energy loss come from the parent directory and gas manifest.

[sources.json](sources.json) records measured rows, canonical mappings, endpoint
completion, transitions and source hashes.

| Descriptor | Profile |
|---|---|
| [schumann_runge.angular](schumann_runge.angular) | [schumann_runge_pdf.txt](schumann_runge_pdf.txt) |
| [herzberg_continuum.angular](herzberg_continuum.angular) | [herzberg_continuum_pdf.txt](herzberg_continuum_pdf.txt) |
| [longest_band.angular](longest_band.angular) | [longest_band_pdf.txt](longest_band_pdf.txt) |
| [a_1Delta_g.angular](a_1Delta_g.angular) | [a_1Delta_g_pdf.txt](a_1Delta_g_pdf.txt) |
| [b_1Sigma_g_plus.angular](b_1Sigma_g_plus.angular) | [b_1Sigma_g_plus_pdf.txt](b_1Sigma_g_plus_pdf.txt) |
| [second_band.angular](second_band.angular) | [second_band_pdf.txt](second_band_pdf.txt) |
