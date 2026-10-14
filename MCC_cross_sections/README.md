# Monte Carlo collision cross sections

Data are organized by target gas. Existing Ar, H, He and Xe datasets retain
their established layouts. The complete electron–air IAA data are in:

- [N₂ / IAA](N2/IAA/README.md)
- [O₂ / IAA](O2/IAA/README.md)

```text
MCC_cross_sections/
├── N2/IAA/
│   ├── README.md
│   ├── manifest.json
│   ├── elastic/
│   ├── rotational/
│   ├── vibrational/
│   ├── electronic/
│   ├── dissociation/
│   └── ionization/
└── O2/IAA/
    ├── README.md
    ├── manifest.json
    ├── elastic/
    ├── rotational/
    ├── vibrational/
    ├── electronic/
    ├── dissociation/
    ├── ionization/
    └── attachment/
```

Production integral tables sit directly inside each family. `source_bounded/`
contains source-limited counterparts; `angular/` contains excitation angular
profiles and descriptors. Each gas manifest resolves table paths from its own IAA
directory and preserves canonical reaction identifiers and exclusive alternatives.

Ordinary cross-section files have two columns: energy in eV and cross section
in m², or explicitly marked m⁵ for three-body attachment. Elastic DCS, normalized
excitation PDFs and combined rotation bundles have documented separate formats.
Adaptive tables require the nonuniform-grid WarpX reader. Bounds and `#` comments
are retained. Three-body density belongs in the simulation input, not in a file.

Only production physical data, provenance and usable manifests belong here.
Generate tests, synthetic fixtures, plots and audit/benchmark outputs in WarpX
build directories. No Git history has been rewritten.

## Updating older paths

- `IAA/air/manifest.json` is replaced by each gas's `IAA/manifest.json`.
- `<gas>/IAA/elastic.txt` and `elastic_dcs.txt` are now
  `elastic/cross_section.txt` and `elastic/differential_cross_section.txt` below that gas's IAA directory.
- `reciprocal_hybrid_300K/` is now `rotational/elastic_rotation_300K/`.
- Elementary `rotation_0_2.txt`-style filenames now explicitly identify the
  transition, for example `rotational/state_to_state/J0_to_J2.txt`.
- `IAA/RBEQ/<gas>_<model>.txt` is now `<gas>/IAA/ionization/rbeq_<model>.txt`.
- The old `air/production`, `air/source` and `air/angular` contents are grouped
  by physical family, with descriptive state/transition filenames.

Use the current WarpX `load_iaa_air_processes` helper with the MCC_cross_sections
root and target gas. It reads the gas-local manifests and retains support for
older aggregate manifests. Native input decks need their literal paths updated.

## Common IAA model specification

Each gas-local `manifest.json` selects the completed IAA model without evolving neutral
internal states. The default N2 configuration contains the existing combined
elastic/rotation family, 15 vibrations, residual dissociation, 17 electronic
bands, the 7/14 eV complements, and one source-consistent RBEQ ionization process.
O2 contains the combined family, 41 vibrations, resonant vibrational
dissociation, eight electronic loss channels (including two Schumann-Runge
pieces and the 11 eV complement), one ionization process, and two attachment
reactions. Alternatives are exclusive, not additive.

Within each gas, the excitation, dissociation and attachment family directories contain:

* Integral `.txt` files: explicitly documented production coverage through 1 GeV.
* `sources.json`: original physical source records, coefficients and corrections.
* `source_bounded/`: integral tables limited to the supplied source coverage.
* `angular/`, where applicable: normalized profiles, elastic fallback descriptors,
  measured coverage and physical completion provenance.

Only production physical data and provenance are stored here. Verification
fixtures, sensitivity variants, plots, audit outputs and benchmarks belong in
WarpX or its build directory.

### Sources and accounting

The elmolcs archive is pinned by SHA256
`b864381086120fff37eb8c658dfcb0f150b80969cf9add626a38d89bab0a3c38`.
Each record preserves the original process label, source location and citation.
The thesis reference is A. Schmalzried, 2023, chapters 4, 11 and 15.
Reproduce these files using WarpX `Tools/CrossSections/export_iaa_air.py`.
The corresponding runtime and exporter are on ssarwar/WarpX
`codex/iaa-electron-air-production`.

Do not add separate elementary rotations, aggregate excitation rates, effective
momentum-transfer data or total dissociation alongside the selected processes.
N2 residual dissociation already subtracts predissociation. Dissociative
electronic excitation is counted once. The complementary channels follow the
source completion convention without changing the existing elastic residual.
The N2 v=1 data restore the omitted 20 and 30 eV source points. O2's two
Schumann-Runge losses, 6.12/8.6 eV, partition one analytic total continuously
between 9.955581 and 10.213826 eV. Source thresholds, fitted thresholds, tabulated
onsets and configured losses are recorded separately.

