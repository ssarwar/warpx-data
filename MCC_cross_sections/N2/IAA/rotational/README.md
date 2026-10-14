# N2 elastic and rotational scattering

Use [elastic_rotation_300K/thermal_rotation.rot](elastic_rotation_300K/thermal_rotation.rot)
for the default **single combined elastic/rotation collision family**. Its
[README](elastic_rotation_300K/README.md) describes initialization, units,
physical sources, signed energy transfers and continuations through 1 GeV.

- `elastic_rotation_300K/`: the prepared reciprocal model and its readable
  probability/lookup arrays. The array files form one distribution; they are
  not separate collision processes.
- `state_to_state/`: elementary integral source cross sections. Filenames such
  as `J0_to_J2.txt` identify the rotational levels (the O₂ index denotes nuclear
  rotation with electronic spin unresolved). These are ingredients and source
  references, not additional rates to add to the combined family.
- [sources/](sources/README.md): published numerical inputs and source provenance.
- `spectator/thermal_spectator.rot`: the older optional kinetic IAA spectator
  model. It is an alternative to the reciprocal family.

The ordinary consistency-check cross section is
[../elastic/cross_section.txt](../elastic/cross_section.txt).
