<!-- SPDX-FileCopyrightText: 2024-2026 Fabian Januszewski -->
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
# `integral_census_dataset/v1` -- the integral census data set (ICENSUS P5)

Schema of record of the integral census, edition 1 (the first dataset built from `integral_descent_cell/v1` files of the kit
`mk_icensus.py`). Builder: `docs/integral_census/mk_icensus_dataset.py` (`build`, `verify`, `selftest`). Status: **signed off by the owner 2026-11-03**
(the rulings of 2026-11-02 made the signature depend on the [C] 1.23.0 release and on the cross-check against census 4.0, gate P6; both were met).

## 1. What a cell is

A cell is a pair **(N, n)**: the group `Gamma_0(N)` and the local system `Sym^n` of the standard representation, with **integral**
coefficients (`coefficient_ring = Z`). The computed object is the integral cohomology `H^q(Gamma_0(N); Sym^n Z^2)` in the degrees
`q = 0, 1, 2, 3` (`q_max = 3`; for `q >= 2` the groups are periodic of period 2, `periodic_from = 2`), as a finitely generated abelian
group, with the Hecke operators `T_p` acting on its free quotient and on each `ell`-primary torsion part.

One row of `rows.tsv` is one cell; the key is `cell_key = N<N>_n<n>`. The parity of `n` is even throughout (odd `n` vanishes).

## 2. The mathematics behind each column group

**Free part.** `H^q = Z^{b_q} (+) torsion`. Columns `b0 b1 b2 b3`. On the free quotient of `H^1` the operator `T_p` has a characteristic
polynomial in `Z[x]` of degree `b_1` (`char_Z`), recorded for the *Hecke primes* `p` of the cell (`hecke_primes`, the primes `p` the record ran).

**Torsion.** `H^q (q >= 1)` has an `ell`-primary part `(+) Z/ell^{e_i}` for every prime `ell`. Written as the multiset of exponents `e_i`.
`tors_q1 tors_q2 tors_q3` read `ell:e*m.e*m;ell:...`, where `e*m` means `m` summands `Z/ell^e` (so `2:1*5.2*3` is `(Z/2)^5 (+) (Z/4)^3`).
`-` is "no torsion". The long table `torsion.tsv` has one line per `(N, n, q, ell)` with the exponent list, the number of summands and
`log_ell_order = sum e_i`, plus the route (`tree`, `cokernel`, ...) and the width `K` of the `Z/ell^K` computation that certified it.

**Torsion-module invariants (for the Tor terms).** For each `(q, ell)` and each Hecke prime `p`, `torsion_ops.tsv.gz` holds the
descent's invariant string of the `T_p`-module structure on the `ell`-torsion, stored verbatim (fields `a=` the exponents, then `P=`, `B=`,
`ker=`, `cok=`, `fit=`; their definition is [C] `thm:descentcert`, not repeated here). `module_certified` says that the
module structure was certified (`thm:descentcert`(3)), `commute` that the recorded `T_p` commute. These are exactly the pieces of
`Tor_1^Z(H^{q+1}, F_ell)` in the universal-coefficient sequence `0 -> H^q (x) F_ell -> H^q(.; F_ell) -> Tor_1(H^{q+1}, F_ell) -> 0`.
`ops_digest` in `rows.tsv` is the first 16 hex digits of the sha256 of all invariant strings of the cell.

**char_Z.** `charz.tsv.gz`: one line per `(N, n, p)`: the degree `r` (equals `b_1`; the builder stops if not), `digest` (the FNV-1a-64 digest
of the full polynomial, computed by the C++ record), the number of coefficients printed exactly (`exact_coeffs`), the maximum digit count,
and `coefficients`: the leading coefficients as exact integers, the others as `+31d` / `-31d` (sign and digit count: the record elides the
huge ones). The *full* coefficients are recomputable from the cell (section 4). `charz_digests` in `rows.tsv` lists `p:digest`; `charz_digest`
is a 16-hex digest of that list.

**The faithful coefficients (since the FMTMP format, [C] `sec:recordschema` item 7 (l)).** A cell written by a binary at or after FMTMP
carries every coefficient beyond 2^63 exactly, in the charpoly entry's list `big` (`{"i": index, "hex": "-0x1f..."}`: canonical signed
hexadecimal, Python `int(h, 16)`; reference decoder `icensus_exact.py`). The builder (tool v1.1) writes them to `charz_big.tsv.gz`, one line per
coefficient (`N, n, p, i, sign, bits, decimal_digits, hex`), and for every polynomial whose coefficients are all known (printed or in `big`)
recomputes the digest of `prop:crtcharpolyN` output rule (5) and stops if it differs from the record's (`manifest.json` `checks.charz_digest_recomputed`).
A cell written before FMTMP (the first edition-1 run, `ICENSUS_ed1_run.md`) has the tokens without `big`: `charz_big` has no line for it, and
`checks.charz_old_format_entries` counts its entries. The edition-1 dataset of record is built from the exact-format re-run ED1X
(`ICENSUS_ed1x_run.md`): every cell equal to the first run outside `big`, the record hash and timing / RSS; `charz_old_format_entries = 0`.
`charz.tsv` is unchanged (its `coefficients` keep the compact `+31d` form; the exact values live in `charz_big`).

