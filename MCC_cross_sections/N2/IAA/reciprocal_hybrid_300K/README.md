# Electron–N2 vibrationally elastic and rotational scattering, 300 K

This directory is **one combined collision family**, covering collision
energies from **0 to 1 GeV** at a prescribed **300 K rotational temperature**.
It includes rotationally unchanged scattering, excitation and de-excitation.
The files contain real elmolcs-based distributions with the published
constraints and explicit model continuations described below. They are not
synthetic test fixtures. No neutral state populations are evolved.

The complete derivation, source assessment, equations and algorithms are in
[WarpX's multiphysics theory manual](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering.rst).
The pages cover [physical quantities](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering/physics.rst),
[source construction](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering/sources.rst),
[reciprocity and normalization](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering/reciprocity.rst),
[continuations](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering/continuations.rst),
[sampling and recoil](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering/algorithm.rst), and
[the full file specification](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering/data_format.rst).
Validation and performance results are maintained with WarpX, not in this
production data directory.

## Using these files

Configure one electron Background MCC elastic process with:

```text
cross_section = ../elastic.txt
scattering_angle_model = IAA
rotation_model = reciprocal_hybrid
rotation_file = thermal_rotation.rot
rotational_temperature = 300
rotation_sampling = alias
```

These are per-process parameter suffixes; prepend the collision/process names
as required by WarpX. Paths shown are relative to this directory for explanation;
ordinary WarpX input paths are resolved from the run directory. The proton-beam
example resolves its paths relative to its JSON manifest.

The ordinary elastic table checks source consistency. Its rate is replaced by
the combined rate, not added to it. Do not configure the elementary rotational
tables as extra collision processes. This option supplies its own elastic
angular distribution; do not also set `differential_cross_section`.
Translational temperature is independent. The requested rotational temperature
must match this bundle; another rotational temperature requires separately
prepared physical data.

## What the physical data mean

Let E be electron kinetic energy in eV and let ε(J) be the molecule's rotational
energy above its ground rotational level. The canonical level model is

```text
ε(J) = B [J(J+1) − Jg(Jg+1)]
B = 0.0002477204284695341 eV; Jg = 0
Δ = ε(final) − ε(initial)
```

For O2 the common code index J denotes nuclear rotation N, with electronic
spin unresolved. N2 has statistical weights 6(2J+1) for even J and 3(2J+1)
for odd J. O2 permits only odd N with weight 2N+1. The equilibrium population
is proportional to `g(J) exp[−ε(J)/(kB T)]`; populations and state sums were
included in constructing this fixed-temperature data set.

Positive Δ means electron energy loss (rotational excitation); negative Δ
means electron gain (de-excitation). Zero Δ still means a real deflection and
elastic recoil. All gaps and level labels remain discrete; interpolation never
creates an intermediate level. The source scattering approximation uses the
nominal threshold E ≥ Δ without imposing additional recoil-angle vetoes.
WarpX subsequently applies signed internal-energy exchange and molecular recoil,
with the documented narrow continuation at the nominal excitation threshold.

The IAA residual elastic integral and elastic DCS include unresolved rotation.
The normalized angular shape is the supplied elastic DCS divided by its own
integral; its absolute normalization is the residual integral. The latter
need not equal the unmodified DCS integral. The Kutz–Meyer and other elementary
rotational strengths cannot simply be added to this total or subtracted without
checking positivity.

Below 1 keV the source model uses a common positive forward primitive X:

```text
P(E) = sqrt[E(E + 2 me c²)]
D_if(E,θ) = P(E−Δ)/P(E)² X_if(E,θ)
D_fi(E−Δ,θ) = (g_i/g_f) X_if(E,θ)/P(E−Δ)
```

Thus `g_i P(E)² D_if = g_f P(E−Δ)² D_fi` in the heavy-target approximation.
The inclusive constraint sums these paired kernels over the bath. Reverse
terms use corrected forward data at E+Δ. Independently normalizing each
incoming row afterward would generally break that identity. Changing kernels
are protected where justified by the sources; elsewhere a common paired
normalization reconciles them with the IAA total and angular marginal.
A negative physical unchanged remainder is rejected, not clipped.

The runtime samples the angle from the inclusive distribution and then the
unchanged/change outcome conditional on that angle. Among changed outcomes it
samples a discrete signed Δ. This preserves energy diffusion that would be
lost by applying only the average rotational loss of thesis Eq. 2.48.
Above 1 keV the bounded spectator momentum-transfer spectrum is an asymptotic
approximation: exact finite-energy differential balance is not claimed there.
Finite sampling and interpolation approximate the continuous low-energy model.

## Energy continuations and physical limits

* At E=0 the total rate K is finite from superelastic events and the emission
  direction is isotropic. Below 1 meV a specified s-wave unchanged background
  joins the reciprocal changing kernels. A finite ordinary cross section
  multiplied by zero speed would incorrectly eliminate this heating.
* From 1 meV through 6 keV the combined rate follows the IAA residual.
  Above 6 keV it follows the continuously matched elmolcs fitted Born model
  through 1 GeV. This is a modeled continuation, not constant endpoint holding.
* The angular marginal joins screened Rutherford smoothly between 8 and
  10 keV, and remains energy dependent above the join.
* Rotational sources have shorter ranges: N2 elementary curves end at 1 keV;
  the O2 ground Born integral ends at 20 eV. The combined family uses the
  gas-specific hybrid and bounded spectator continuation, not those endpoints
  held constant. Reverse evaluations include the required E+Δ support during
  preparation without extending the advertised runtime range.
* Bound rigid-rotor final levels stop at J=197 for N2 and odd N=167 for O2.
  This closure does not model dissociation, non-rigid high-J spectroscopy,
  vibrationally excited rotors or resolved O2 spin structure.
* Queries above 1 GeV fail. An eight-particle-machine-epsilon relative upper
  allowance is solely for endpoint roundoff. There is no arbitrary zero tail.

Physical uncertainty in source theory, digitization and unmeasured angular
ranges is distinct from numerical accuracy. In particular the O2 1–20 eV
bridge is not an experimentally validated resonance DCS, and the high-energy
sudden approximation neglects finite rotational-gap corrections.

## Readable files and units

`thermal_rotation.rot` is a UTF-8 V7 index. Every payload in this directory is
plain text. The arrays are numerical representations of a joint distribution,
not independent processes. Zero-based indices refer to other arrays as follows.

| Files | Contents |
|---|---|
| `energies.txt`, `rates.txt` | E in eV; K=vσ in m³/s, including its finite zero-energy limit |
| `coordinates.txt` | 0=linear energy mixing, 1=square-root mixing; last entry unused |
| `angular_offsets.txt` | Start/end of each energy row, including final sentinel |
| `angular_u.txt`, `deflection.txt` | Angular CDF probability u; deflection d=1−cosθ |
| `changing.txt` | Probability of a nonzero rotational change at that angular node |
| `conditional_offsets.txt`, `conditional_u.txt` | Energy-row offsets and angular quantiles for changing spectra |
| `conditional_cells.txt` | Cell identifiers at those quantiles |
| `cell_offsets.txt` | Start/end of expanded alias entries for each probability cell |
| `outcomes.txt` | Repeated pairs `(signed loss Δ, initial internal energy εi)`, both eV |
| `high_edges.txt`, `high_cells.txt` | Dimensionless z=kR sin(θ/2) bin edges and cell identifiers |
| `probabilities.txt` | Compact numerical factors and exact supports for discrete probabilities |

Ordinary array files allow comments beginning with `#`. A `values N` line is
followed by N decimal numbers. `repeat N value` repeats one number. `copy N start`
reuses an already written row beginning at zero-based index `start`; its entire
range must precede the copy. Reuse changes no values. Float64 and float32 numbers
carry sufficient digits to round-trip exactly. Integer types are checked.

The index starts with `WARPX_THERMAL_ROTATION_V7`, the target/model and
`probabilities`, then seven physical values: temperature in K, mass in kg,
maximum energy in eV, high-energy switch in eV, Rutherford switch in eV,
separation in Bohr radii, and screening radius in Bohr radii. After the ordinary
array count, each line gives `name type count filename`. The final line
`probabilities alias count probabilities.txt` declares the expanded alias count
and is additional to the ordinary array count.

`probabilities.txt` begins with `WARPX_PROBABILITY_FACTORS_V1`, the cell count
and expanded entry count. A block begins with
`block first_cell cell_count outcome_count factor_rank`. Each outcome line
contains its palette index, a count of half-open support intervals within that
block, their `[first,end)` endpoints, then its left-factor coefficients. The
remaining lines contain one right-factor row per cell. Each block has at most
256 cells. The factor rank is purely numerical; it is **not** a molecular
angular-momentum transfer rank.

For left factors L, right factors R and exact support mask S, reconstruct

```text
q(outcome,cell) = S(outcome,cell) max[sum_k L(outcome,k) R(cell,k), 0]
p(outcome,cell) = q(outcome,cell) / sum_outcomes q(outcome,cell)
```

Signed coefficients are mathematical encoding coefficients, not negative
physical cross sections. The small numerical projection to nonnegative
probabilities is checked during export against the original distribution,
including rare energy-transfer tails; it must not be confused with clipping a
negative physical elastic residual. Supports preserve all exact forbidden
outcomes. The reader rejects nonfinite or inconsistent factors and excessive
negative reconstruction or normalization defects.

## Why initialize instead of shipping binary caches?

The original V6 files stored about 87 million 8-byte alias entries across both
gases. Those entries repeated closely related distributions at many angular
nodes. The 24 `.bin` parts were just 32 MiB slices of that memory layout, with
no physical meaning assigned to individual files.

V7 stores the probability information in readable factored form. WarpX reads
and reconstructs it, prepares aliases (or cumulative reference arrays) and exact
quantile-search bounds **once at initialization**, then uploads immutable arrays.
No fitting to papers, nonlinear source solve, elmolcs dependency, Python call
or per-particle table construction occurs in the simulation. GPU collision
sampling retains its constant-time alias step and discrete energy changes.
It creates no new data files at startup. Additional collision objects sharing
the same path/temperature/sampling mode reuse the in-memory tables.

The text data are smaller than the former device-memory images but remain
substantial: they represent energy- and angle-dependent distributions over
many rotational outcomes. This encoding separates a reviewable input format
from the optimized device representation. Historical binary files remain in
older Git commits; this update does not rewrite repository history. V6 readers
remain supported for existing inputs. The separate parent-directory N2 V5
`thermal_spectator.rot` is legacy binary data for the older optional model.

The device alias entry is eight bytes: float32 cutoff plus uint16 alternate
and outcome indices. Canonical energies and recoil calculations use double.
The combined table allocation is bounded by 1 GiB per MPI process. Use one
MPI process per GPU to avoid duplicating that allocation on one device.
Double particle storage is recommended when retaining meV changes in MeV
particle energies matters.

## Reproducing or extending these data

The maintained exporter and independent checks live in WarpX
`Tools/CrossSections/reciprocal_rotation`. Follow its README to prepare the
source reference, refine it, validate physical and numerical moments, and
encode V7 with `text_bundle.py`. The simulation's C++ reader independently
reconstructs the probabilities; `check_text_cells.py` compares every decoded
cell, including float32 alias packing, with the original validated distribution.
Only validated production inputs and numerical source data belong here.
Put intermediate V6 caches, additional-temperature test fixtures, reports and
benchmarks in a build directory outside warpx-data.
