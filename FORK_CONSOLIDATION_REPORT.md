# Fork Consolidation Report

Generated 2026-08-23; revised for **Baseline 1.0** after alternate-CRC audit. This report documents structural/database integration; it does **not** claim gameplay verification of every imported cheat.

## Agreed base-database corrections

- `102` old `/patch=` lines changed to `//patch=` so intentionally disabled codes remain disabled.
- `1` `atch=` corruption restored to `//patch=` (Dark Cloud 2; traced to an original disabled `/patch=` line).
- `2` `gameplay=` directives corrected to `gametitle=`.
- `1` `gamettitle=` directive corrected to `gametitle=`.
- `zfata.rar` removed from the active cheats tree.

Affected base files:

- `433951E7 - Mana Khemia 2 - Fall of Alchemy SLUS-21890.pnach` — 96 old `/patch=` lines properly commented.
- `D382C164 - Shin Megami Tensei - Digital Devil Saga 2 SLUS-21152.pnach` — 1 old `/patch=` line properly commented; it is the static “No Random Battles” alternative next to an active controller-toggle implementation.
- `1DF41F33 - Dark Cloud 2 SCUS-97213.pnach` — 5 `/patch=` lines commented plus 1 mangled `atch=` restored as a commented patch.
- `SLES-53143_CC14A552.pnach` and `SLES-53147_F67E4351.pnach` — `gameplay=` → `gametitle=`.
- `SLES-54171_D3F182A3.pnach` — `gamettitle=` → `gametitle=`.

## Fork-network reduction

The GitHub fork graph contains many snapshots whose code state is already represented by later forks. To avoid double-importing the same work, integration was based on actual PNACH content and commit ancestry rather than fork count alone.

- **Nexxus-Drako** contains/consolidates the substantive 2025 cheat work from anthoneous, CW-B-W, godmd5, dirsors, FuzzyWuggzy, NangenX, and paynssj4, then adds its own changes.
- **Souvlaki42** is a fork-of-anthoneous with expanded documentation; the Xenosaga cheat ancestry is already represented through Nexxus.
- **WINDROID-EMU** contains independent 2026 bulk additions.
- **HiroTex** contains independent 2026 DBZ trial/prototype additions.
- Other reviewed direct forks were snapshots, documentation-only branches, or ancestry already represented above.

## Import policy

- Never deduplicate individual patch lines.
- A source cheat is considered already present only when its **complete ordered patch sequence** already exists for the intended CRC.
- Conditional/extended code order is preserved.
- Immediately adjacent repeated converter headers are collapsed only when the group name and author/description metadata match; all patch commands remain in order.
- If two different patch sequences use the same group name, both are retained and the later collision receives a provenance suffix instead of being overwritten.
- Existing legacy filenames are not globally migrated in this pass; newly added files use `SERIAL_CRC.pnach` when the identifiers are trustworthy.

## Source-by-source result

