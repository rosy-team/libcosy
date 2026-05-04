# libcosy

ROSY port of COSY INFINITY 10.2 — the canonical library for [Rosy](https://github.com/rosy-team/rosy), a scientific programming language for beam physics and differential algebra.

## Use

```rosy
BEGIN;
  MODULE GITHUB "rosy-team/libcosy" "v1.0.0";

  OV 2 2 0;          { order 2, 2D phase space, 0 parameters }
  RPP 100;           { 100 MeV proton reference particle }
  UM;                { identity map seed }

  DL 0.5;            { 0.5 m field-free drift }
  MQK 0.2 0.5 0.05;  { quad: L=0.2 m, K=+0.5 m^-2, aperture 5 cm }

  PM 6;              { print 6-component one-pass map to stdout }
END;
```

The `MODULE GITHUB` directive fetches the tagged source tarball from this repo on first use and caches it under `.rosy_output/packages/libcosy-<version>/`. Subsequent runs reuse the cache.

## Layout

- `physics/` — beam-physics elements (drifts, dipoles, quads, sextupoles, RF, …) and analysis (Twiss, normal form, Lie factorization, symplectification, …)
- `helpers/` — DA primitives, polynomial ops, I/O
- `globals.rosy` — shared state (`ND`, `TWOND`, `NV`, `P0`, `E0`, …)

## Versioning

`Rosy.toml` declares `rosy_version` as a [semver requirement](https://semver.org) the running Rosy transpiler must satisfy.

## Releasing a new version

1. Bump `[package].version` in `Rosy.toml` (semver: patch for fixes, minor for additions, major for breaks).
2. Commit and push to `master`.
3. `.github/workflows/release.yml` extracts the new version, tags `v<version>`, and publishes a GitHub Release with auto-generated notes. The source tarball is auto-attached and becomes the artifact `MODULE GITHUB ... "v<version>"` resolves against.

The workflow only fires when `Rosy.toml` is in the diff, and silently skips if the tag already exists — so non-version commits and re-pushes are no-ops.
