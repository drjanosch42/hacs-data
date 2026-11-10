<!-- SPDX-FileCopyrightText: 2024-2026 Fabian Januszewski -->
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
# The integral Hecke structure data set, edition 2

Edition 2 (`results/hecke_ed2/`) is `integral_hecke_dataset/v1` with the same columns as edition 1. Read `hecke_dataset_v1.md` for what a cell,
a record, a status and each table mean: all of it stays true here. This note says only what edition 2 changes. The dataset schema string stays
`integral_hecke_dataset/v1`, because no column moves; the file `provenance_pass3.tsv` is the one addition.

## 1. What edition 2 is

Edition 1 carried 85 `full` records at level $N > 1$ whose $U_p$ data were waiting for a package, status word `blocked(U_p)`. Package Z1 of slot 26
serves those $U_p$ at $\ell \ge 5$, and the HZ-RUN pass 3 (package Z4) recomputed the 85 records with the binary of the merge of Z2 (`cf93a542`, source `c4a5fd65`; Z2 includes Z1 and Z9).

* The **141 level-1 and `anemic` records of edition 1 are carried byte for byte**, with their lanes and their pass column.
* The **85 recomputed `full` records** replace their edition-1 versions under the same record ids and sit in lane `p3` of the bundle.
* The **settings fingerprint is unchanged** (`0x8ffc29730c4f9672`).
* Edition 1 is not modified and keeps verifying on its own.

A record computed in pass 3 is named in `provenance_pass3.tsv`, with its binary, the source commit and the two `EFFECTIVE` lines of its log. The
pass column reads 3 for these records.

## 2. Reading the words of a recomputed `full` record ([C] item 8 (c) and (d′))

The records of this edition are written by the Z2 binary: `G10.contract` reads `gl2_contract 1.26.0-pre.9` or later on them, and the words are those of
[C] 1.26.0 item 8 (c) and (d'). Z2 fixed the words of `G3.C_T.lower` at $\ell \in \{2, 3\}$ (the $U_p$ read on the tree route), so **a `full` record at $N > 1$
is `final` or `pending` by its own words**: 18 of the 85 are `final` (9 `open(cap)`, 9 `refused(non_reduced)`), the other 67 stay `pending` with
`blocked(U_p)` until package HZ-EISUP writes the Eisenstein $U_p$ systems (3 of them also carry `refused(export_word)`).

**The derived `not_run`.** On any record, `maximal_ideals = not_run` beside a `generation.status` of the pending class is a *derived* word ([C] (d')(ii)):
it records that the decomposition was not attempted because generation is pending. It asserts nothing final, and it is rewritten when
`generation.status` moves. On the other hand, `not_run` beside a final `generation.status` (`open(sandwich)`, `open(cap)`, ...) is a final
statement. The word itself is of the final class in the status table, so `status_words` counts it among the final words; the pending
`generation.status` beside it is what makes the record `pending`.

The new final word `refused(non_reduced)` (amendment A2) is in the status table of the builder and the verifier.

## 3. What moved between editions, and the rule that admits it

Every move in the hashed body of the 85 records is admitted by rule on the pair (old value, new value). The gate is `HZUP_DIFF` (plan v0.7, Z4 card;
tool `hz_up_diff.py`): a path passes if it is new, or its edition-1 value was a pending word, or it is `G0.status`, or the move satisfies one
of five rows (`G0.operators` gaining `T_ℓ` with `t_ell = added`; `G1.S_cand.ell=*.reason` gaining a discriminant reason; the `v_disc` that goes with
it; `G2.ell=*.maximal_ideals` as above; `G3.C_T.status` with `G3.C_T.lower`, from the first Z2 record on). A removed path is a failure.

The result: `HZUP_DIFF full=85 moved_outside_rule=0 removed=0 verdict=OK` (`slot26/Z4_report.md`). The two pins that moved are `(2, 6, full)` and `(8, 10, full)` (Z2's golden ledger).

## 4. What is still missing, and why

`not_computed.tsv` lists the records not computed, each with its reason. Records that exist but are `pending` are counted in `manifest.json`
(`coverage.pending`); the words that keep them pending are the columns `pending_words` and `n_pending` of `rows.tsv`. For the records of edition 1 that no slot-26
package serves, those words are `refused(export_word)` and `refused(k1_route)`: the at-root read for $\ell \ge 46341$ and the (C3) route at $K = 1$ for
$\ell \in [2^{31.5}, 2^{63})$ (parked for slot 27).

`(2089, 0)` has `anemic` `planned_not_finished` (started, over 18 h against a 0.5 - 1.2 h prediction, not stopped, not re-accepted by the Z2 binary: a later pass)
and `full` `not_computed` (predicted peak RSS about 50 GiB). Edition 2 carries 226 records, 140 final and 86 pending.
