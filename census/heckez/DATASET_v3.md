<!-- SPDX-FileCopyrightText: 2024-2026 Fabian Januszewski -->
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
# The integral Hecke structure data set, edition 3

Edition 3 (`results/hecke_ed3/`) is `integral_hecke_dataset/v1` with one amendment, **D1**: the table `components.tsv` gains the column `U_p`.
Read `hecke_dataset_v1.md` (what a cell, a record, a status and each table mean) and `hecke_dataset_v2.md` (the words of a recomputed `full`
record): both stay true here. This note says only what edition 3 changes. Because a column is added, the schema string of edition 3 is
`integral_hecke_dataset/v1+D1` (head line of every table and `manifest.json`); editions 1 and 2 keep `integral_hecke_dataset/v1`.

## 1. What edition 3 is

Edition 2 carried 67 `full` records at level $N > 1$ that were `pending` with the word `blocked(U_p)`: their Eisenstein $U_p$ systems over
$\mathbb Z$ were not yet built. Package HZ-EISUP of slot 26 builds them ([C] 1.27.0, Proposition `prop:heckeZeisup`, item 8 (d′)(ii),
paragraph *The components and the maximal ideals from HZ-EISUP on*), and its re-run recomputed all 226 records of edition 2 with the frozen
binary `1988501f938eacb05ece3848f396913b92fd67a3d8076163fa64226a6c977a2e` (source `6cc94503`, branch `s26-wip-hzeisup`; report
`docs/implementation/slot26/HZEISUP_rerun.md`).

