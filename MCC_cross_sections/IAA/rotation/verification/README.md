# Independent-angle rotational verification inputs

These are synthetic software tests, not production N2/O2 cross sections.
All files are generated offline. WarpX simulations and benchmarks only load
these files; they do not need elmolcs or a data-generation step at startup.

Each gas has a V3 binary `.rot` bundle covering 0–10 eV, an inclusive elastic
cross section of 2e-20 m², an ordinary comparison channel of 1e-21 m², and
isotropic/anisotropic DCS inputs. The anisotropic shape is
`1 + 3*sin(theta/2)`; its absolute normalization is arbitrary because the
inclusive integral supplies the rate. The rotational reduced amplitude is
1e-20 m², with canonical rigid-rotor thresholds and heavy-target integral
detailed balance. The reference population is at zero temperature. State
coverage is verified through 1000 K. See `manifest.json` for file hashes.

Regenerate from the matching WarpX branch, before any simulation:

```sh
python Examples/Tests/collision/analysis_rotation_reference.py --export-only /path/to/warpx-data/MCC_cross_sections/IAA/rotation/verification
```

Pass this directory to the WarpX rotational PICMI verification input or
`Tools/CrossSections/benchmark_rotation.py` using `--data-dir`.
