# Rotational source audits and verification inputs

The physical source-audit records are **not production collision tables**.
The `verification/` directory contains offline-generated synthetic V3 inputs
for software and performance tests, clearly distinguished from molecular data.

The current WarpX model draws the elastic DCS angle independently of unchanged,
excitation and de-excitation outcomes. Excitation requires the canonical rotor
level spacing. Recoil shifts and angular-accessibility filters are omitted
from the rates and branching probabilities. A bounded heavy-target kinematic
continuation retains nominal thresholds near the very small recoil boundary.

WarpX `Tools/CrossSections/audit_iaa_rotation.py` regenerates these audits from
archive SHA256 b864381086120fff37eb8c658dfcb0f150b80969cf9add626a38d89bab0a3c38.
The default zero-temperature reference is an explicit audit assumption;
`--reference-temperature 300` reproduces the additional 300 K diagnostics.
The source measurements do not establish zero temperature as their reference.

Both zero-temperature unchanged integral residuals are nonnegative over the
audit range. The 300 K candidates have negative low-energy residuals with the
current extrapolations: finite inclusive cross sections give v*sigma -> 0,
whereas thermally populated superelastic channels have a finite limiting rate.
These extrapolations require correction before production use. Negative
residuals are not clipped. The earlier spectator/Born angular-residual issue
does not apply to the independent-angle model.

N2 uses corrected elementary integral rates and sudden scaling. O2 uses an
angular-integrated Born approximation for rates only; the elastic angular
sampler does not extend that approximation's sub-eV validity. Validated rate
continuations, omitted-rank moment convergence and beam-energy coverage or
an omitted-effect bound remain production requirements.

V3 bundles contain integral rates only. V1/V2 files must be regenerated.
Their source rates obey detailed balance in the heavy-target limit with
relativistic electron momenta. The independent elastic-angle approximation
does not enforce differential detailed balance. Physical data are generated
offline and versioned here; simulation startup only validates and prepares
in-memory sampling tables at the selected rotational temperature.
