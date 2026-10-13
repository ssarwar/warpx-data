This folder contains collision cross-sections for electron-neutral and
ion-neutral scattering processes that can be used with the MCC routine.

Each cross-section file should consist of exactly 2 columns, the first
containing impact energy in eV and the second the collision cross-section in
m^2. Legacy WarpX MCC readers require equally spaced energies. The N2/O2 IAA
tables use adaptive grids and require the nonuniform-grid reader in the
`codex/beam-air-development-sync-2026-10-08-r2` branch of ssarwar/WarpX or its successors.
Their elastic differential tables have a separate angular layout, documented
in those directories.

Three-body attachment tables explicitly marked m^5 require multiplication by
the specified third-body number density in m^-3 before use as an effective m^2
cross section. They do not contain a gas density.

Only production physical data, source provenance, and usable collision manifests
belong in this repository. Generate synthetic verification inputs and audit
outputs in the WarpX build directory using Tools/CrossSections; do not add them here.
