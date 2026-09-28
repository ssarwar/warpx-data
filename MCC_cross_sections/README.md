This folder contains collision cross-sections for electron-neutral and
ion-neutral scattering processes that can be used with the MCC routine.

Each cross-section file should consist of exactly 2 columns, the first
containing impact energy in eV and the second the collision cross-section in
m^2. Legacy WarpX MCC readers require equally spaced energies. The N2/O2 IAA
tables use adaptive grids and require the nonuniform-grid reader in the
`codex/rigid-beam-immobile-ions-development-sync` branch of ssarwar/WarpX.
Their elastic differential tables have a separate angular layout, documented
in those directories.
