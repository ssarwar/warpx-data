# O2 electronic data

Individual electronic states and bands retain separate rates and prescribed losses. Complementary excitations belong to this completed IAA set.

Integral tables here are the production coverage through 1 GeV. Matching
[source_bounded/](source_bounded/) files stop at their declared source limits.
The [source ledger](sources.json) preserves the underlying physical records.

[angular/](angular/) contains shared angular distributions and their provenance.
Channels without supported inelastic DCS use the normalized elastic shape
identified by the gas manifest, with their own rate and energy loss.

The 6.12 eV and 8.60 eV Schumann–Runge files partition one analytic total.
They share one angular profile; the bands must not also be added as an aggregate rate.

| Production file | Configured loss (eV) | Units | Selection |
|---|---:|---|---|
| [schumann_runge_6.12_eV.txt](schumann_runge_6.12_eV.txt) | 6.12 | m2 | included |
| [schumann_runge_8.60_eV.txt](schumann_runge_8.60_eV.txt) | 8.6 | m2 | included |
| [herzberg_continuum.txt](herzberg_continuum.txt) | 6 | m2 | included |
| [longest_band.txt](longest_band.txt) | 9.97 | m2 | included |
| [a_1Delta_g.txt](a_1Delta_g.txt) | 0.977 | m2 | included |
| [b_1Sigma_g_plus.txt](b_1Sigma_g_plus.txt) | 1.627 | m2 | included |
| [second_band.txt](second_band.txt) | 10.29 | m2 | included |
| [complement_11_eV.txt](complement_11_eV.txt) | 11 | m2 | included |
