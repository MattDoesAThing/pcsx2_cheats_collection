# Full PNACH Database Audit — 2026-08-25

This audit validates the post-reconciliation cheat tree against the current PCSX2 PNACH parser model and checks that the 2026-08-23 consolidated baseline was preserved except for explicitly documented additions/corrections.

## Result

**PASS** — no structural/parser errors remain in the audited tree.

- PNACH files: **5,483**
- Canonical `SERIAL_CRC.pnach` files: **5,461**
- Unresolved legacy/CRC-only files: **21**
- Distinct CRC keys, including BIOS `00000000`: **5,255**
- Active `patch=` lines: **107,552**
- Parsed groups: **54,971**
- Commented `//patch=` lines: **258**
- Malformed active patch/group lines: **0**
- Active patches outside a group: **0**, excluding the intentional BIOS `00000000.pnach` special case
- Duplicate group names within a PNACH: **0**
- Bad legacy directives (`/patch=`, `atch=`, `gameplay=`, `gamettitle=`): **0**


## Text encoding validation

A dedicated encoding pass was added after the structural audit. Current PCSX2 displays PNACH group names as UTF-8 text, so legacy single-byte Windows-1252 text was normalized to UTF-8.

- PNACH files checked: **5,483**
- Legacy Windows-1252 PNACH files found and normalized: **36**
- Invalid UTF-8 PNACH files after normalization: **0**
- Numeric/directive patch lines changed by encoding normalization: **0**
- Existing valid UTF-8 non-ASCII text (for example `Los 4 Fantásticos`) was preserved unchanged.
- The stale metadata mojibake `dÃ©fense` was corrected to `défense` in `CONSOLIDATION_REPORT.md` and `CONSOLIDATION_AUDIT.json`.

The conversion is encoding-only: each affected PNACH was decoded as Windows-1252 and re-encoded as UTF-8, and all patch/directive lines were verified text-identical before and after conversion.

## Parser model checked

Current PCSX2 stores PNACH patch addresses as `u32` and patch data as `u64`. Its parser rejects addresses that cannot be parsed into `u32`; non-`bytes` data must parse into `u64`. For `extended` cheats, the extended-code engine consumes the low 32 bits or narrower portions according to the code type.

The group-header check mirrors current PCSX2 behavior: a non-comment line beginning with `[` and ending with `]` is treated as a group header. Internal `]` characters are not independently rejected by that parser.

Reference implementation:
- https://github.com/PCSX2/pcsx2/blob/master/pcsx2/Patch.h
- https://github.com/PCSX2/pcsx2/blob/master/pcsx2/Patch.cpp
- https://github.com/PCSX2/pcsx2/blob/master/common/StringUtil.h

## Deterministic corrections made by this audit

These corrections change only malformed/over-wide field text whose intended 32-bit value is independently determinable. No group was added or removed by these corrections, so the active patch-line count remains unchanged.

### Wizardry: Tale of the Forsaken Land — `SLUS-20259_DD11BEF7.pnach`

Three occurrences in `Unlock Everything At Vigger's Shop`:

- `090909094` → `09090909` (address field, three occurrences)

`090909094` exceeds PCSX2's `u32` address parser and is rejected. The same PNACH contains the corresponding split shop-unlock groups using `09090909` for the identical repeated-value payload, making the missing/extra digit correction deterministic.

### The Chronicles of Narnia — four PAL CRCs

One occurrence in `1 Hit Kill` in each file:

- `SLES-53706_41094602.pnach`: `100000CE2` → `100000CE`
- `SLES-53707_F29FA505.pnach`: `100000CE2` → `100000CE`
- `SLES-53708_FD9FA4FC.pnach`: `100000CE2` → `100000CE`
- `SLES-53710_F59EA4FB.pnach`: `100000CE2` → `100000CE`

GameHacking.org's raw PAL code lists `202610E4 100000CE`, confirming the trailing `2` in the consolidated source was corruption:
https://gamehacking.org/game/101190

### Critical Velocity — `SLPS-25532_4CCEF323.pnach`