**Boundary pair** (only the 210 cells of the check grid `pairchk12`, N <= 30, n <= 12): the relative cohomology of the pair
(bulk, boundary, compact support) in degrees 0..4. `pair_bulk_b / pair_boundary_b / pair_compact_b` are the free ranks `b_0..b_4`; the full groups
are in `pair.tsv`. Where a pair cell and a main-grid cell have the same `(N, n)`, the builder checks that groups, bad primes, char_Z digests and
torsion exponents agree (all 210 do) and merges them into one row.

**Bad primes.** `bad_primes = ell:count;...` copies the record's `bad_primes` list (the record's own notion, [C] `sec:recordschema`).

## 3. Status and refusal

`status` is the record's status (`ok`, or a refusal), `refusal` its text (`-` if none); a refused cell keeps its row with `na` in the data
columns. `charz_status` is `ok`, `none` (no Hecke primes), `not_run` (`parts = groups`), `partial` or `refused`. Edition 1 has 1290 rows, all `ok`,
none refused (the run gates of `ICENSUS_ed1_run.md` and of the exact-format re-run `ICENSUS_ed1x_run.md`: A+B `1290/1290`, pairchk12 `210/210`).

## 4. Coherence with the mod-ell census, and the lattice handle (the long-term design constraint)

The owner's long-term direction (rulings of 2026-11-02) is a compatible system of mod-`ell` Hecke modules built from the integral modules and
their reductions. The dataset therefore keeps two things recoverable.

* **Join key to the mod-ell census.** `rows.tsv` column `n` is the weight index shared with the mod-`ell` census (`k = n + 2`);
  `join_modl.tsv` has one line per `(N, n, ell)` for every `ell >= 5` that is a torsion prime or a bad prime of the cell, with `in_census40`
  = whether census 4.0 (main, b1, b2) has the cell `(ell, n)`, and the integral torsion exponents in `q = 1, 2, 3` at that `ell`. Census 4.0 is level 1:
  the join is meaningful for `N = 1`; for `N > 1` it records only the weight/prime coincidence. The census files used are pinned by sha256 in `manifest.json`.
* **Lattice handle.** `cells.tsv` gives for every cell file the run, the relative path, size, **sha256**, `record_fnv1a64`, the settings
  fingerprint and the id; `cells_bundle.tar.gz` contains the 1500 cell files byte for byte (members `work/...`, `work_pairchk12/...`),
  so every figure of the tables can be re-derived and the descent re-run for any cell from `manifest.json`'s provenance (binary sha256,
  kit commit, settings fingerprint). Nothing the run produced is discarded.

## 5. Files

| file | rows (ed. 1) | content |
|---|---|---|
| `rows.tsv` | 1290 | one row per cell: ranks, torsion, char_Z digests, module flags, pair summary, join, hashes |
| `torsion.tsv` | 9691 | one line per `(N, n, q, ell)` |
| `torsion_ops.tsv.gz` | 21264 | one line per `(N, n, q, ell, p)`: the module invariants |
| `charz.tsv.gz` | 3870 | one line per `(N, n, p)` |
| `charz_big.tsv.gz` | 467713 (ED1X; 0 for an old-format run) | (tool v1.1) one line per coefficient beyond 2^63, exact (canonical signed hex) |
| `pair.tsv` | 3150 | one line per `(N, n, part, q)` for the 210 pair cells |
| `cells.tsv` | 1500 | handle on every cell file (sha256) |
| `join_modl.tsv` | 1951 | join to the mod-ell census |
| `cells_bundle.tar.gz` | 1500 members | the cell files, byte for byte |
| `manifest.json`, `MANIFEST.sha256` | | provenance, counts, sha256 and size of every file |

**Out-of-tree files.** Built with `--ext-out DIR`, the two large files `cells_bundle.tar.gz` and `charz_big.tsv.gz` (together ~0.4 GB) are
written to DIR, not beside the tables; `manifest.json` records their sha256 and size under `external_files`, and `verify DIR --ext EXTDIR`
checks them. The edition-1 tables in git (`docs/integral_census/results/ed1/`) are built this way; where the external files are kept is
recorded in `ICENSUS_ed1x_run.md`.

Every `.tsv` starts with one `# schema=... table=... rows=...` line, then the column header. `builder verify DIR` re-checks all hashes.
Deterministic: no timestamps, sorted rows, gzip and tar headers zeroed; building twice gives identical bytes.
