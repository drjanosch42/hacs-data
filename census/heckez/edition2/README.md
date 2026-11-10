<!-- SPDX-FileCopyrightText: 2024-2026 Fabian Januszewski -->
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
# The integral Hecke structure data set, edition 2 (`integral_hecke_dataset/v1`, `hecke_ed2`)

**Status: signed (pre-signed by the owner 2026-11-09, conditions met).** The conditions were: `HZUP_DIFF` clean, status counts consistent,
checksums, `(2089, 0)` handled as briefed. All gate lines are below, verbatim. The schema and the meaning of every column are in
`docs/integral_census/hecke_dataset_v1.md`; what edition 2 changes is in `docs/integral_census/hecke_dataset_v2.md`. Read both first.

**What this is.** The same 141 cells $(N, n)$ and 226 records as edition 1, with the 85 `full` records at levels $2 \le N \le 883$ recomputed
on the Z2 binary (slot 26: $U_p$ over $\mathbb Z$ at $\ell \in \{2, 3\}$ and the words of [C] 1.26.0). The 141 other records are edition 1's, byte for byte.
Edition 1 is not modified and keeps verifying.

## Contents of this edition

| | |
|---|---|
| records | **226** (56 level-1 `full`; 85 level-$N$ cells x {`anemic`, `full`}), settings fingerprint `0x8ffc29730c4f9672` |
| final / pending / refused | **140 / 86 / 0** (edition 1: 122 / 104 / 0). 18 of the 85 recomputed records went final (9 with `open(cap)`, 9 with `refused(non_reduced)`), as the Z2 report predicted |
| pending words | `blocked(U_p)` 67 (the Eisenstein $U_p$ systems, package HZ-EISUP), `refused(export_word)` 20, `refused(k1_route)` 2 (3 records carry two words) |
| golden-eligible | 140 (the 15 level-1 cells of the first golden event among them; the two moved pins are `(2, 6, full)` `0x980a4da95386391f`, `(8, 10, full)` `0xe15555ccce92ed3e`) |
| passes | pass 1: 120 records, pass 2: 21, **pass 3: 85** (binary `cf93a54265f82c35e7095d48c63322353279f91b5260592408c530d767812b3f`, source `c4a5fd65`, the merge of Z2) |
| level-1 join to census 4.0 | 140 `equal`, 60 `census_extra(torsion_q1)`, 0 mismatches (unchanged) |
| not computed | 4 cells / 5 records, `not_computed.tsv` (below) |

**Not computed.**

* $(2089, 0)$ **anemic**: started on the Z9 route (threads = 16) on 2026-11-08 19:53 UTC and still running at 2026-11-09 14:30 UTC, more than 18 h against Z9's prediction of
  0.5 to 1.2 h (a prediction miss, recorded). It was not stopped (no-premature-kill rule) and has not been re-accepted by the Z2 binary, so it is not in this
  edition (`planned_not_finished`); it is added in a later pass, which needs the builder's per-root binary support.
* $(2089, 0)$ **full**: not computed, predicted peak RSS about 50 GiB (an extrapolation), above the box's agreed load limit; a candidate for a later window or the cluster.
* $(1, 484)$, $(1, 700)$, $(1, 702)$: deferred before the run (117 000 - 354 000 word primes).

## Files

As edition 1, plus `provenance_pass3.tsv` (per pass-3 record: binary, source commit, the two `EFFECTIVE` lines of its log). `manifest.json` and
`MANIFEST.sha256` (sha256 of every file) are in this directory; everything is in this directory, no external files. `sha256(provenance_pass3.tsv)`
is pinned in `hz_dataset_verify.py` (`PASS3_PIN`).

## How to check it

```
python3 docs/integral_census/hz_dataset_verify.py docs/integral_census/results/hecke_ed2 --base docs/integral_census/results/hecke_ed1 \
        [--binary <cohomology_app, sha256 cf93a542...2b3f> --binary-sha256 <that> --scratch <scratch>]
```

Gate lines of record (2026-11-09):

```
HZUP_DIFF full=85 moved_outside_rule=0 removed=0 verdict=OK
HZDS_BUILD edition=hecke_ed2 cells=141 records=226 final=140 pending=86 refused=0 not_computed=5 files=23 verdict=OK
HZDS_VERIFY edition=hecke_ed2 records=226 cells=141 final=140 pending=86 refused=0 not_computed=5 rehash=226 pins=20/20 golden15=15/15 ... files=21 bad=0 fingerprint=0x8ffc29730c4f9672 verdict=OK
HZDS_VERIFY edition=hecke_ed2 ... binary=cf93a54265f82c35 accepted=226/226 ran=0 ... files=21 bad=0 verdict=OK
HZDS_VERIFY edition=hecke_ed1 ... binary=cf93a54265f82c35 accepted=226/226 ran=0 ... bad=0 verdict=OK   (edition 1 on the Z2 binary: re-acceptance is a consistency re-scan of each record, not a recompute, so it does not say edition 1 is current under Z2)
```

## How it was made

Passes 1 and 2 are edition 1's (see `hecke_ed1_README.md`). Pass 3 (slot 26, package Z4): the 85 `full` records regenerated on the Z2 binary by
`hz_up_recompute.py` (one record per call, `pass3z2/`), every `EFFECTIVE campaign` line `rejected=0`; the earlier pass on `21a00a01` (equal to the C3 recompute on all 85
`record_fnv1a64`) is a superseded data point. Built by `mk_hecke_dataset.py build --base ... --overlay ...` (deterministic).
Reports: `docs/implementation/slot26/Z4_report.md`, `Z1_report.md`, `Z2_report.md`.

## Appendix. History

* 2026-11-09: edition 2 built (HZ-DS, slot 26 Z4), pre-signed by the owner, conditions met.

## Note (2026-11-09)

Manifest corrected to include README.md (Z4 tool audit M1); data files byte-identical; the owner's pre-signature (conditions: gates verdict=OK) holds on the corrected manifest.