In `0 Time limit stop`:

- `A4408A900` → `A4408A90`

The same PNACH already contains a correct copy of the instruction, and an external raw-code compilation independently lists `202354A4 A4408A90`:
https://www.metagames-eu.com/forums/les-tutoriels-playstation-2/tuto-convertir-des-codes-de-differents-formats-raw-armax-actionreplay-etc-4-print-131552.html

### Dragon Ball Z: Budokai 3 — `SLUS-20998_C97EF0A4.pnach`

In `Tournament Max Level 7 KI Energy P1`:

- `45DAC0007` → `45DAC000`

The paired P1 write immediately above uses `45DAC000`, and the PAL `SLES-52730_CD787D68.pnach` plus retained `SLES-52730_90205D97.pnach` compatibility alias both use `45DAC000` at the same `20499B90` P2 address. The final `7` is therefore a deterministic stray digit.

## Preserved over-wide data fields

The audit found **18** data strings longer than eight hex digits whose numeric value still fits in `u32` because of leading zeroes. These are parser-safe and behavior-equivalent, so they were preserved rather than cosmetically rewritten.

Seven additional `extended` data fields exceed `u32`. PCSX2 accepts them as `u64`, then the relevant extended-code operations consume only the low 32 bits (or less). They remain active and unchanged because a source-specific intended replacement is not deterministic for every case:

| File | Stored data | Effective low 32 bits | Audit disposition |
|---|---:|---:|---|
| `SLES-52569_6E51213C.pnach` | `FFFFFFFFF` | `FFFFFFFF` | Preserved; matching Spyro raw-code provenance uses the same 9-F source value |
| `SLES-54554_07EC202C.pnach` | `FFFFFFFFF` | `FFFFFFFF` | Preserved pending exact-source confirmation |
| `SLES-54554_07EC202C.pnach` | `FFFFFFFFFF` | `FFFFFFFF` | Preserved pending exact-source confirmation |
| `SLPM-62244_A3F06070.pnach` | `1456E60CC` | `456E60CC` | Suspect source value; no deterministic correction established |
| `SLPS-25290_7281FD94.pnach` | `24820000F` | `4820000F` | Suspect source value; no deterministic correction established |
| `SLPS-25771_101CFA06.pnach` | `240300003` | `40300003` | Suspect source value; no deterministic correction established |
| `SLUS-20884_94C56923.pnach` | `FFFFFFFFF` | `FFFFFFFF` | Preserved; published Spyro raw code uses the same 9-F source value |

The published NTSC-U Spyro raw-code page reproduces `20502028 FFFFFFFFF`, so that value is retained as provenance rather than silently normalized:
https://psxdatacenter.com/psx2/games2/SLUS-20884.html

The three suspect non-all-ones values above should be revisited only when a reliable raw source or matching executable evidence establishes the intended instruction/value. They are **warnings, not parser failures**.

## Baseline preservation

All 5,478 files from the authoritative 2026-08-23 baseline remain present. After accounting for the documented deterministic corrections above, every original active patch sequence is preserved in order; the five parent-reconciliation targets only append reviewed groups.

The following sensitive files remain byte-identical to the authoritative baseline:

- `00000000.pnach`
- `SLUS-20228_FE06A030.pnach`
- `SLUS-20228_8E8E384B.pnach`
- `SLUS-21075_901AAC09.pnach`
- `SLUS-20184_0EEFD2E0.pnach`
- `SLUS-20733_28211F7D.pnach`
- `SLUS-21448_99D3310F.pnach`
- `SLES-52730_90205D97.pnach`

## Generated artifacts

- `CHEAT_INDEX.csv` — regenerated from the current 5,483-file tree.
- `CURRENT_FULL_AUDIT.json` — machine-readable current-state audit.
- `BASELINE_1_0_AUDIT.json`, `CONSOLIDATION_AUDIT.json`, and `CONSOLIDATION_MAP.csv` remain historical artifacts of the original consolidation unless explicitly stated otherwise.
