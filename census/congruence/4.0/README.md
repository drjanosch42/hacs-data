<!-- SPDX-License-Identifier: CC-BY-4.0 -->
# `congruence_census/edition3/4.0`: census edition 4.0. **SIGNED OFF by the owner 2026-10-27 (route A): main section + section b1**

**Status (2026-10-27): census 4.0 FINAL, SIGNED OFF by the owner 2026-10-27 (route A) = the main section + section b1; section b2 (4.0-pre.3) is a PARTIAL pre-release addendum, NOT signed off**

**Edition 4.0 is the two files `rows40.tsv.gz` (main) and `rows40_b1.tsv.gz` (section b1)**: 4.0-pre.1's two files with the `edition` column relabelled
to `4.0`, nothing else changed (owner ruling 2026-10-27, "route A": 4.0 final = 3.5-pre.4 + C1K-a + section b1, signed now; b2 stays a partial
pre-release addendum, to be completed later). 23 005 cells, 448 978 rows, 46 primes (5 … 199 and 547, 691), even n ≤ 1000. Release report:
`docs/implementation/slot24/CENSUS40_release_report.md`.

* **Proof of the relabel** (`final40.txt`, `docs/implementation/slot24/census40/final40.py`, deterministic, as `../3.4/final34.txt` for 3.4):
  ```
  FINAL40 section=main base_rows=279093 rows=279093 same_key_same_position=279093 columns_identical=74 (all but edition) rows_differing_there=0 edition_old=4.0-pre.1 edition_4.0=279093
  FINAL40 section=main projection_sha256 base=5c5fef4c1c084e9d2a244133acedf6f5df83ad7d88efdae767bdb5a03236ba36 final=5c5fef4c1c084e9d2a244133acedf6f5df83ad7d88efdae767bdb5a03236ba36 equal=True
  FINAL40 section=main verdict=OK violations=0
  FINAL40 section=b1 base_rows=169885 rows=169885 same_key_same_position=169885 columns_identical=74 (all but edition) rows_differing_there=0 edition_old=4.0-pre.1 edition_4.0=169885
  FINAL40 section=b1 projection_sha256 base=fe0f7fe2046ad116f6b27a52068ce0e081689ff7404561efd5dd17decaea43f1 final=fe0f7fe2046ad116f6b27a52068ce0e081689ff7404561efd5dd17decaea43f1 equal=True
  FINAL40 section=b1 verdict=OK violations=0
  ```
* **Add-only / section / key gates re-run on the final files** (`addonly40_final.txt`, `merge40.py proof --label 4.0`; every record re-hashed):
  ```
  ADDONLY40 base=149961 unchanged_but_edition=149961 columns_compared=74 (all but edition) violations=0
  ADDONLY40 main=279093 new_rows=129132 new_cells=4050 c1ka_cells=4050 extra=0 not_covered=0 already_in_base=0 duplicated_keys=0
  SECTION40 b1 rows=169885 cells=9500 c1kb1_cells=9500 extra=0 not_covered=0 cells_also_in_main=0 primes_also_in_main=0
  SECTION40 edition_labels={'4.0': 448978}
  KEY40 section=a records=4050 sha256_ok=4050 keys=budget_mb=16384/43364ef4020ac327:3750;budget_mb=81920/61b71f1aeb290a17:300 u_ell_off_cells=580 expected=580 verdict=OK
  KEY40 section=b1 records=9500 sha256_ok=9500 keys=budget_mb=262144/951d3b518e6e289a:9500 u_ell_off_cells=0 expected=0 verdict=OK
  ADDONLY40 verdict=OK violations=0
  ```
  The overlap and cross-check files of 4.0-pre.1 (`overlap40.txt`, `overlap40_records.tsv`, `xcheck40.txt`, `gates40.txt`) hold unchanged: no value moved.
