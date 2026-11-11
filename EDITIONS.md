<!-- SPDX-FileCopyrightText: 2024-2026 Fabian Januszewski -->
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
# Editions in this repository

| Data set | Edition | Directory | Signed | Files shipped | Notes |
|---|---|---|---|---|---|
| Congruence census | **4.0** (main + section b1) | `census/congruence/4.0/` | 2026-10-27 | `rows40.tsv.gz` (279 093 rows, 13 505 cells), `rows40_b1.tsv.gz` (169 885 rows, 9 500 cells) and their gate / coverage / findings files | 75 columns (`census/congruence/SCHEMA.md`); the two sections are never mixed (different U_ell budget keys) |
| Integral census | **edition 1** | `census/integral/edition1/` | 2026-11-03 | 10 files, 3.1 MB | grids A + B 1290 / 1290 cells ok, boundary-pair check 210 / 210 |
| HeckeZ data set | **edition 1** | `census/heckez/edition1/` | 2026-11-07 | 22 files incl. the 226 record files (`records_bundle.tar.gz`) | 122 final / 104 pending records, 4 cells not computed (`not_computed.tsv`) |
| HeckeZ data set | **edition 2** | `census/heckez/edition2/` | 2026-11-09 | 23 files (all but `MANIFEST.sha256` itself hashed in `MANIFEST.sha256`; manifest corrected 2026-11-09, data bytes unchanged) | same 141 cells / 226 records; 85 level-N `full` records recomputed; edition 1 unchanged and still verifies |
| HeckeZ data set | **edition 3** | `census/heckez/edition3/` | 2026-11-11 | 23 files (all but `MANIFEST.sha256` itself hashed in `MANIFEST.sha256`) | same 141 cells / 226 records; the 85 level-N `full` records recomputed with the Eisenstein U_p systems over Z: 204 final / 22 pending (edition 2: 140 / 86); schema `integral_hecke_dataset/v1+D1` (new column `U_p` of `components.tsv`, `census/heckez/DATASET_v3.md`); editions 1 and 2 unchanged and still verify |

## Not shipped (pre-release addenda and unsigned material)

* Congruence census 4.0, **section b2** (`rows40_b2.tsv.gz`, 4.0-pre.3): a partial pre-release addendum (11 911 of 60 000 planned cells), not signed.
* Congruence census 4.0, **section L23** (ell = 2, 3, n <= 700, plus the ell = 2 deep tables): a pre-release addendum, not signed.
* Earlier congruence editions (3.x), superseded by 4.0.

## Checksum record

Each edition carries its own manifest of file hashes (`census/integral/edition1/MANIFEST.sha256`, `census/heckez/edition<k>/MANIFEST.sha256` for k = 1, 2, 3,
`census/congruence/4.0/MANIFEST40.txt`); every shipped data file equals the hash recorded there (the manifest of 4.0 also lists the
section-b2 files, which are not shipped). `CHECKSUMS.sha256` lists every file of this repository (`sha256sum -c CHECKSUMS.sha256`).

## This branch (`scrubbed-paths`)

The signed text files below carried absolute paths of the machine the data was produced on (and a user name). On this branch those
paths are replaced by `<scratch>/` and the edition manifests that list the changed files are updated; **no data file changed**
(every `.tsv`, `.gz`, `.tar.gz` is byte-identical to the signed edition). `census/SCRUB_MAP.tsv` gives the signed and the scrubbed hash of each
changed file, so the change is reversible and auditable. The `main` branch is byte-identical to the signed files.
(The same three files of `census/heckez/edition2/` are scrubbed alike; in `census/heckez/edition3/` the two files `manifest.json` and `provenance_verify.json`, its `README.md` carries no such path. `census/heckez/edition1/manifest.json` also embeds the hashes of that edition's `README.md` and `provenance_verify.json`; those two values are updated to the scrubbed files' hashes.)
