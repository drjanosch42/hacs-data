<!-- SPDX-FileCopyrightText: 2024-2026 Fabian Januszewski -->
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
# HACS census data

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24118877.svg)](https://doi.org/10.5281/zenodo.24118877)

*Data sets produced with HACS, the Harder Arithmetic Cohomology System: high-performance simplicial orbifold cohomology implementation for arithmetic groups (C++ / AVX-512 / CUDA).*

**In memoriam Prof. Dr. Günter Harder (1938-2025).**

This repository holds the **signed editions** of the HACS census data: tables of Hecke algebras and Hecke modules in the cohomology of
SL_2(Z) and of Gamma_0(N) with coefficients in the symmetric powers of the standard representation. The software that produced them is the
HACS code repository (a separate repository; not part of this one), Zenodo concept DOI
[10.5281/zenodo.24118841](https://doi.org/10.5281/zenodo.24118841).

## Contents

| Directory | Data set | Edition | Status |
|---|---|---|---|
| `census/congruence/4.0/` | Congruence census: Hecke algebras mod ell of H^1(SL_2(Z); Sym^n F_ell^2), one row per maximal ideal; ell = 5 ... 199 and 547, 691, even n <= 1000 | 4.0 (two table sections: `rows40.tsv.gz` main, `rows40_b1.tsv.gz` b1) | signed 2026-10-27 |
| `census/integral/edition1/` | Integral census: Hecke modules over Z of the free and torsion parts, descent cells (N, n) | edition 1 | signed 2026-11-03 |
| `census/heckez/edition1/` | HeckeZ data set: structure of the Hecke ring over Z (components, maximal ideals, local rings, Eisenstein congruence module) for 141 cells | edition 1 | signed 2026-11-07 |
| `census/heckez/edition2/` | HeckeZ data set, edition 2: the 85 level-N `full` records recomputed with U_p over Z (ell = 2, 3); the other 141 records are edition 1's | edition 2 | signed 2026-11-09 |
| `census/heckez/edition3/` | HeckeZ data set, edition 3: the 85 level-N `full` records recomputed with the Eisenstein U_p systems over Z (204 final / 22 pending records, was 140 / 86); new column `U_p` in `components.tsv`; the other 141 records are edition 2's | edition 3 | signed 2026-11-11 |

Per-edition status, sign-off dates and the checksum comparison against the editions' own manifests: `EDITIONS.md`.
Schemas: `census/congruence/SCHEMA.md`, `census/integral/DATASET.md`, `census/heckez/DATASET.md` (+ `DATASET_v2.md` for edition 2, `DATASET_v3.md` for edition 3).
Release history: `CHANGELOG.md`.

**Not included** (not signed, or not yet decided): the section b2 and the section L23 (ell = 2, 3) of the congruence census 4.x, which are
pre-release addenda; earlier census editions (3.x), which 4.0 supersedes; the census explorer (browser application; separate decision).

**A note on the documents.** The README and manifest files inside each edition are byte-identical to the signed originals (their checksums
are part of the signed record), so some of them mention paths of the development repository (for example `docs/...`) and the development
history of the data. Those paths are not part of this repository; the data files and their manifests are self-contained.

## Verifying

```
sha256sum -c CHECKSUMS.sha256                                   # every file of this repository
(cd census/integral/edition1 && sha256sum -c MANIFEST.sha256)   # an edition's own manifest
(cd census/heckez/edition1   && sha256sum -c MANIFEST.sha256)
(cd census/heckez/edition2   && sha256sum -c MANIFEST.sha256)
(cd census/heckez/edition3   && sha256sum -c MANIFEST.sha256)
(cd census/congruence/4.0    && grep -E '^[0-9a-f]{64}  ' MANIFEST40.txt | awk '$2 !~ /b2/' | sha256sum -c)  # the 13 b2 files listed there are not shipped
```

## Licence and citation

All data and documentation: **CC-BY-4.0** (`LICENSES/CC-BY-4.0.txt`, mapping in `REUSE.toml`); reuse requires attribution.

Please cite these data using the metadata in `CITATION.cff`, together with the edition shown in the table's header. To
cite all versions, use the Zenodo concept DOI [10.5281/zenodo.24118877](https://doi.org/10.5281/zenodo.24118877), which
always resolves to the latest release. Each release also has its own version DOI on Zenodo (for example, 0.1.0 is
[10.5281/zenodo.24118878](https://doi.org/10.5281/zenodo.24118878)). The software that computed the data, HACS, is cited
with its concept DOI [10.5281/zenodo.24118841](https://doi.org/10.5281/zenodo.24118841).

## Acknowledgements

The data were computed with HACS, whose development was assisted by Anthropic's Claude AI models, which were used under
the author's direction for code, documentation and verification work; all computed results are validated by the
project's own test suite and, where stated, by formal Lean proofs.

Author: Fabian Januszewski, fabian.januszewski@math.uni-paderborn.de.
