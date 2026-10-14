# O2 attachment data

Select exactly one dissociative source and one three-body source. IAA dissociative and MuroranIT three-body attachment are the defaults. Three-body files contain raw m⁵ values; multiply by the O₂ stabilizer density outside the file. The target O₂ density enters the collision rate separately. See the common specification for the effective-rate approximation and pressure/temperature limits.

Integral tables here are the production coverage through 1 GeV. Matching
[source_bounded/](source_bounded/) files stop at their declared source limits.
The [source ledger](sources.json) preserves the underlying physical records.

| Production file | Configured loss (eV) | Units | Selection |
|---|---:|---|---|
| [dissociative_iaa.txt](dissociative_iaa.txt) | 0 | m2 | default |
| [three_body_muroran.txt](three_body_muroran.txt) | 0 | m5 | default |
| [dissociative_muroran.txt](dissociative_muroran.txt) | 0 | m2 | alternative |
| [three_body_phelps.txt](three_body_phelps.txt) | 0 | m5 | alternative |
| [three_body_biagi.txt](three_body_biagi.txt) | 0 | m5 | alternative |
