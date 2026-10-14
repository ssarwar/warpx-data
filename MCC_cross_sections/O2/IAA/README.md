# Electron–O2 IAA collision data

Start with [manifest.json](manifest.json), the complete production process ledger
and default model selection for this gas. Table paths are relative to this
`IAA` directory, so the gas dataset can be moved as a unit. Original-source
filenames in provenance refer to the cited archives.

| Directory | Contents |
|---|---|
| [elastic/](elastic/README.md) | Inclusive residual elastic integral and elastic angular DCS |
| [rotational/](rotational/README.md) | Combined elastic/rotation model, elementary transitions and sources |
| [vibrational/](vibrational/README.md) | Individual bound vibrational transitions and shared angular profiles |
| [electronic/](electronic/README.md) | Identified electronic states/bands and complementary excitations |
| [dissociation/](dissociation/README.md) | The separately retained neutral-dissociation channel |
| [ionization/](ionization/README.md) | Mutually exclusive RBEQ total cross sections |
| [attachment/](attachment/README.md) | Dissociative and three-body attachment alternatives |

## File conventions

- Production integral tables are directly inside each collision-family directory.
  They contain energy in eV and cross section in m²; three-body attachment uses
  m⁵. Headers specify bounds, energy losses, sources and continuations.
- `source_bounded/` contains the corresponding tables limited to their source
  coverage. It is an alternative energy coverage, not an additional process.
- `angular/` contains `.angular` reader descriptors and normalized `_pdf.txt`
  profiles in sr⁻¹. These describe angular probabilities, not extra event rates.
- `sources.json` records physical source values, citations/hashes and modeling
  choices for its directory. Rotational source inputs are in `rotational/sources/`.
- Electronic filenames include molecular-state labels. For example,
  `a_1Delta_g` and `b_1Sigma_g_plus` identify the two low-lying singlet states.
  Vibrational names such as `v0_to_v01` name
  the initial and final vibrational quantum numbers.

The default completed IAA set uses production coverage through 1 GeV, the
300 K combined elastic/rotation family and the `iaa_thesis_2023` ionization
model. Energy coverage does not imply experimental validation throughout that
range. Source limits and modeled continuations remain explicit in the files.

The combined rotational family already includes elastic scattering. Do not add
its elementary rotational source tables as additional collision rates. Neutral
excited-state populations and neutral fragments are not evolved.

See the [common model specification](../../README.md#common-iaa-model-specification)
for angular prescriptions, source repairs, process accounting and attachment
limitations. Software tests, synthetic fixtures, plots and benchmarks belong
in WarpX build directories, not in this production-data repository.
