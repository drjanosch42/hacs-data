# `hecke_census_dataset/v2` — the census data-set schema of edition 3

**Status: schema of record for census edition 3 (slot-15 WP-14, step C3-0). Edition 3 is published under it (slot-15 WP-15, 2026-10-01: `results/congruence_census/edition3/`, 80 293 rows, partial at ell = 17 … 59).**
The column set was owner-signed 2026-09-30 ([X] v1.19 slot-15 note (5); plan `docs/implementation/slot15/00-slot15-plan.md`
§4, row C3-0). Owner Q15 (2026-09-30) added `saturated` / `mult_one`. Owner Q6: edition 3 is published **beside**
edition 2 (`results/congruence_census/edition3/`, a sibling), at C3-2, after the owner's sign-off.

Builder: `docs/matrix-engine/tools/census_dataset_v2.py` (`build`, `regress`, `--selftest`). It extends the tools of
record by import and edits none of them. The tools are `congruence_census.py`, and from slot 14 `wpc14/i4prime.py`,
`wpc14/np_extra.py`, `wpc13/classify_any.py` and `wpc14/wslib.py`. So editions 1 and 2 and `w2_dataset` stay
byte-identical by construction, and the regression checks this every run.

## 1. File layout

A v2 data set is a directory holding:

| file | holds |
|---|---|
| `rows.tsv` | line 1 `# schema=hecke_census_dataset/v2 rows=<N> tool=<version> name=<name> base=<files> binary=<sha>`; line 2 `# v1_columns=58 … v2_columns=13 (<names>)`; line 3 the column header; then one row per maximal ideal |
| `v2_summary.txt` | the base files' first lines, then one `V2_COLUMN <name> <value>=<count> …` line per v2 column and one `V2_JOIN …` line (the join statistics below) |
| `inputs.sha256` | sha256 of every base file and side input, and of every `ext_sturm_l5` record read |

Edition 3's own directory (C3-2) adds the edition-level files of edition 2 (`category_table.txt`, `extremal.tsv`,
`findings.txt`, `README.md`, …). That is C3-1 / C3-2's work, not the schema's.

## 2. Columns

**Columns 1–58 are `hecke_census/v1`'s, unchanged and in order** (`congruence_census.py` `COLUMNS`; their meaning is
edition 1's README and [E] `sec:census`). The builder never writes a v1 column. The one exception is `edition`, which
`--edition-label` may set; the regression never sets it. A v2 row's first 58 fields are therefore the base row's, byte
for byte. The base is an edition's `census.tsv` or a data set's `rows.tsv` (`hecke_census_dataset/v1`).

**Columns 59–71 are the v2 columns**, in this order. The signed-off set comes first, then Q15's pair, then two companion
columns. The companions come last so that dropping them moves no other column.

