# PCSX2 Cheat Collection — Unified & Modernized

A consolidated, modern-PCSX2-compatible cheat database built from the original [`xs1l3n7x/pcsx2_cheats_collection`](https://github.com/xs1l3n7x/pcsx2_cheats_collection), a much larger legacy/local collection, and the meaningful cheat additions found across the public fork network.

**Consolidated:** 2026-08-23  
**PNACH files:** 5,478  
**Distinct CRC keys:** 5,250  
**Active `patch=` lines:** 107,384

## Installation

Copy the contents of `cheats/` into PCSX2's `cheats` directory. In PCSX2, enable cheats globally if required, then use **Game Properties → Cheats** to enable individual groups.

For new files, use PCSX2's documented modern naming form:

```text
SERIAL_CRC.pnach
```

Example:

```text
SLUS-20388_339A0B8C.pnach
```

### PS2 BIOS special case

`cheats/00000000.pnach` is intentionally **not** renamed to `SERIAL_CRC.pnach`. It targets the **USA PS2 BIOS v2.20**, and BIOS execution has no normal game serial. Current PCSX2 has been confirmed to load CRC `00000000` during BIOS boot.

The file intentionally has **no `[group]` header**. This preserves the legacy auto-enable behavior for its four patch lines when cheats are globally enabled, since the BIOS has no normal per-game Properties/cheat-toggle context.

This patch is **BIOS-revision-specific**, not universal. PCSX2 will load `00000000.pnach` for BIOS boot regardless of the selected BIOS revision; loading it therefore does not prove the addresses match that BIOS. A USA v1.70 SCPH-50001 BIOS was observed to report the four patches as enabled, but the patch itself is labeled for USA v2.20.

The serial+CRC consolidation pass is complete for every file that could be identified safely. **5,461** files now use the canonical `SERIAL_CRC.pnach` form. **16** legacy files remain unresolved because a serial and/or CRC cannot yet be established without guessing. `00000000.pnach` is a separate intentional BIOS exception rather than an unresolved game file.

## What was modernized

- Converted legacy/unlabelled cheat material into toggleable group syntax where required by the earlier migration pass.
- Repaired malformed group headers and normalized old `Cheats/...` hierarchy separators to `Cheats\...` where applicable.
- Semantically reclassified **16 automatically-created technical/visual/input groups** that the initial legacy migration had generically placed under `Cheats\...`; their `Cheats\` prefix was removed while all patch instructions remained unchanged.
- The original **43 automatically-created legacy groups** have now all been semantically audited: **16 genuine cheats** remain under `Cheats\...`, while **27 technical/visual/input/framerate groups** are standalone (11 handled in the Silent Hill 2 cleanup and 16 in the subsequent database-wide header cleanup).
- Properly disabled old author-disabled `/patch=` entries as `//patch=` instead of exposing them as active modern toggles.
- Corrected clear metadata/directive corruption:
  - `gameplay=` → `gametitle=` (2 occurrences)
  - `gamettitle=` → `gametitle=` (1 occurrence)
  - the lone corrupted `atch=` line was traced to an old disabled `/patch=` entry and restored as `//patch=`
- Removed the stale `zfata.rar` Fatal Frame backup.
- Consolidated meaningful additions from the fork network without bulk-overwriting the existing database.
- Deduplicated **complete ordered cheat groups** only. Individual patch lines are never deduplicated independently, and reordered conditional/extended code sequences are not assumed equivalent.
- Repaired converter output where the same group header was emitted once per raw code line, preserving all patch lines in their original order.
- Preserved distinct implementations under separate toggle names when two groups shared a title but had different patch sequences.
- Standardized documented cheat-tree hierarchy prefixes to backslashes where that earlier migration identified old `Cheats/...` forms. PCSX2 documents `\` as the group hierarchy delimiter; `/` remains legal text in a group name but is not the delimiter used by the current parent/child UI parser.

## Fork consolidation

The public fork network was reviewed rather than simply selecting the most recently updated fork. Most forks are untouched snapshots or contain work already merged elsewhere. The substantive branches reduce to:

- **Nexxus-Drako** — the main consolidation branch for the 2025 contributions from anthoneous, CW-B-W, godmd5, dirsors, FuzzyWuggzy, NangenX, paynssj4, plus additional Nexxus work.
- **WINDROID-EMU / PCSX2OID-CHEATS** — independent large GameHacking-derived additions in 2026.
- **HiroTex** — two later Dragon Ball Z trial/prototype additions in 2026.
- **Souvlaki42** — useful README/documentation expansion, based on anthoneous' branch; its cheat-content ancestry is already represented by the Nexxus consolidation.

This pass examined 30 source artifacts from the meaningful content branches. Results:

- **4** genuinely new PNACH files added.
- **19** source files contributed new groups to existing games.
- **4** source files were already fully represented.
- **3** source artifacts were skipped because they were Git symlink payloads or exact duplicates.
- **5,902** complete cheat groups / **7,704** patch lines imported.
- **476** complete groups skipped because the same ordered patch sequence already existed.

See [`FORK_CONSOLIDATION_REPORT.md`](FORK_CONSOLIDATION_REPORT.md) for the source-by-source audit and corrected fork metadata.

## New files added by the fork sweep

- `SLPM-61152_251BE718.pnach` — from HiroTex/pcsx2_cheats_collection
- `SLUS-21441_FF724D56.pnach` — from HiroTex/pcsx2_cheats_collection
- `SLES-55637_DBAAB66D.pnach` — from Nexxus-Drako/pcsx2_cheats_collection
- `SLPS-25026_32C7D77B.pnach` — from Nexxus-Drako/pcsx2_cheats_collection


## Fork CRC audit and compatibility aliases

Four fork files supplied a CRC that disagrees with the independently established retail executable CRC for the stated serial. The cheat content was still merged into the confirmed retail target, but the fork CRC is now preserved as a **modern serial+CRC compatibility alias** rather than being discarded.

| Fork source | Serial | Fork-provided CRC | Confirmed retail CRC | Current status |
|---|---|---:|---:|---|
| `WINDROID/0EEFD2E0.pnach` | SLUS-20184 | `0EEFD2E0` | `24036809` | Unverified alternate executable CRC; alias retained |
| `WINDROID/28211F7D.pnach` | SLUS-20733 | `28211F7D` | `28270F7D` | Unverified alternate executable CRC; alias retained |
| `WINDROID/99D3310F.pnach` | SLUS-21448 | `99D3310F` | `F3FD313E` | Unverified alternate executable CRC; alias retained |
| `Nexxus/90205D97.pnach` | SLES-52730 | `90205D97` | `CD787D68` | Unverified alternate executable CRC; alias retained |

For the three WINDROID cases, the confirmed retail CRC was calculated from the supplied retail ELF using PCSX2's ELF CRC algorithm, and representative fork patch addresses/instructions were checked against those retail executables. This confirms that the fork cheat sets are applicable to the normal retail targets. It does **not** prove that the alternate CRCs are erroneous: they may refer to modified executables whose code layout remains compatible.

The Nexxus `90205D97` case has no matching ELF available. `SLES-52730 / CD787D68` is independently documented as the retail target, while `90205D97` is retained conservatively as an unverified alternate executable CRC.

The retained aliases are:

- `SLUS-20184_0EEFD2E0.pnach`
- `SLUS-20733_28211F7D.pnach`
- `SLUS-21448_99D3310F.pnach`
- `SLES-52730_90205D97.pnach`

Each alias contains only the cheat set actually supplied under that fork CRC; it does not copy unrelated cheats from the retail file. This avoids asserting that every retail cheat works against an unknown modified executable.

`Nexxus/SLPM-66550.pnach` was **not** a CRC reassignment: the source filename supplied a serial only. It was resolved to the independently documented retail `SLPM-66550 / AEDAEE99` target, so no speculative alternate CRC exists to preserve.

`WINDROID/5A6A935D.pnach` likewise did not require a CRC change. Its CRC is the documented NTSC-U `SLUS-21813` executable; only the source comment incorrectly called it PAL-M5. The PAL `SLES-55345` release uses a different CRC.

## Haunting Ground NTSC-U post-consolidation correction

The previously unresolved `Haunting Ground (NTSC-U).pnach` was user-confirmed as **SLUS-21075 / CRC `901AAC09`** and identified as a GameHacking.org export. It has been merged into `SLUS-21075_901AAC09.pnach` and the redundant unmarked file removed.

A full comparison against the current original `xs1l3n7x/pcsx2_cheats_collection` Haunting Ground file also found four logical gameplay cheats that were absent from the downloaded legacy copy used as the original consolidation input: **Max Souls**, **Infinite Item Usage (Need More Than 1)**, **Always Max Items (On Pickup)**, and **Always Invisible**. Those were restored. The missing **Have Ultra Big Boobs** visual group was also restored.

The canonical file additionally preserves material not present in the current original GitHub file, including the GameHacking-derived **Gives Fiona More Natural Breasts** and **Press L3 For Closer Camera, R3 Regular Camera** groups, plus the pre-existing `Master/Test` group from the larger legacy collection.

## Validation status

This release is **structurally validated, not gameplay-tested across the entire database**.

Automated checks verify that:

- no active `patch=` line occurs before a valid group, **except the intentional headerless PS2 BIOS `00000000.pnach` special case**;
- no active `/patch=`, `atch=`, `gameplay=`, or `gamettitle=` directives remain;
- no duplicate group name exists within an individual PNACH file;
- every imported fork group is represented under its intended CRC after consolidation;
- source patch sequences are kept intact and ordered;
- the existing retail/fallback database structure is unchanged; the four newly retained alias CRCs are serial-specific compatibility files and do not alter their confirmed retail targets;
- `zfata.rar` is absent.

Old `/patch=` cheats remain commented out on purpose. They can be revisited against the appropriate ELF/game build later rather than being silently activated just because the modern UI can toggle them.

## Index

[`CHEAT_INDEX.csv`](CHEAT_INDEX.csv) contains one row per PNACH file with filename, detected serial, CRC, title/metadata hint, group count, active patch-line count, and commented patch-line count. This replaces an enormous hand-maintained README table and can be regenerated mechanically.

## Provenance and credits

The original collection credits **Ultimate PCSX2 Cheater (UPC)** and **root670/CheatDevicePS2** as its initial decrypted-code sources. This unified edition preserves existing author/description metadata and incorporates public contributions from the fork network, including xs1l3n7x, OctopusButtons, YoshiCrystal9, anthoneous, Souvlaki42, lily-gh, CW-B-W, godmd5, dirsors, FuzzyWuggzy, NangenX, paynssj4, Nexxus-Drako, WINDROID-EMU, HiroTex, and the original code authors identified inside individual PNACH files.

GameHacking.org was used as a **cross-reference/provenance aid**, not as a separate wholesale import source. Some fork additions themselves explicitly derive from GameHacking exports; those contributions remain credited as supplied by their fork source.

## Related projects

There are other active PS2/PCSX2 patch repositories, but current searches did not reveal a newer drop-in general-purpose cheat database that supersedes this collection. Projects such as PCSX2's official patches repository, PS2-Widescreen's PNACH database, and game-specific cheat repositories have different scopes and should not be blindly merged into a general cheat collection.

## Pre-consolidation integrity cleanup

Before the legacy filename/file-content consolidation, the entire PNACH tree received a stricter parser-oriented corruption audit. Mechanical syntax errors, converter-swallowed raw codes, concatenated groups, malformed metadata keys, and other defects capable of hiding or mis-grouping patch lines were repaired. See [`MALFORMATION_AUDIT.md`](MALFORMATION_AUDIT.md) for the detailed record.

Non-hex modifier templates are **not deleted**. If a source intentionally uses human-replaceable notation such as `?`, `????`, `xxxx`, `zz`, or `??`, the whole affected group is kept in the PNACH but commented so users inspecting the file can still recover the original template without PCSX2 attempting to execute an incomplete code.

Two placeholder-looking cases were instead recoverable as deterministic typos: twelve Ripple no Tamago `000000x64` favorability values were corrected to `00000064`, and Oharuchan GOGOGO's `??FFFFFFFF` middle unlock word was corrected to `FFFFFFFF` based on its contiguous all-ones bitfield.

A final numeric-width audit also repaired six 9-digit address typos whose intended 8-digit forms were determinable from repeated conditions or neighboring field patterns; all active address fields now fit the expected width.

The finalized Silent Hill 2 US cleanup is already part of this baseline: the database contains one authoritative PNACH for black-label v1.20 (`SLUS-20228_8E8E384B.pnach`) and one for Greatest Hits v2.01 (`SLUS-20228_FE06A030.pnach`).

## Remaining unresolved identities

The serial+CRC consolidation is complete except for **16** legacy files whose identity still cannot be established safely from the available filename/content evidence. They remain under their original filenames rather than receiving guessed serials or CRCs. The BIOS `00000000.pnach` file is **not** counted among these 16; it is an intentional special-case filename documented above.

## Serial + CRC consolidation

The collection has been consolidated around PCSX2's modern `SERIAL_CRC.pnach` naming convention. Legacy CRC/title files that could be resolved safely are renamed or merged by **serial + CRC**; files sharing a CRC across different serials are kept as separate serial-specific targets. Existing modern files are preferred as the merge base.

Exact duplicate complete code groups are deduplicated, while same-name groups containing different implementations are preserved as alternatives. No missing CRC is guessed merely from a matching serial. Files that still lack enough trustworthy identity information remain under their original names and are listed in [`CONSOLIDATION_REPORT.md`](CONSOLIDATION_REPORT.md).

See [`CONSOLIDATION_MAP.csv`](CONSOLIDATION_MAP.csv) for the complete source-to-target mapping and [`CONSOLIDATION_AUDIT.json`](CONSOLIDATION_AUDIT.json) for machine-readable verification details.

