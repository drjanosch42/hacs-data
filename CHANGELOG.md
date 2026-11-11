<!-- SPDX-FileCopyrightText: 2024-2026 Fabian Januszewski -->
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
# Changelog

Releases of this repository. The data sets are versioned by edition (`EDITIONS.md`); a release adds editions and never changes a
published one.

## [0.2.0] - 2026-11-11

- **HeckeZ data set, edition 3** (`census/heckez/edition3/`, schema `census/heckez/DATASET_v3.md`): the same 141 cells and 226 records
  as editions 1 and 2; the 85 level-N `full` records recomputed with the Eisenstein U_p systems over Z. 204 final / 22 pending records
  (edition 2: 140 / 86); no record is pending on `blocked(U_p)` any more. New column `U_p` of `components.tsv` (schema
  `integral_hecke_dataset/v1+D1`): the U_p eigenvalue of each Eisenstein component for every prime p dividing the level.
- Editions 1 and 2 and all other data sets unchanged (byte for byte).
- `CITATION.cff`: version 0.2.0; `README.md`, `EDITIONS.md`: edition 3; this changelog.

## [0.1.1] - 2026-11-10

- Zenodo DOIs and citation metadata; no change of data.

## [0.1.0] - 2026-11-10

- First release: congruence census 4.0 (main + section b1), integral census edition 1, HeckeZ data set editions 1 and 2.