| # | column | values | source and rule |
|---|---|---|---|
| 59 | `residual_type` | `eis(i,j)`, `theta^a(Delta)`, `theta^a(f<k0>)`, `dihedral(Q(sqrt(-ell)))`, `irreducible_other`, `unknown`, `not_evaluated` | I-0's `residual.classify` (the adapter over WP-C13's `classify_any`), on the record's `split.eigenvalues` at `split.primes`. For residue degree `f > 1` it runs over `F_{ell^f}` (WP-C16 J-6, `split.field_polynomial`). It reads `not_evaluated` when the split is not ok. **A type is proved only where `residual_status` starts with `certified`.** |
| 60 | `residual_confidence` | integer | the adapter's confidence: for a dihedral type, the inert primes used; otherwise the number of primes `≠ ell` used |
| 61 | `bernoulli_v` | integer, `na` | `v_ell(B_k/k)` at `k = n + 2` (I-3a `bernoulli_depth.v_exact`); `na` when `(ell − 1) \| k` |
| 62 | `kummer_exact_agree` | `true`, `false`, `undetermined`, `not_applicable` | whether the pipeline's Kummer predictor (the row's `predicted`) matches I-3a's independent irregularity. The expected classes on an `f = 1` H1 row with reducible `ρ̄ = ω^i + ω^j` are `{(k − 2a) mod (ell − 1) : a ∈ {i, j}, a ≤ n/2, irregular}`; on an irreducible row the expected set is empty. On a row with `residual_status = ambiguous` (since census 4.0-pre.3, section b2, 2026-10-27; `np_extra.py`, `slot24/C1K_b2_kummer311.md`) the expected set is computed per candidate `ρ̄` of the classifier's certificate, after the ordinary filter (`a_ell ≠ 0` ⇒ `ρ̄|I_ell ≅ 1 ⊕ ω^{k−1}`, so a reducible candidate whose exponents are not `{0, k − 1}` is dropped); if the surviving candidates disagree the row reads `undetermined` (the trace primes 2, 3, 5, 7 cannot decide: e.g. ell = 311, 479, 719, 839, 1009, where all four are quadratic residues). Rows of editions ≤ 4.0 main / b1 are unchanged by this rule (re-checked: `4.0/kummer40_final.txt`). `not_applicable` on H0 rows and at `f > 1`. `false` is a **finding**. |
| 63 | `residue_degree_full` | integer, `na` | the Hecke-field degree of `ρ̄` (J-6); `na` at `f = 1` |
| 64 | `chi_order` | integer, `na` | for a dihedral `ρ̄` over `F_{ell^f}`, the order of its class character (J-6); `na` otherwise |
| 65 | `ext_sturm_l5` | `passed`, `heuristic`, `null`, `no_sturm`, `not_covered`, `not_computed` | the ideal's `sturm.outcome` in the `ext_sturm_l5` record set (`ell = 5`, even `n ≤ 200`, `sturm = cap=1000`). Rows are matched by `(n, ideal_id)`, with the `witness_poly` equal, and every record must read `cap=1000`, else STOP. `no_sturm` means the ideal has no `sturm` object. `not_covered` means another `ell`, or `n > 200`. `not_computed` means no set was given. The set is `hecke_ext/v3`, run **before** the (d0) / (d1) finality certificates, so this column is the Sturm route at cap 1000 **alone**. It is not the row's finality (that is v1's `final` / `certificate`). |
| 66 | `hidden_congruence` | `true`, `false`, `not_stable:<verdict>`, `not_computed`, `not_applicable` | the [M1] hidden-congruence flag: `dim A_1 < dim A_K/ell` on a `stable` weight-shift block (WP-C13 `classify_any.hidden`), where `A_1` is the image of `T` on `H^1(F_ell)_m` and `A_K` is over `Z/ell^K`. The row is joined to the WS block of its cell whose system agrees mod `ell` with the ideal's split eigenvalues on every common prime (`ell` included: `U_ell` against the WS `a_ell`). The join must be exactly one block, one-to-one, with the WS `D` equal to `module_D`, else STOP. `not_stable:<verdict>` is `reduced_rank1` → `false`, or e.g. `torsion_block`. `not_applicable` on H0 rows and at `f > 1` (WS blocks are `F_ell`-rational). `not_computed` means there is no WS input at the cell. Inputs: `WS_RUNG` / `WS_BLOCK` jsonl (through `wslib.read_ws`, repair R1) or c9 `blocks.tsv`. |
| 67 | `depth_window` | as `zl_depth` (`1`, `>=2`, `{…}`, `cell_level:{…}`), `0`, `not_surveyed`, `not_applicable`, `not_computed` | the `Z/ell²` depth window (Noctua 2 run 1, `depth_window_l2.tsv`: `ell ∈ {7, 11, 13}` at even `66 ≤ n ≤ 300`, `ell ∈ {5, 691}` at `66 ≤ n ≤ 200`), read by `zl_depth`'s rule ([E] `sec:census` (d5)). An `H^0`-meeting row gets the `H^0` invariant-factor exponents over `Z/ell²` (`>=2` at the ring level); a row not meeting `H^0` reads `0`. It is gated per window cell: the window's `dim H^0(F_ell)` must equal the sum of `module_D` over the own-`H^0` rows, and the number of exponents must equal that dimension, else STOP. `not_surveyed` means outside the window: at `n ≤ 64` the ring survey's `zl_depth` is the value of record. It **resolves** v1's `>=1 / not_surveyed`. It is the `Z/ell²` datum only: I-3b's `ell`-adic Eisenstein depth `L_coh` is a WS quantity and is not a v2 column. |
| 68 | `saturated` | `true`, `false`, `unknown`, `not_applicable`, `not_computed` | Q15: `T_m = A` (the `M_k`-side `equal` verdict), from the `FORMS_CENSUS` lines of `HeckeFormsHeckeAlgebraValidation` arm (T) (**post-WP-20 header, a binary at or after `974d915`**; an older header STOPs). `unknown` when the verdict is `quotient_of_subalgebra`. `not_applicable` (`n/a` in the log) off H1 or where `h > 0`. `not_computed` on rows without a line. Arm (T) runs on **non-reduced rows only**, so reduced rows read `not_computed`. Each line is matched on `(source_set, n, ell, ideal_id)`, with `A_D`, `A_dim`, `A_hilbert`, `own_group` and `boundary_split` equal to the row's, else STOP. A line for a row not in the base also STOPs. |
| 69 | `mult_one` | `yes`, `no`, `unknown`, `not_applicable`, `not_computed` | Q15: `Λ_m` free of rank 2 over `T_m`. `yes` / `no` from `min_generators == 2` on an H1, `h = 0`, cuspidal row; `unknown` otherwise. The source is the same as `saturated`'s. |
| 70 | `residual_status` | `certified`, `evidence:_<k>_primes`, `ambiguous`, `inconsistent`, `none`, … | *companion.* `detail.status` of the classifier (spaces as `_`). `residual_type` is a certificate exactly when this starts with `certified` (I-0 report). |
| 71 | `residual_reducible` | `true`, `false`, `unknown` | *companion.* `ρ̄` reducible, from the classifier's raw tag. It differs from "`residual_type` starts with `eis(`" exactly where the adapter gives `theta^0(Delta)` precedence (Δ mod 5, 7, 691 is reducible). F4 of the I-4' findings is keyed on it. `false` at `f > 1`. |

**The two companion columns are not in the signed-off list.** They are carried from the np_extra sidecar because
`residual_type` and `residual_confidence` cannot be read correctly without them. Owner question (WP-14 report §5):
keep them, or drop them (a tail cut, nothing else moves).

**Not columns (proposals only).** WP-12's `loewy_onset_law` / `f1_layer`, I-3b's `L_coh` and the `twisted_eis_split`
tag stay proposals until signed. Adding any of them is a new trailing column (v2.x), never a reordering.

## 3. Value conventions

- No field is empty or contains a tab or newline (the builder STOPs otherwise).
- `not_computed` means the input was not given (or does not cover that cell). `not_applicable` means the column has no
  meaning on that row. `not_covered` means a given input by design does not cover that row (`ext_sturm_l5` only).
- Lists and objects are compact JSON, as in v1.

## 4. Invariants the builder enforces (STOP, never patched)

1. The base is `hecke_census/v1` or `hecke_census_dataset/v1`, with exactly the 58 columns. No `(source_set, n, ell,
   ideal_id)` appears twice.
2. Every row's record exists at `<results>/<record_path>`, is the row's cell, holds the ideal id once, and has the
   row's `witness_poly`.
3. The joins of §2 (Sturm, WS, depth window, forms) hold as stated.
4. `--out` is not under the results tree, and a directory holding `rows.tsv` is never rewritten.

## 5. Regression and self-test (C3-0's exit)

- `census_dataset_v2.py regress` prints
  `C3_0_REGRESSION edition1=ok edition2=ok w2_dataset=ok native1=ok native2=ok native_v5=ok`. It rebuilds editions 1
  and 2 from records and logs and diffs them byte for byte against the published files. It builds v2 over the
  **rebuilt** editions and checks the v1 projection row for row:
  `EDITION1_NATIVE rows=2569 identical=2569`, `EDITION2_NATIVE rows=3285 identical=3285`. It also runs the tool's
  hecke_ext/v5 native check (`EDITION2_NATIVE_V5 rows=3285 identical=3285`).
- `census_dataset_v2.py --selftest` prints `CENSUS_V2_SELFTEST checks=36 failed=0` (per-column oracles and negative
  controls; `docs/implementation/slot15/WP-14_report.md` §3).

## 6. For C3-1

```bash
# the 849 C1a / C1b records + RB / RA returns, staged as v1 data sets first (i4prime.py stage), then:
python3 docs/matrix-engine/tools/census_dataset_v2.py build \
    --base docs/matrix-engine/results/congruence_census/edition2/census.tsv \
    --out <scratch>/s15/edition3_staged --name edition3 --binary-commit <sha> \
    --ws-blocks-tsv docs/implementation/slot14/wpl1/w1_287/blocks.tsv --ws <WS jsonl at the new primes> \
    --depth-window <depth_window_l2.tsv> --sturm-l5-set ext_sturm_l5 \
    --forms-census <arm (T) logs, post-974d915>
```

A data set staged at atlas-free `ell` has its own results view. Its rows' `record_path` values are relative to that
view, so build it in its own `build` call with `--results <view>`. Concatenating the two outputs is C3-1's assembly
step.

## Addendum (S24, 2026-10-22): column 75 `sat_kmax`, from edition 3.5

Owner ruling 2026-10-22. From 3.5-pre.1 (`3.5/rows35_satc.tsv.gz`) the row has **75 columns**: column 75 `sat_kmax`, after `sat_length`, is
**the cap K of the run that produced the row's `sat_defect_*` values** (the stability limit the run allowed: the `K` of
`HECKE_SAT_DEFECT_CELLS="n:ell:K"`). It is not the level at which the tower stabilised: every block was certified stable at some
`K_stab ≤ K`, and `K_stab` is not a census column. A number exactly where `sat_defect_dim` is a number; elsewhere it repeats the
`sat_defect_dim` marker (`not_applicable`, `not_computed:<reason>`). Values 6, 11, 16. The arithmetic word (u32 / u64) depends on ℓ^level, not on K, and is not recorded in any row or log line; in 3.5-pre.1
the K = 16 records have module level 16 (`Lv_run=[11,16]`; 7¹⁶ > 2³²), so they need the u64 word (levels ≥ 12 for ℓ = 7, since 7¹¹ < 2³² < 7¹²),
while the K = 6 / 11 records have `Lv_run` ⊆ {2, 3, 6, 11} (u32 suffices). This is a fact about these records, not a column semantics.
Per-cell provenance: `3.5/sat_kmax35_cells.tsv`. Editions up to 3.4 keep 74 columns (their sat values all have K = 6).