* **The `kummer_exact_agree` fix changes no row of main or b1** (`kummer40_final.txt`, `census40/kummer40.py check`: the fixed `np_extra` re-applied to
  every `residual_status = ambiguous` row from its record, no cell recomputed): main 3 ambiguous rows, b1 132, `changed=0`, `new_false=0`, `verdict=OK`.
* The 4.0-pre.1 bytes of the two files (label `4.0-pre.1`) are in git at `d6cc9f8e` and listed in `MANIFEST40.txt`. Edition 3.4's files (`../3.4/`) are
  untouched; 3.4 stays a signed edition.
* **Not contained:** ℓ = 2, 3 (the separate L23 section of 3.5, `../3.5/`); section b2 (below). The soft spots of 4.0-pre.1 stay as stated below:
  580 main cells without U_ℓ, 862 non-final C1K-a rows, no saturation / split columns on the C1K rows, the WS cell (912, 59) left out.

**Section b2: 4.0-pre.3 (2026-10-27), PARTIAL pre-release addendum, NOT signed off** — see "Section b2" below. Coverage ℓ = 211 … 347 complete,
349 … 419 partial (411 of 6000 cells), 421 … 997 not yet run; to be completed later.

---

*History (pre-releases). Status of 2026-10-26: pre-edition 4.0-pre.2, not signed off (= 4.0-pre.1's two sections, files unchanged, + section b2, PARTIAL (E1R
ingested 2026-10-26), below). 4.0-pre.1 (2026-10-24), not signed off: the owner signs editions.*
Content: census 3.5-pre.4 (itself unsigned; `../3.5/`) + the local runs **C1K-a** and **C1K-b1** (`docs/implementation/slot24/00-slot24-plan.md` §1;
reports `docs/implementation/slot24/C1K_a_report.md`, `C1K_b1_report.md`). Ingest report: `docs/implementation/slot24/C40_ingest_report.md`.

## Two table sections (owner ruling 2026-10-20: the two U_ℓ budget keys are never mixed in one table section)

| section | file | rows | cells | primes | n | U_ℓ key (`settings.u_ell` / fingerprint) |
|---|---|---:|---:|---|---|---|
| **main** | `rows40.tsv.gz` | **279 093** | **13 505** | the 27 primes of 3.4 (5 … 103, 547, 691) | even 0 … 1000 | the census-3.4 keys: `budget_mb=16384` / `43364ef4020ac327`; ℓ = 547, 691: `budget_mb=81920` / `61b71f1aeb290a17` |
| **b1** | `rows40_b1.tsv.gz` | **169 885** | **9 500** | the 19 new primes 107 … 199 | even 2 … 1000 | **one** key `budget_mb=262144` / `951d3b518e6e289a` |

* **main** = 3.5's 149 961 rows (`../3.5/rows35_satc.tsv.gz`, same order, every column but `edition` byte-identical) followed by the
  129 132 rows of the 4050 C1K-a cells (n = 702 … 1000, 150 per prime). Its 580 cells at ℓ = 73 … 103 (from n = 992 … 836), 547 (from 814) and 691
  (from 726) have **no U_ℓ** (`u_ell.included = false`, `u_ell_over_budget`, by design under the 3.4 keys; list `docs/cluster/local/c1k/cells/c1k_a_over_budget.tsv`);
  the rows file has no U_ℓ column, the record says it (the explorer reads ordinarity as "n/a" there).
* **b1** = the 169 885 rows of the 9500 C1K-b1 cells alone, in a file of its own with its own header (`section=b1 u_ell_key=… fingerprint=951d3b518e6e289a`).
  Every b1 cell keeps U_ℓ. No cell and no prime of b1 is in the main file.
* Both: 75 columns as 3.5 (`../SCHEMA.md`, incl. column 75 `sat_kmax`); `edition` reads `4.0-pre.1` on every row; the C1K rows have split columns
  `not_computed`, `saturated` / `mult_one` `not_computed`, `sat_defect_*` and `sat_kmax` `not_computed:no_log` (the runs made no saturation pass, `sat_pass=0`).
  The v2 columns were built by `census_dataset_v2.py` with edition 3's side inputs (WS jsonl, W1 blocks, depth window, `ext_sturm_l5`), less one WS file (below).