Uppercase N2 triplet names use `triplet_` filenames to distinguish A/a, B/b and
W/w on case-insensitive filesystems. The logical spectroscopic identifiers
remain unchanged in the process ledger.

### Angular models

Explicit thesis recommendations take precedence. N2 bound vibrations use the
d-pi law below 5 eV, joining measured v=0->1 scattering or elastic IAA over
5--10 eV. O2 bound vibrations use isotropy through 20 eV and join elastic IAA
over 20--40 eV. The O2 boundaries are an adopted approximation, not a numerical
limit stated in the thesis.

Electronic profiles use Khakoo 2005/2008 and Malone 2009 for N2;
Shyn-Sweeney 1993/2000 and Suzuki 2011 for O2. In particular:

* [Khakoo 2005](https://doi.org/10.1103/PhysRevA.71.062703)
* [Khakoo 2008](https://doi.org/10.1103/PhysRevA.77.012704)
* [Malone 2009](https://doi.org/10.1103/PhysRevA.79.032704)
* [Linert-Zubek 2009](https://doi.org/10.1088/0953-4075/42/8/085203)
* [Shyn-Sweeney 1993](https://doi.org/10.1103/PhysRevA.47.1006)
* [Shyn-Sweeney 2000](https://doi.org/10.1103/PhysRevA.62.022711)
* [Suzuki 2011](https://doi.org/10.1063/1.3533442)

The provenance records actual per-energy angular coverage. Missing endpoints
use boundary-matched elastic shapes; their modeled probability is recorded.
Narrow-cone N2 100 eV and O2 100--200 eV measurements remain reference data.
Experimental profiles are normalized independently of the integral rate.
Outside documented transition regions, the normalized elastic IAA shape is
used without introducing another elastic or rotational event.

Two O2 b-state archive entries are corrected against Table I(b), p. 1007 of
[Shyn-Sweeney 1993](https://doi.org/10.1103/PhysRevA.47.1006): 94 becomes 9.4
at 5 eV/12 degrees and 89 becomes 8.9 at 15 eV/36 degrees, in units of
1e-20 cm2/sr. The decimal points are visible in the published table but also
missing from the PDF text layer. `O2/IAA/electronic/angular/sources.json`
records the original values, corrections, citation and verified PDF SHA256.
The normalized angular profile is regenerated; integral rates and energy losses
are unchanged.

`.angular` files use `WARPX_EXCITATION_ANGULAR_V1`, followed by the target,
relative profile path, relative elastic-fallback path and active energy bounds.
Profile rows contain energy followed by 181 values at uniformly spaced angles
from 0 to 180 degrees. Values are normalized angular probability densities in
sr^-1. Startup constructs shared inverse CDFs; simulation kernels do not fit
or generate physical cross sections.

### Ionization and attachment alternatives

RBEQ defaults to `iaa_thesis_2023`; `elmolcs_b8643810` is a complete alternative
including its matching secondary model. The manifest references the existing
RBEQ tables, with explicit bounded-range metadata.

O2 dissociative attachment defaults to IAA; MuroranIT is an alternative.
Three-body attachment defaults to MuroranIT, with Phelps and Biagi alternatives.
The archived reference-density Phelps/Biagi numbers are multiplied by 1e-6 to
obtain m5. MuroranIT's supplied m5 values are preserved. Their peak differs by
0.0177724% from the nominal normalization in
[Kawaguchi et al. 2025](https://doi.org/10.1088/1361-6595/ade626), section 2.6.

Keep the third-body density outside the files. The supported default uses O2
as stabilizer, so the collision frequency is `n_O2 * n_O2 * v * sigma_m5`.
Do not use total air density or infer an N2 stabilization contribution from
these files. The effective-rate approximation does not resolve the complete
pressure-dependent competition with resonant autodetachment.

### Runtime use

Use `pywarpx.iaa_air.load_iaa_air_processes` with this repository's
`MCC_cross_sections` path, the target, product species, and explicit O2
third-body density. Pass its dictionary to `picmi.MCCCollisions`.
The helper selects the existing 300 K reciprocal rotational family.

Integral files use eV and m2 (or explicitly marked m5), with `#` comments.
The `native_energy_grid = resample` metadata permits startup interpolation onto
unique representable single-precision energies while retaining the accurate
binary64 reference grid. Values outside the declared upper energy range are
errors. Production continuations and interpolation accuracy do not establish
experimental validity through 1 GeV.