| Source artifact | Result | Target | Groups imported / source | Patch lines imported / source | Notes |
|---|---|---|---:|---:|---|
| `hiro/SLPM-61152_251BE718.pnach` | added-new-file | `SLPM-61152_251BE718.pnach` | 8 / 8 | 226 / 226 |  |
| `hiro/SLUS-21441_FF724D56.pnach` | added-new-file | `SLUS-21441_FF724D56.pnach` | 1 / 1 | 3 / 3 |  |
| `nexxus/2CD5794C.pnach` | already-covered | `2CD5794C - Haunting Ground SLES-52877.pnach` | 0 / 1 | 0 / 1 |  |
| `nexxus/348CEAC4.pnach` | already-covered | `348CEAC4 - The adventures of Cookie & Cream SLUS-20170.pnach` | 0 / 1 | 0 / 1 |  |
| `nexxus/581954FC.pnach` | merged | `581954FC - Grand Theft Auto III SLES-50330.pnach` | 21 / 30 | 41 / 56 |  |
| `nexxus/6E8BAF03.pnach` | merged | `6E8BAF03 - Monster Hunter SLES-52707.pnach` | 2 / 2 | 2 / 2 |  |
| `nexxus/6EA9DDA9.pnach` | merged | `6EA9DDA9 - Resident Evil Code - Veronica X SLES-50306.pnach` | 127 / 155 | 128 / 156 |  |
| `nexxus/79585776.pnach` | merged | `79585776 - Drakengard SLES-52322.pnach` | 134 / 137 | 134 / 137 |  |
| `nexxus/90205D97.pnach` | merged | `CD787D68 - Dragon Ball Z - Budokai 3 SLES-52730.pnach` | 47 / 49 | 89 / 91 | retail merge target resolved as CD787D68 for SLES-52730; fork CRC 90205D97 now retained as an unverified alias; renamed 1 target group collision(s) |
| `nexxus/945FBF31.pnach` | merged | `945FBF31 - Dragon Quest VIII SLES-53974.pnach` | 822 / 835 | 834 / 861 |  |
| `nexxus/A04B52DB.pnach` | merged | `A04B52DB - Ace Combat Zero - The Belkan War SLPS-25629.pnach` | 2 / 7 | 6 / 12 |  |
| `nexxus/DBAAB66D.pnach` | added-new-file | `SLES-55637_DBAAB66D.pnach` | 12 / 12 | 99 / 99 | collapsed 38 immediately repeated converter group headers |
| `nexxus/F266B00B.pnach` | merged | `F266B00B - Kingdom Hearts II - Final Mix SLPM-55020-66675.pnach` | 6 / 8 | 54 / 61 |  |
| `nexxus/SLES-54151_3CB245D5.pnach` | merged | `3CB245D5 - Let's Make a Soccer Team! SLES-54151.pnach` | 1 / 1 | 2 / 2 |  |
| `nexxus/SLES-54153_F0DD361F.pnach` | already-covered | `F0DD361F - Virtua Pro Football SLES-54153.pnach` | 0 / 7 | 0 / 32 |  |
| `nexxus/SLPM-65913_E263BC4B.pnach` | merged | `E263BC4B - Demento SLPM-65913.pnach` | 12 / 21 | 47 / 68 |  |
| `nexxus/SLPM-66550.pnach` | merged | `AEDAEE99 - God Hand SLPM-66550.pnach` | 2 / 4 | 2 / 5 |  |
| `nexxus/SLPS-25026_32C7D77B.pnach` | added-new-file | `SLPS-25026_32C7D77B.pnach` | 60 / 60 | 60 / 60 |  |
| `nexxus/SLUS-20469_6D1276AB.pnach` | merged | `6D1276AB - Xenosaga - Episode I - Der Wille zur Macht SLUS-20469.pnach` | 661 / 677 | 791 / 832 | collapsed 3 immediately repeated converter group headers; renamed 2 residual duplicate group labels |
| `nexxus/SLUS-20892_EB39ABEC.pnach` | merged | `EB39ABEC -  Xenosaga Episode II - Jenseits von Gut und B#U00f6se SLUS-20892 SLUS-21133.pnach` | 351 / 364 | 392 / 408 |  |
| `nexxus/SLUS-21133_EB39ABEC.pnach` | skipped | `` | 0 / 0 | 0 / 0 | Git symlink to SLUS-20892_EB39ABEC.pnach; content represented by the shared CRC source. |
| `nexxus/SLUS-21389_2088950A.pnach` | merged | `2088950A - Xenosaga - Episode III - Also Sprach Zarathustra SLUS-21389 SLUS-21417.pnach` | 918 / 1010 | 940 / 1035 |  |
| `nexxus/SLUS-21417_2088950A.pnach` | skipped | `` | 0 / 0 | 0 / 0 | Git symlink to SLUS-21389_2088950A.pnach; content represented by the shared CRC source. |
| `nexxus/SLUS-21586_EDE83C62.pnach` | already-covered | `EDE83C62 - Guitar Hero Encore - Rocks the 80s SLUS-21586.pnach` | 0 / 5 | 0 / 7 |  |
| `windroid/0518D274.pnach` | merged | `0518D274 - Need for Speed - Most Wanted (Black Edition) SLUS-21351.pnach` | 24 / 36 | 42 / 88 | renamed 1 residual duplicate group labels |
| `windroid/0EEFD2E0.pnach` | merged | `24036809 - Resident Evil Code - Veronica X SLUS-20184.pnach` | 928 / 1166 | 1003 / 1248 | retail merge target confirmed as 24036809 for SLUS-20184; fork CRC 0EEFD2E0 now retained as an unverified alias; collapsed 33 immediately repeated converter group headers; renamed 73 residual duplicate group labels |
| `windroid/28211F7D.pnach` | merged | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | 888 / 905 | 1081 / 1104 | retail merge target confirmed as 28270F7D for SLUS-20733; fork CRC 28211F7D now retained as an unverified alias; collapsed 26 immediately repeated converter group headers; renamed 14 residual duplicate group labels |
| `windroid/5A6A935D.pnach` | merged | `5A6A935D - 007 - Quantum of Solace SLUS-21813.pnach` | 8 / 8 | 15 / 15 | source header region corrected logically: 5A6A935D is SLUS-21813 NTSC-U, not PAL-M5 |
| `windroid/99D3310F.pnach` | merged | `F3FD313E - Rule of Rose SLUS-21448.pnach` | 867 / 868 | 1713 / 1714 | retail merge target confirmed as F3FD313E for SLUS-21448; fork CRC 99D3310F now retained as an unverified alias; collapsed 5 immediately repeated converter group headers; renamed 1 residual duplicate group labels |
| `windroid/SLUS-20184.pnach` | skipped | `` | 0 / 0 | 0 / 0 | Byte-identical duplicate of 0EEFD2E0.pnach and lacks a CRC in the filename. |

