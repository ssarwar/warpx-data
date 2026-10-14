# O2 RBEQ electron-impact ionization

[rbeq_iaa_thesis_2023.txt](rbeq_iaa_thesis_2023.txt) is the default;
[rbeq_elmolcs_b8643810.txt](rbeq_elmolcs_b8643810.txt) is the alternative.
Select exactly one total and its matching secondary-electron model.

These tables contain electron energy in eV and the positive-part RBEQ total in m²,
from the lowest shell threshold to 1 GeV. The model and normalization comments
are recognized by WarpX. Select the matching `rbeq_model` and `rbeq_target`.
The angular prescription is selected separately.

`iaa_thesis_2023` follows Table 11.12 and Eq. 11.121 of A. Schmalzried's thesis.
`elmolcs_b8643810` follows the fitted shell blocks and `_RBEQ` kernel of archive
SHA256 b864381086120fff37eb8c658dfcb0f150b80969cf9add626a38d89bab0a3c38.
The archive's `genRBEQ` helper and default orbital data are different models;
see WarpX's `Docs/source/theory/multiphysics/mcc_iaa_sources.rst` for the audit.

Regenerate with WarpX `Tools/CrossSections/export_rbeq.cpp` using its shared
host source definition. Adaptive knots target 0.02% linear-interpolation error,
with an absolute floor of 1e-8 of peak. The grid remains strictly increasing in
float32. Independent differential quadrature checks all four tables through
`Examples/Tests/collision/analysis_rbeq_sources.py`.

The explicit energy-range metadata reject queries above the adopted 1 GeV
model limit. These files do not establish experimental accuracy through that
range. Raw signed curves can be exported for comparison but are not production
collision tables.
