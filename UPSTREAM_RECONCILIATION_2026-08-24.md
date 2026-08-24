# Upstream reconciliation — 2026-08-24

This pass reconciles the consolidated 2026-08-23 database against the pinned current parent snapshot `xs1l3n7x/pcsx2_cheats_collection` commit `6cb57b55939f47dd91a157bfb6cdc7fd157a780c`.

## Baseline

- Consolidated baseline: 5,478 PNACH files
- Distinct CRC keys: 5,250
- Active `patch=` lines: 107,384
- Baseline ZIP SHA-256: `d2f0f9abd30ed1a7846e786306e1e8dac84d4ab914be76571e04051c057e989e`
- Group comparison policy: complete ordered group bodies only; individual patch lines are not deduplicated independently.

## Identified-title additions

Five canonical files were found to be missing reviewed groups from the pinned parent. Existing implementations were preserved; only missing ordered group bodies were added.

- `SLES-54962_9F6F78DB.pnach` — Guitar Hero III PAL: +1 group, 39 patch lines. Adds YoshiCrystal's Fortnite Festival control remap. Parent metadata labels the serial inconsistently; the canonical filename/identity is retained instead of propagating that metadata.
- `SLES-51229_76A68274.pnach` — Virtua Cop: Elite Edition: +21 distinct groups, 29 patch lines. Exact duplicates of existing P1/P2 health, reload, gallery and random-mode bodies were not re-added.
- `SLUS-20967_81D233DC.pnach` — Enthusia Professional Racing: +12 distinct groups, 35 patch lines.
- `SLPM-65555_E09E454C.pnach` — Dragon Quest V: +8 distinct groups, 25 patch lines. Six existing `Everything cost 0 MP` values had whitespace after `extended,`; whitespace was normalized only, preserving addresses and values.
- `SLUS-21006_95CC86EF.pnach` — Ghost in the Shell: Stand Alone Complex: +1 group, 1 patch line (`Extra Ammo`, Code Master).

Identified-title total: **43 groups / 129 active patch lines**.

## PES 2011 correction

An earlier provisional pass attributed four additional groups to `SLES-55637_DBAAB66D.pnach`. A fresh fetch of the pinned parent did not contain those groups and repository search did not establish a traceable source at that pinned revision. They are therefore **not imported**.

## Restored unresolved CRC-only files

Five current-parent files were absent from the consolidated baseline. The parent itself labels their serial as unknown, so no serial has been invented. They remain CRC-only pending independent identification.

- `02E1970F.pnach` — Sega Ages 2500 Series Vol.26 - Dynamite Deka — 1 logical group / 1 patch line
- `0DEDC3B7.pnach` — Persona 4 — 1 logical group / 1 patch line. The CRC resembles retail Persona 4 `DEDC3B71`, but identity is not confirmed and no merge is performed.
- `3A32FD60.pnach` — NFL Blitz 2002 — 4 logical groups / 4 patch lines
- `6926B199.pnach` — 7 Sins — 4 logical groups / 14 patch lines
- `F4D4EBE1.pnach` — Ace Combat — 6 logical groups / 19 patch lines

Unresolved restoration total: **5 files / 16 logical groups / 39 active patch lines**.

## Resulting database totals

After this reconciliation:

- PNACH files: **5,483**
- Distinct CRC keys: **5,255**
- Canonical serial+CRC files: **5,461**
- Unresolved legacy/CRC-only files: **21**
- Special BIOS file: `00000000.pnach`
- Active `patch=` lines: **107,552**

The 2026-08-23 generated audit/index files are retained as historical baseline artifacts rather than silently rewritten without rerunning the complete generator. This reconciliation report is the authoritative delta record for the changes above. A later full regeneration should update derived indexes/audits from the reconciled tree.

## Validation expectations

- No active `patch=` before a group except the intentional BIOS special case.
- No `/patch=`, `atch=`, `gameplay=`, or `gamettitle=` corruption introduced.
- No serial was inferred for the five unresolved CRC-only files.
- Existing Silent Hill 2, Haunting Ground, BIOS and alternate-CRC alias files are outside this reconciliation and must remain unchanged.