## Addendum (S24, 2026-10-22): off-grid side tables, from edition 3.5-pre.2

`3.5/offgrid_sat35_cells.tsv` and `3.5/offgrid_sat35_blocks.tsv` are **not** census tables (no `hecke_census_dataset/v2` rows): block-level and cell-level
`SAT_DEFECT` data of four cells with `n > 700` (no records exist). Block columns are the log's `SAT_DEFECT` fields of the same names (`dim_I` = `sat_defect_dim`
of the block); they can be joined to nothing in the rows file.

## Addendum (S24, 2026-10-22): ring-probe side tables, from edition 3.5-pre.3

`3.5/ring_loewy35_cells.tsv`, `3.5/ring_loewy35_np5r_blocks.tsv`, `3.5/ring_loewy35_o1r_blocks.tsv` and `3.5/hida_heavy35_acl2o.tsv` are **not** census tables
and add **no** column to the row. Their columns are the log fields of the same names (`LOEWY_RING` lines of `HeckeRingLoewyValidation`; `CELL_LOEWY` JSON of the
WPM1 scan; `HIDA_TOWER` / `HI11` lines of `HeckeHidaHeavyCellProbe`), lists in compact JSON. `hidden` there is the ring sense (slot-14 WP-6 / WP-16: 𝕋/ℓ deeper
than the census algebra on the free part), **not** the census column `hidden_congruence`. Only NP5R lies on the grid; its `(n, ell)` join to the rows is for
reading (the D cross-check), not for filling.