* The **141 level-1 and `anemic` records are carried byte for byte** from edition 2 (= edition 1's bytes), with their lanes and their pass
  column (1 or 2). The re-run recomputed them too, with an unchanged record hash for each (`HZUP_RECOMPUTE ... nonfull=141 changed=0`); the
  edition keeps the bytes already signed rather than re-wrapped copies whose only difference is the run's `seconds` / `rss_mib`.
* The **85 `full` records at $N > 1$** are the re-run's, in lane `p4` of the bundle, pass column **4**. They replace edition 2's pass-3
  records under the same record ids; no pass-3 record is left, so `provenance_pass3.tsv` is not shipped and `provenance_pass4.tsv` is.
* The **settings fingerprint is unchanged** (`0x8ffc29730c4f9672`); the descent bundle is edition 2's, byte for byte (no descent cell is added).
* Editions 1 and 2 are not modified and keep verifying.

Result: **204 final / 22 pending / 0 refused** (edition 2: 140 / 86 / 0). 82 of the 85 recomputed records are `final`; 64 of them were
`pending` in edition 2. The 3 still pending are $(167, 0)$, $(173, 0)$, $(197, 0)$ `full`, through `refused(export_word)` only. No record carries
`blocked(U_p)` any more; the 22 pending records carry `refused(export_word)` (20) and `refused(k1_route)` (2).

## 2. Amendment D1: the column `U_p` of `components.tsv`

HZ-EISUP writes, for each Eisenstein component $j$ of a recomputed `full` record, the eigenvalue of $U_p$ on that component for every prime
$p \mid N$, as the hashed key `G1.components[j].U_<p>` (amendment A4 of the record schema; [C] item 8 (d′)(ii)). The new last column of
`components.tsv` lists them:

```
U_p = "<p>:<value>;<p>:<value>;..."   p ascending, value exactly as in the record (canonical signed hex); "-" if the component has no U_<p> key
```

So a level-1 component, an `anemic` component and a cuspidal component read `-`; an Eisenstein component of a `full` record at $N > 1$ lists
one entry per prime divisor of $N$. A value longer than 4096 characters would go to `big.tsv.gz` under the path `G1.components[j].U_p`
(none does: the longest value is 12 characters). Edition 3 has 102 such keys on 67 records. Every other column of every table is as in
edition 2 (the column lists of `hecke_dataset_v1.md` §3); the independent verifier re-derives the new column from the records.

This is the deferral of `HZEISUP_report.md` §4 deviation (6): the dataset tools already read `components_through.<x>` and `pair_kinds[<x>]`
generically; the `U_p` column is the one header change, made in this edition.

## 3. `G10.contract` on the pass-4 records

The 85 pass-4 records carry `G10.contract = gl2_contract 1.27.0-pre.1, item 8 (d')`. This is correct: [C] item 8 (d′)(vii) fixes the value
per binary, as the version whose words that binary writes, and the HZ-EISUP binary writes the words of 1.27.0-pre.1 (the value was fixed
before any record was written, design M3). The owner signed [C] 1.27.0 on 2026-11-11 as the text of 1.27.0-pre.1 unchanged, so the words
these records follow are those of the signed 1.27.0. `G10` is provenance: it is in neither `record_fnv1a64` nor `cert_sha256`. The 141
carried records carry no `G10.contract` key (written before the key existed), as in editions 1 and 2.

## 4. What moved between editions, and the rule that admits it

Two gates, both on the 85 records of the edition's lane `p4` (extracted from the bundle), both by rule on the pair (old value, new value):

* against edition 1, `hz_up_diff.py` (the `HZUP_DIFF` rows of `hecke_dataset_v2.md` §3): `HZUP_DIFF full=85 moved_outside_rule=0 removed=0 verdict=OK`;
* against edition 2, `hz_up_diff.py --eisup` (the tight mode of `HZEISUP_design.md` §5.3: `G0.status` pending → final only with no pending
  path left; `G1.components` `blocked(U_p)` → `certified`; `G2.ell=*.maximal_ideals` `blocked(U_p)` → `certified` | `not_run`; new paths only by
  key list; nothing removed; a record without a moved path keeps its `record_fnv1a64`), and the closed form of `prop:heckeZeisup` (5):
  `HZEISUP_DIFF records=85 moved=67 equal=18 ... verdict=OK`, `HECKEZ_EISUP_RUN records=67 in_scope=67 systems=93 mismatched=0 ... verdict=OK`.

Pins: `(2, 6, full)` moves again, `0x980a4da95386391f` (pending) → `0xaa516b325e24eed5` (final) (`slot26/golden_ledger.md`, row HZ-EISUP;
`HeckeZRecordExpected.inc`); `(8, 10, full)` stays `0xe15555ccce92ed3e`; the other 18 pins are edition 1's. The level-1 join to census 4.0 is
unchanged (no level-1 record moved).

## 5. Provenance of pass 4 (`provenance_pass4.tsv`)

One row per pass-4 record: id, key, binary sha256, source commit, seconds, peak RSS, cell, the two `EFFECTIVE` lines of its log, and — new in
this edition (Z4 tool audit S1) — the record's `record_fnv1a64` and the sha256 of its file. The sha256 of `provenance_pass4.tsv` is pinned in
`hz_dataset_verify.py` (`OVERLAY["hecke_ed3"]`), so the 85 record files are anchored outside the edition.

## 6. What is still missing, and why

`not_computed.tsv`: the same 4 cells / 5 records as edition 2.

* $(2089, 0)$ **anemic**: the pass-3 run on the Z9 binary `21a00a01` finished on 2026-11-09 after 23.8 h (final), after edition 2 was built.
  It is not in edition 3 because no later binary has re-accepted it and one overlay holds the records of one binary; the row's reason is
  restated in this edition. A later edition adds it after re-acceptance on the current binary.
* $(2089, 0)$ **full**: not computed (predicted peak RSS about 50 GiB).
* $(1, 484)$, $(1, 700)$, $(1, 702)$: deferred before the run.

The columns `kind`, `lane`, `pred_wall_s` of the two $(2089, 0)$ rows are edition 1's (Z4 tool audit S2): `pred_wall_s` is the pass-2
single-thread prediction, not a current one.