* Side tables of 3.5 (off-grid sat cells, ring-Loewy / Hida probes, the L23 section, DEEP-1) stay in `../3.5/` and are not repeated here.

## Section b2, 4.0-pre.3 (2026-10-27): a PARTIAL pre-release addendum, not part of the signed edition 4.0

`rows40_b2.tsv.gz` is now **4.0-pre.3**: 4.0-pre.2's b2 rows rebuilt (`build40.sh b2 8`, `run40b2.sh` with `C40ROOT` / `B2_LABEL=4.0-pre.3` / `B2_PREV`)
with the **`kummer_exact_agree` fix** (`np_extra.py`: on an ambiguous classification the expected Kummer set is taken per candidate ρ̄ after the
ordinary filter, `undetermined` when the candidates disagree; `slot24/C1K_b2_kummer311.md`; schema column 62). Same records, same cells, same coverage
as 4.0-pre.2 (below); nothing recomputed. Proof (`kummer40_b2.txt`; also appended to `section40_b2.txt`):

```
KUMMER40_DIFF rows=152760 same_order=True other_columns_differing=0 kummer_changed=6 transitions=false->true:6 edition=4.0-pre.2->4.0-pre.3 verdict=OK
KUMMER40 section=b2 rows=152760 ambiguous=543 reevaluated=543 no_record=0 changed=0 now=true:543 old_false=0 new_false=0 undetermined=0 verdict=OK
SECTION40B2 edition_labels={'4.0-pre.3': 152760}
KEY40B2 section=b2 records=11911 sha256_ok=11911 keys=budget_mb=262144/951d3b518e6e289a:11911 commit=9939d33c u_ell_off_cells=0 expected=0 verdict=OK
OVERLAP40 comparisons=8 cells=4 identical=8 differing=0 verdict=OK
ED3_ANALYSE rows=152760 new_rows=152760 loewy_ge3=0 R_violations=0 KF=0 kummer_false=0 cf9=none
```

* The six ℓ = 311 rows (n = 290, 328, 600, 638, 910, 948) read `true` now (were `false`): ℓ = 311 is the first prime at which 2, 3, 5, 7 are all
  quadratic residues, so the trace primes cannot tell ρ̄ from its twist by (·/311); a_311 = 1 (ordinary) leaves only the Eisenstein candidate,
  expected {292} = predicted. All 543 ambiguous b2 rows read `true`; no row reads `false` or `undetermined`. Main and b1: no change (`kummer40_final.txt`).
* The overlap check now finds 8 other records of b2 cells (4 before): the 4 anchor re-runs and 4 records of (349, 236) from a later kernel probe
  (`s24/avx2k/`, commit `d6cc9f8e`; portable / AVX2 / AVX-512 builds): all `SAME`.
* Gates of the cluster runs, coverage, missing cells and findings: unchanged from 4.0-pre.2 (`gates40_b2.txt`, `coverage40_b2.tsv`, `missing40_b2.txt`;
  `findings40_b2.txt` E3-3 now reads `not_applicable=79853 true=72907`). Manifest: `MANIFEST40_b2.txt`.
* **Coverage still PARTIAL**: ℓ = 211 … 347 complete, 349 … 419 411 of 6000 cells, 421 … 997 not yet run (48 089 cells left). Every completing
  run builds the record commit **`9939d33c`** (the fix is in the post-processing, not in the binary). The next blind primes are 479, 719, 839
  (`mk_c1k.py check` lists them; `kummer40.py check` gates their rows).

### Section b2 as of 4.0-pre.2 (2026-10-25; E1R added 2026-10-26): C1K-b2, PARTIAL (history; the label now reads 4.0-pre.3)

