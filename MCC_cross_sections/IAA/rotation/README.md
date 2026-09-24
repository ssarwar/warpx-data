# Rotational source gates

These records are **not production collision tables**. The current WarpX
model samples the existing elastic DCS and conditions integral rotational
probabilities only on exact energy/recoil accessibility. It does not use a
separate rotational angular distribution.

`Tools/CrossSections/audit_iaa_rotation.py` regenerates these records from
archive SHA256 b864381086120fff37eb8c658dfcb0f150b80969cf9add626a38d89bab0a3c38.
The zero-temperature reference population is an explicit audit assumption;
the inclusive elastic measurements do not establish that temperature.

Both tested unchanged **integral** residuals are nonnegative over the audit
range. The negative angular residuals in the earlier spectator/Born
construction do not apply to this model. N2 uses corrected elementary
integral rates and sudden scaling. O2 uses an angular-integrated Born rate
approximation, without using that approximation to sample angles.

Production still requires validated low-energy rate continuations,
convergence in omitted elementary rotational ranks, and high-energy rate
coverage or a demonstrated omitted-effect bound. In particular, using the
elastic DCS does not extend the validity of O2's sub-eV Born rate model.

WarpX V2 bundles contain integral rates only. Its analytic verification
bundles test software, integral detailed balance, signed recoil, and thermal
power balance; they do not establish N2/O2 beam-physics accuracy.
