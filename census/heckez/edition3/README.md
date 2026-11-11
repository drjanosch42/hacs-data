<!-- SPDX-FileCopyrightText: 2024-2026 Fabian Januszewski -->
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
# The integral Hecke structure data set, edition 3 (`integral_hecke_dataset/v1+D1`, `hecke_ed3`)

**Status: signed (pre-signed by the owner 2026-11-11, conditions met).** The owner pre-signed edition 3 on 2026-11-11; the signature takes
effect when every edition gate reads `verdict=OK`. The gate lines are below, verbatim. The schema and the meaning of every column are in
`docs/integral_census/hecke_dataset_v1.md`; edition 2's changes in `hecke_dataset_v2.md`; what edition 3 changes in `hecke_dataset_v3.md`.

**What this is.** The same 141 cells $(N, n)$ and 226 records as editions 1 and 2. The 85 `full` records at levels $2 \le N \le 883$ are
recomputed by the HZ-EISUP binary (slot 26: the Eisenstein $U_p$ systems over $\mathbb Z$, [C] 1.27.0 `prop:heckeZeisup`); the 141 other
records are edition 2's (= edition 1's), byte for byte. New column `U_p` in `components.tsv` (amendment D1). Editions 1 and 2 are not modified.

## Contents of this edition

| | |
|---|---|
| records | **226** (56 level-1 `full`; 85 level-$N$ cells x {`anemic`, `full`}), settings fingerprint `0x8ffc29730c4f9672` |
| final / pending / refused | **204 / 22 / 0** (edition 2: 140 / 86 / 0; edition 1: 122 / 104 / 0). 82 of the 85 recomputed records are final, 64 of them newly |
| pending words | `refused(export_word)` 20, `refused(k1_route)` 2; `blocked(U_p)` 0 (edition 2: 67) |
| golden-eligible | 204 (the 15 level-1 cells of the first golden event among them); pins 20/20, `(2, 6, full)` now `0xaa516b325e24eed5`, `(8, 10, full)` `0xe15555ccce92ed3e` |
| passes | pass 1: 120 records, pass 2: 21, **pass 4: 85** (binary `1988501f938eacb05ece3848f396913b92fd67a3d8076163fa64226a6c977a2e`, source `6cc94503`, HZ-EISUP) |
| `U_p` of the components | 102 `G1.components[j].U_<p>` keys on 67 records (column `U_p` of `components.tsv`) |
| level-1 join to census 4.0 | 140 `equal`, 60 `census_extra(torsion_q1)`, 0 mismatches (unchanged) |
| not computed | 4 cells / 5 records, `not_computed.tsv` (below) |

The 85 pass-4 records carry `G10.contract = gl2_contract 1.27.0-pre.1, item 8 (d')`: [C] (d′)(vii) fixes the value per binary as the version
whose words the binary writes; the owner signed [C] 1.27.0 on 2026-11-11 as the text of 1.27.0-pre.1 unchanged. `G10` is in neither record hash.

**Not computed.**

* $(2089, 0)$ **anemic**: finished in pass 3 on the Z9 binary `21a00a01` after 23.8 h, after edition 2 was built; not re-accepted by a later
  binary, so not in this edition (`planned_not_finished`, reason restated); added in a later edition after re-acceptance.
* $(2089, 0)$ **full**: not computed, predicted peak RSS about 50 GiB (an extrapolation), above the box's agreed load limit.
* $(1, 484)$, $(1, 700)$, $(1, 702)$: deferred before the run (117 000 - 354 000 word primes).

## Files

As edition 2, with `provenance_pass4.tsv` in place of `provenance_pass3.tsv` (per pass-4 record: binary, source commit, seconds, RSS, the two
`EFFECTIVE` lines of its log, `record_fnv1a64`, the file's sha256; its own sha256 is pinned in `hz_dataset_verify.py`). `manifest.json` and
`MANIFEST.sha256` (sha256 of every file) are in this directory; everything is in this directory, no external files.

## How to check it

```
python3 docs/integral_census/hz_dataset_verify.py docs/integral_census/results/hecke_ed3 --base docs/integral_census/results/hecke_ed2 \
        [--binary <hacs, sha256 1988501f...7a2e> --binary-sha256 <that> --scratch <scratch>]
```

Gate lines of record (2026-11-11):

```
HZUP_DIFF full=85 moved_outside_rule=0 removed=0 verdict=OK
HZEISUP_DIFF records=85 moved=67 equal=18 moved_outside_rule=0 new_outside_rule=0 removed=0 verdict=OK
HECKEZ_EISUP_RUN records=67 in_scope=67 systems=93 mismatched=0 not_predicted=0 verdict=OK
HZDS_BUILD edition=hecke_ed3 cells=141 records=226 final=204 pending=22 refused=0 not_computed=5 files=23 verdict=OK
HZDS_VERIFY edition=hecke_ed3 records=226 cells=141 final=204 pending=22 refused=0 not_computed=5 rehash=226 pins=20/20 golden15=15/15 ... pass=[1:120,2:21,4:85] ... files=21 bad=0 fingerprint=0x8ffc29730c4f9672 verdict=OK
HZDS_VERIFY edition=hecke_ed3 ... binary=1988501f938eacb0 accepted=226/226 ran=0 ... bad=0 verdict=OK
```

The full lines, the self-tests and the release checks are in `docs/implementation/slot26/DS3_report.md`.

## How it was made

Passes 1 and 2 are edition 1's, see its README. Pass 4 (slot 26, HZ-EISUP re-run, 2026-11-11): the 226 records recomputed by
`hz_up_recompute.py` on the frozen binary, `HZUP_RECOMPUTE records=226 nonfull=141 changed=0 full_recomputed=85 ... full_final=82
full_pending=3 verdict=OK`; the 85 `full` records assembled by `mk_hecke_dataset.py overlay --pass 4 --keys <the 85>`, the edition by
`mk_hecke_dataset.py build --base results/hecke_ed2 --overlay <overlay> --pass 4 --descent <edition 2's descent cells> --edition hecke_ed3`
(deterministic; the same tool still rebuilds edition 2 byte for byte). Reports: `docs/implementation/slot26/HZEISUP_rerun.md`,
`HZEISUP_report.md`, `DS3_report.md`.

## Appendix. History

* 2026-11-11: edition 3 built (HZ-DS, slot 26 DS3), pre-signed by the owner, conditions met.