| section | file | rows | cells | primes | n | U_ℓ key | record commit |
|---|---|---:|---:|---|---|---|---|
| **b2** | `rows40_b2.tsv.gz` | **152 760** | **11 911** | 35 primes 211 … 419 (of b2's 120, 211 … 997) | even 2 … 1000 | `budget_mb=262144` / `951d3b518e6e289a` (as b1) | `9939d33c` |

* Source: the C1K-b2 cluster runs of 2026-10-24 (`docs/implementation/slot24/C1K_b2_return_report.md`): **O1** (Otus 3847347) complete,
  **E1** / **E2** (Noctua 2 34745070 / 34745072) stopped at their 8 h limit; **E1R** (Otus 3923088, 2026-10-25, the E1 remainder) is complete (5599 of 5599 cells). Only the cells `c1k_gate_cluster.py` accepted (the `cells.tsv` PASS rows)
  are here.
* **Coverage (PARTIAL):** ℓ = 211 … 269 (11 primes) **complete**, 500 cells each (5500); ℓ = 271 … 347 (12 primes) **6000 of 6000** cells (E1 401 + E1R 5599: **complete**);
  ℓ = 349 … 419 (12 primes) **411 of 6000** cells. Per prime: `coverage40_b2.tsv` (23 `complete`, 12 `partial`); missing cells: `missing40_b2.txt`
  (5589 = the remainder E2R). A statement "at ℓ" in `findings40_b2.txt` means "on the rows present".
* A section of its own: same key as b1, but another source and commit, and partial, so it is not merged into b1. No cell and no prime of b2 is in main
  or b1 (`section40_b2.txt`). `edition` reads `4.0-pre.2`; the C1K columns are as in b1 (split / saturation columns not computed).
* Gates and proofs (read by text):

```
C1K_GATE_CLUSTER run=oc_gl2_otus_c1kb2_o1_3847347 planned=5500 accepted=5500 remainder=0 fail=0 wrong_key=0 u_ell_off=0 refused_or_fallback_or_error_logs=0 verdict=OK
C1K_GATE_CLUSTER run=oc_gl2_noctua2_c1kb2_e1_34745070 planned=6000 accepted=401 remainder=5599 fail=0 wrong_key=0 u_ell_off=0 refused_or_fallback_or_error_logs=0 verdict=OK
C1K_GATE_CLUSTER run=oc_gl2_noctua2_c1kb2_e2_34745072 planned=6000 accepted=411 remainder=5589 fail=0 wrong_key=0 u_ell_off=0 refused_or_fallback_or_error_logs=0 verdict=OK
C1K_GATE_CLUSTER run=oc_gl2_otus_c1kb2_e1r_3923088 planned=5599 accepted=5599 remainder=0 fail=0 wrong_key=0 u_ell_off=0 refused_or_fallback_or_error_logs=0 verdict=OK
SECTION40B2 b2 rows=152760 cells=11911 accepted_cells=11911 extra=0 not_covered=0 duplicated_keys=0 cells_also_in_main_or_b1=0 primes_also_in_main_or_b1=0
SECTION40B2 edition_labels={'4.0-pre.2': 152760}
KEY40B2 section=b2 records=11911 sha256_ok=11911 keys=budget_mb=262144/951d3b518e6e289a:11911 commit=9939d33c u_ell_off_cells=0 expected=0 verdict=OK
SECTION40B2 verdict=OK violations=0
OVERLAP40 comparisons=4 cells=4 identical=4 differing=0 verdict=OK
WSJOIN40 section=b2 ws_cells_joined=0 rows_joined=0 D_equal=0 A1_eq_hilbert=0 ws_blocks_unjoined=0 (census_dataset_v2.py STOPs on any D mismatch)
ED3_COVERAGE rows=152760 cells=11911 ells=35 missing_c1_cells=5589
ED3_ANALYSE rows=152760 new_rows=152760 loewy_ge3=0 R_violations=0 KF=0 kummer_false=6 cf9=none
```

  `kummer_false=6` is new (E1R): six rows of ℓ = 311 (n = 290, 328, 600, 638, 910, 948; tag `eisenstein_cuspidal:k=292`), see the return report §9. The gate of E1R was run on a view whose `build/verdict.txt` is cut to the last build block (`run40b2.sh`).
  The four other records of b2 cells are the box re-runs of the return report's anchor (§5): all `SAME` (`overlap40_b2_records.tsv`).
  No WS block joins a b2 cell. The main section's cross-checks (`xcheck40.py`: O1R side table, WS (912, 59)) do not apply to b2.
* Files: `rows40_b2.tsv.gz`, `gates40_b2.txt`, `section40_b2.txt`, `overlap40_b2.txt`, `overlap40_b2_records.tsv`, `coverage40_b2.tsv`,
  `missing40_b2.txt`, `findings40_b2.txt`, `category_table40_b2.txt`, `kummer_frobenius40_b2.tsv`, `inputs_c1kb2.sha256`, `MANIFEST40_b2.txt`
  (`MANIFEST40.txt` of pre.1 is unchanged). Scripts: `docs/implementation/slot24/census40/` (`stage40.sh b2`, `build40.sh b2 8`, `run40b2.sh`).

## Gates, proofs, cross-checks (read by text)

```
C1K_GATE stage=a cells=4050 records=4050 keys=2 wrong_key=0 u_ell_off=580 refused_or_fallback_or_error_logs=0 gpu_requeued=0 verdict=OK
C1K_GATE stage=b1 cells=9500 records=9500 keys=1 wrong_key=0 u_ell_off=0 refused_or_fallback_or_error_logs=0 gpu_requeued=1 verdict=OK
ADDONLY40 base=149961 unchanged_but_edition=149961 columns_compared=74 (all but edition) violations=0
ADDONLY40 main=279093 new_rows=129132 new_cells=4050 c1ka_cells=4050 extra=0 not_covered=0 already_in_base=0 duplicated_keys=0
SECTION40 b1 rows=169885 cells=9500 c1kb1_cells=9500 extra=0 not_covered=0 cells_also_in_main=0 primes_also_in_main=0
KEY40 section=a records=4050 sha256_ok=4050 keys=budget_mb=16384/43364ef4020ac327:3750;budget_mb=81920/61b71f1aeb290a17:300 u_ell_off_cells=580 expected=580 verdict=OK
KEY40 section=b1 records=9500 sha256_ok=9500 keys=budget_mb=262144/951d3b518e6e289a:9500 u_ell_off_cells=0 expected=0 verdict=OK
ADDONLY40 verdict=OK violations=0
OVERLAP40 comparisons=76 cells=34 identical=76 differing=0 verdict=OK
WSJOIN40 section=a ws_cells_joined=9 rows_joined=56 D_equal=56 A1_eq_hilbert=56 ws_blocks_unjoined=0
WSJOIN40 section=b1 ws_cells_joined=283 rows_joined=741 D_equal=741 A1_eq_hilbert=741 ws_blocks_unjoined=0
XCHECK40 cells=7 disagree=0 verdict=OK
```

* **Overlap with the census rows: none.** 3.5's rows stop at n = 700 and hold no b1 prime, so no C1K cell has a row in 3.5 (`already_in_base=0`); no
  held value is touched. The cells that *were* computed before are checked instead, all agreeing:
  * `overlap40.txt` / `overlap40_records.tsv`: every other `hecke_ext` record of a C1K cell under a scratch directory (C1K-0 calibration, G6, GL-2 T2, P4R, P7, the
    GPU/CPU spot checks): 76 comparisons on 34 cells, JSON-equal with `commit` / U_ℓ settings masked; 4 of them (ℓ = 103, n = 1000) `SAME_PLUS_UELL`
    (the reference ran the 262144 key and has U_103, the C1K-a record is over budget: equal but for the additive U_ℓ entries).
  * WS joins of the v2 build (slot-14 weight-shift runs at the same cells): 9 cells / 56 rows in main, 283 cells / 741 rows in b1, block D = `module_D` on
    all (the builder STOPs otherwise) and block A1 Hilbert = row `hilbert` on all.
  * `xcheck40.txt`: 3.5's O1R side table (ℓ = 7, n = 756 … 778, off-grid in 3.5, on the grid now): 6 / 6 cells agree (block count = H1 rows; stable block's
    (D, A1 Hilbert) = one non-reduced row's (`module_D`, `hilbert`)); and the WS cell (912, 59) below.
* **WS file left out**: `s14/wpc14/i3b/ws_912_59.jsonl` is an ordinary-only probe (`WS_ONLY_ORDINARY=1`, 1 block of the cell's 54 H1 rows); the builder's
  one-block-per-row join STOPs on it. Its one block agrees with the row (ideal 28: system, D = 3, Hilbert [1,1]); `hidden_congruence` stays `not_computed` at (912, 59).

## New non-final rows (data, not violations)

862 of the C1K-a rows read `final = false` (`certificate = unavailable`) in 188 cells; b1 has none. By `final_reason`:
`closure_unreachable` 696 rows in 22 cells (ℓ = 83 … 103; **all 22 are U_ℓ-over-budget cells**: without U_ℓ the generator closure is not reached;
a value new in 4.0), `sturm_heuristic` 136 rows / cells (ℓ = 17 … 31), `certificate_failed:sturm` 30 rows / cells (ℓ = 37: 28, 547: 2).
3.5's 21 non-final rows are unchanged.

## Files

| file | holds |
|---|---|
| `rows40.tsv.gz`, `rows40_b1.tsv.gz` | the two sections of edition 4.0 (final, `edition` = 4.0; gzip -n -9) |
| `MANIFEST40.txt` | rows / cells / sha256 (gz, tsv, data rows) per section (final, plus the 4.0-pre.1 hashes), base and binary sha256, keys, sha256 of every file here |
| `final40.txt`, `addonly40_final.txt`, `kummer40_final.txt` | the relabel proof, the add-only / section / key gates on the final files, the `kummer_exact_agree` re-check of main and b1 (2026-10-27) |
| `kummer40_b2.txt` | section b2 4.0-pre.3: the diff against 4.0-pre.2 (only column 62 on six ℓ = 311 rows, and `edition`) and the re-check |
| `gates40.txt`, `addonly40.txt`, `overlap40.txt`, `overlap40_records.tsv`, `xcheck40.txt` | the lines above in full |
| `inputs_c1ka.sha256`, `inputs_c1kb1.sha256` | sha256 of the base rows file, the binary, `cells.tsv`, `heartbeat.log` and every record of the run |
| `coverage40.tsv`, `missing40.txt`, `findings40.txt`, `category_table40.txt`, `kummer_frobenius40.tsv` | slot-15 analysis tools over **main** (grid even n 2 … 1000: `ed3_coverage.py --c1l 5,7,11,13,691,547 --nmax 1000`); all 27 primes complete, `MISSING_TOTAL cells=0 of 13500` |
| `*40_b1.*` | the same over **b1** (`--c1l 107,…,199 --nmax 1000 --no-c1ab`); 19 primes complete, 0 missing. The tools' header texts are edition 3's boilerplate |

Binary of both runs: `s24/bin_c1k2/build/cohomology_app` (sha256 `cb517186…`, record commit `f22b64cc`). Reproduce (deterministic: two runs byte-identical):
`docs/implementation/slot24/census40/stage40.sh a|b1`, `build40.sh a 12`, `build40.sh b1 14` (~5 min, ≤ 1.4 GB per shard), then `run40.sh` (~4 min).
Scratch: `<scratch>/s24/census40/` (copy of the two rows files and the manifest in `out/`).
