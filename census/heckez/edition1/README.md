<!-- SPDX-FileCopyrightText: 2024-2026 Fabian Januszewski -->
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
# The integral Hecke structure data set, edition 1 (`integral_hecke_dataset/v1`, `hecke_ed1`)

**What this is.** For 141 cells $(N, n)$ — 56 at level 1 with $2 \le n \le 268$, 85 at levels $2 \le N \le 883$ (57 of them in weight 2, the others with $n \le 12$) — the structure
of the Hecke ring $\mathbb T$ over $\mathbb Z$ acting on the free part of $H^1(\Gamma_0(N);\mathrm{Sym}^n\mathbb Z^2)$: its components over
$\mathbb Q$, its maximal ideals and local rings at the primes where it is not étale or where congruences live, the Eisenstein congruence
module, and the place of the torsion classes relative to the free part. Each statement is qualified by a status (proved, bounded, or waiting
for a named piece of work). The schema and the meaning of every column are in `docs/integral_census/hecke_dataset_v1.md`; read that first.

## Contents of this edition

| | |
|---|---|
| records | **226** (56 level-1 `full`; 85 level-$N$ cells × {`anemic`, `full`}), settings fingerprint `0x8ffc29730c4f9672` |
| final / pending / refused | **122 / 104 / 0** — every pending record waits for a named package: `blocked(U_p)` (85 records, the $U_p$ over $\mathbb Z$, UP-Z), `refused(export_word)` (17, EXPORT-Z step 2), `refused(k1_route)` (2; the $K = 1$ route at level 1 for $\ell \in [2^{31.5}, 2^{63})$ — **not yet assigned** to a package: [C] `sec:recordschema` item 8 (c) names "EXPORT-Z or a slot-26 $K=1$ route", and EXPORT-Z as designed does not include it) |
| golden-eligible | 122 (the 15 level-1 cells of the first golden event among them, at their pinned hashes) |
| local rings | 4272 maximal ideals: 3690 étale, 582 monogenic, all Gorenstein |
| level-1 join to census 4.0 | 140 `equal`, 60 `census_extra(torsion_q1)` (each explained by $\ell$-torsion of integral $H^1$), 0 mismatches |
| descent inputs | 106 cells by reference into the integral census ed.1, 35 regenerated and bundled here |
| not computed | 4 cells / 5 records, `not_computed.tsv` (below) |

**Not computed.** The planned set had 145 cells / 231 records. Absent from this edition, with reasons:

* $(2089, 0)$, both algebras — stopped on the owner's instruction after 1.6 h of a predicted ≈ 635 h (the single-threaded anemic route);
  it returns when that route is parallelised or a longer cap is approved;
* $(1, 484)$, $(1, 700)$, $(1, 702)$ — deferred before the run: the rational step would need 117 000 – 354 000 word primes, far beyond the
  current code.

They will form a later edition; no row of this edition changes then.

## Files

`rows.tsv` (one line per record), `components.tsv`, `badset.tsv`, `maxideals.tsv`, `pairs.tsv`, `congruence.tsv`, `torsion_points.tsv`,
`hc.tsv`, `certs.tsv`, `join_modl.tsv`, `not_computed.tsv`, `cells.tsv`, `records_bundle.tar.gz` (the 226 record files byte for byte),
`descent_cells.tsv` + `descent_bundle.tar.gz` (the 35 regenerated descent cells), `big.tsv.gz` (7 minimal polynomials longer than 4096
characters), the run's own provenance byte for byte (`provenance_driver_log.txt`, `provenance_verify.json`, `provenance_defer_pass2.tsv`),
`manifest.json`, `MANIFEST.sha256`. Everything is in this directory; there are no external files.

## How to check it

```
python3 docs/integral_census/mk_hecke_dataset.py verify docs/integral_census/results/hecke_ed1
python3 docs/integral_census/hz_dataset_verify.py docs/integral_census/results/hecke_ed1 \
        [--run <HZ-RUN work dir>] [--binary <cohomology_app sha256 54ab8480…> --scratch <scratch>]
python3 docs/integral_census/hz_dataset_verify.py docs/integral_census/results/hecke_ed1 --selftest <scratch>
```

The second prints one line `HZDS_VERIFY … bad=0 … verdict=OK`. It re-hashes every record independently, binds each record file's name to
its content, re-derives **every row and column of every table** from the records, checks the not-computed list against the run's own
files and the `pass` column against the run's driver log, and recomputes the census join. The third runs its negative controls (a changed
table value, a dropped record, two swapped record files, a computed record moved to "not computed", a relabelled pass): each must fail.

## How it was made

Records: HZ-RUN passes 1–2 (slot 25, 2026-11-06/07), schema tag `heckez-schema-v1-frozen` with amendment A1, two binaries:

| pass | records | binary sha256 | source pin |
|---|---|---|---|
| 1 (2026-11-06) | **198** | `1d4f5a29e98c71f4713b5b025478cf5475f7704ecd54270fce1263810fcf24e3` | `30c47739` |
| 2 (2026-11-06/07) | **28** (the 7 cells pass 1 stopped at its RSS cap, both algebras; lane s4, 14 level-1 cells) | `54ab8480772f2ab9fa485f6731264b0f2a01d5769608d55efb8ee3b4b2b6a34d` | `15de418f` |

Which binary wrote which record is in `cells.tsv` / `rows.tsv` (`pass`, `binary_sha256`, `source_pin`), derived from the run's driver log.
Pass 2 scheduled only the 22 unfinished cells and left the 198 pass-1 record files untouched (byte-identical to the sha256 list taken
after pass 1); its verify step re-accepted all 226 with the pass-2 binary — that re-hashes each file, it does not recompute it. Evidence that the two binaries agree: 6 pass-1 records recomputed from scratch with the
pass-2 binary in the dataset audit ((157,0) and (197,0), both algebras; (1,36), (1,88)) are identical to the pass-1 files (record hash,
certificate hash, the whole hashed body and G9). The other 192 pass-1 records have not been recomputed with the pass-2 binary. The 35
descent cells were regenerated with the pass-2 binary.
Built by `mk_hecke_dataset.py build` (deterministic). Reports: `docs/implementation/slot25/HZRUN_pass2_report.md`, `HZDS_report.md`.

## Appendix. History

* 2026-11-07 — edition 1 built (HZ-DS, slot 25).
* 2026-11-07 — fix round 1 after the dataset audit (`docs/implementation/slot25/HZDS_audit.md`): per-record binary columns and the
  provenance copies added, the verifier extended; no record and no value changed. Pending the owner's sign-off.