## Source-identifier audit after retail-ELF verification

| Source | Supplied identifier | Confirmed retail target | Final classification |
|---|---|---|---|
| WINDROID `0EEFD2E0.pnach` | SLUS-20184 / `0EEFD2E0` | SLUS-20184 / `24036809` | Retail target confirmed from supplied ELF; `0EEFD2E0` retained as an **unverified alternate executable CRC** alias. |
| WINDROID `28211F7D.pnach` | SLUS-20733 / `28211F7D` | SLUS-20733 / `28270F7D` | Retail target confirmed from supplied ELF; `28211F7D` retained as an **unverified alternate executable CRC** alias. |
| WINDROID `99D3310F.pnach` | SLUS-21448 / `99D3310F` | SLUS-21448 / `F3FD313E` | Retail target confirmed from supplied ELF; `99D3310F` retained as an **unverified alternate executable CRC** alias. |
| Nexxus `90205D97.pnach` | SLES-52730 / `90205D97` | SLES-52730 / `CD787D68` | Retail mapping independently established; no `90205D97` ELF available. Fork CRC retained as an **unverified alternate executable CRC** alias. |
| Nexxus `SLPM-66550.pnach` | SLPM-66550, no source CRC | SLPM-66550 / `AEDAEE99` | Missing CRC resolved from the established retail mapping; this was not a conflicting/reassigned CRC. |
| WINDROID `5A6A935D.pnach` | CRC `5A6A935D`, comment says PAL-M5 | SLUS-21813 NTSC-U / `5A6A935D` | CRC unchanged; only the incorrect region/title comment was corrected. PAL SLES-55345 uses `B43AC68E`. |

Compatibility aliases added in Baseline 1.0:

- `SLUS-20184_0EEFD2E0.pnach`
- `SLUS-20733_28211F7D.pnach`
- `SLUS-21448_99D3310F.pnach`
- `SLES-52730_90205D97.pnach`

These aliases contain only the normalized fork-provided cheat set associated with the alternate CRC. The fork material remains merged into the confirmed retail files as well.

## Validation result

- PNACH files: **5,798**
- Distinct CRC keys: **5,250**
- Active `patch=` lines: **103,747**
- Commented `//patch=` lines: **240**
- Imported groups: **5,902**
- Imported patch lines: **7,704**
- Duplicate source groups skipped: **476**
- Cross-file duplicate-group baseline: **51 CRCs / 164 repeated group names**
- Cross-file duplicate-group final: **51 CRCs / 164 repeated group names** (unchanged; no new cross-file collision introduced)
- Structural audit issues: **0**

## Deliberately not done

- No claim that every cheat works in gameplay. Fork additions are source-preserved and structurally integrated.
- No broad CRC-only → serial+CRC migration. Shared CRCs and cross-region overlaps make that a separate verification task.
- No speculative repair of cheat addresses/values. Only metadata/format corrections with clear provenance or independently confirmed identifiers were changed.

## Reference URLs

- Original collection: https://github.com/xs1l3n7x/pcsx2_cheats_collection
- Nexxus consolidation fork: https://github.com/Nexxus-Drako/pcsx2_cheats_collection
- WINDROID fork: https://github.com/WINDROID-EMU/PCSX2OID-CHEATS
- HiroTex fork: https://github.com/HiroTex/pcsx2_cheats_collection
- Souvlaki42 documentation fork: https://github.com/Souvlaki42/pcsx2_cheats_collection
- Current PCSX2 patch-format documentation: https://github.com/PCSX2/pcsx2-net-www/blob/main/docs/advanced/writing-patches.md