## Addendum (S24, 2026-10-22): the L23 section and the DEEP-1 side table, from edition 3.5-pre.4

`3.5/l23_section35_v2.tsv` has the 19 columns of 3.4's L23 section (`cell_id ell n weight model formal status h0 h1 h2 h3 gate_failures conj_findings record_fnv1a64 lean_dims lean_formal_row lean_commit lean_module lean_replay_log`) and is **not** a `hecke_census_dataset/v2` table; `record_fnv1a64` is the hash of an `orbifold_cell/v2` record (`l23_section35_hash_map.tsv` gives the v1 hash of 3.4). `3.5/deep1_l23_side35.tsv` is not a census table either: columns `ell n mode tier h0 h1 rank_tell rank_tell_minus_1 ok rung K m e_A e_1 reruns rank_lambda ntors max_tors c_2 min_eis_2 c_3 min_eis_3 c_5 min_eis_5 c_7 min_eis_7 tell_units tell_max_finite tell_at_least commute s wall_s maxrss_gib verdict` (empty where the mode has no such field: `h0`, `h1`, `rank_tell*` only in m2; the rest only in r3 / full); `s` is the cell's own seconds, `wall_s` / `maxrss_gib` the driver's. There is no `h2` / `h3`.

## Addendum (S24, 2026-10-24): edition 4.0 — two table sections, grid to n = 1000

From 4.0-pre.1 (`4.0/`, NOT signed off) an edition may hold **more than one rows file, one per U_ℓ budget key** (owner ruling 2026-10-20: rows computed
under different `u_ell` settings keys are never mixed in one table section). `4.0/rows40.tsv.gz` (main: 3.5's rows + C1K-a, the census-3.4 keys) and
`4.0/rows40_b1.tsv.gz` (section b1: C1K-b1, key `budget_mb=262144` / `951d3b518e6e289a`) have the same 75 columns as 3.5; the b1 header line names
`section=b1` and its key. No (ell, n) cell and no prime is in both files. The C1 grid runs to even n = 1000 (500 cells per prime). The `final_reason`
value `closure_unreachable` first occurs in 4.0 (U_ℓ-over-budget cells of C1K-a). Details: `4.0/README.md`.

## Addendum (S24, 2026-10-25): edition 4.0-pre.2 — section b2 (partial)

4.0-pre.2 (NOT signed off) adds a third rows file, `4.0/rows40_b2.tsv.gz` (section b2: C1K-b2 cluster runs O1 / E1 / E2, the same key as b1,
`budget_mb=262144` / `951d3b518e6e289a`, record commit `9939d33c`; header `section=b2`). Same 75 columns; `edition` reads `4.0-pre.2`. b2 is a
section of its own (not merged into b1); no cell and no prime of b2 is in main or b1. It is **partial**: `4.0/coverage40_b2.tsv` marks each
prime `complete` or `partial`, `4.0/missing40_b2.txt` lists the grid cells not yet computed. Details: `4.0/README.md` (section b2).
