# Rotational source gates

These audit records are **not production collision tables**. No rotational
bundle is approved for the beam simulation by the supplied data.

WarpX `Tools/CrossSections/audit_iaa_rotation.py` regenerates the records from
archive SHA256 b864381086120fff37eb8c658dfcb0f150b80969cf9add626a38d89bab0a3c38.
The test uses a zero-temperature reference population as an explicit modeling
assumption; the supplied inclusive elastic data do not establish this temperature.
N2 uses the corrected elementary rates, sudden scaling, and spectator shapes.
O2 uses the thesis long-range quadrupole/anisotropic-polarization Born model.
Both tested decompositions contain negative unchanged angular-bin rates.
Neither is clipped, rescaled, or exported as a production bundle.

Additional unresolved gates are the low-energy and resonant angular closure,
convergence in elementary rotational rank, and a justified high-energy
continuation or bound. Passing table interpolation tests cannot establish
those missing physical inputs. The positive-rate analytic bundles in WarpX's
tests validate software and detailed balance, not N2/O2 beam physics.
