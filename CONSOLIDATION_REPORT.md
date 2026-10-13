# PNACH Serial+CRC Consolidation Report

This pass consolidates legacy CRC/title-named PNACH files into canonical `SERIAL_CRC.pnach` targets. Identity is based on **serial + CRC**, not CRC alone. Patch addresses/opcodes are not rewritten by this pass.

## Current post-reconciliation state (2026-08-25)

The serial+CRC consolidation report below records the original 2026-08-23 consolidation pass. The repository has since received a current-parent reconciliation and a parser-oriented full-tree audit. Current state:

- **PNACH files**: 5,483
- **Canonical `SERIAL_CRC.pnach` files**: 5,461
- **Unresolved legacy/CRC-only files retained**: 21
- **Distinct CRC keys (including BIOS `00000000`)**: 5,255
- **Active `patch=` lines**: 107,552
- **Current structural audit issues**: 0
- **Deterministic audit corrections**: 9 field corrections across 7 existing PNACH files

See [`UPSTREAM_RECONCILIATION_2026-08-24.md`](UPSTREAM_RECONCILIATION_2026-08-24.md) for the parent reconciliation and [`AUDIT_REPORT_2026-08-25.md`](AUDIT_REPORT_2026-08-25.md) / [`CURRENT_FULL_AUDIT.json`](CURRENT_FULL_AUDIT.json) for the current full-tree validation. The original consolidation statistics below remain historical rather than being rewritten as if the 2026-08-23 generator had produced the later additions.

## Original 2026-08-23 consolidation summary

- **Input pnach files**: 5,798
- **Output pnach files**: 5,478
- **Canonical serial crc files**: 5,461
- **Unresolved legacy files retained**: 16
- **Source files consolidated or renamed**: 5,781
- **Targets merged from multiple sources**: 461
- **Existing modern files kept as single source**: 167
- **Legacy single source files renamed**: 4,829
- **Crc only files assigned by unique crc peer**: 18
- **Single source files with case insensitive label collisions normalized**: 5
- **Deduped groups**: 162
- **Same name different code groups preserved as alternatives**: 114
- **Source files covering multiple serial targets**: 129
- **Validation issue count**: 0
- **Mapped source groups missing from output**: 0
- **Single source copy byte changes**: 0
- **Unresolved file byte changes**: 0

## Merge policy

- Existing `SERIAL_CRC.pnach` files are preferred as the base when present.
- Legacy-only targets are renamed to `SERIAL_CRC.pnach` without changing their bytes when no merge is required.
- Multi-serial legacy files are materialized separately for each serial+CRC target rather than creating CRC-wide shared files.
- Complete groups with identical ordered active patch sequences are deduplicated. Duplicate labels/metadata are preserved as comments in the kept group and logged here.
- Same-name groups with different code bodies are **not** discarded; later implementations are renamed `(Alternative)` / `(Alternative N)`.
- CRC-only files are assigned to a serial only when that exact CRC has one unique serial peer elsewhere in this database.
- Missing CRCs are **not inferred from serial alone**. Such files remain unresolved and retain their original filename.
- Unresolved/incomplete modifier templates remain commented in their PNACH files per the pre-consolidation policy.

## Same-name / different-code conflicts preserved

| Target | Source | Original group | Preserved as |
|---|---|---|---|
| `SCPS-15010_C220951A.pnach` | `C220951A - Gran Turismo - Concept 2001 Tokyo SCPS-15010 SCPS-55005.pnach` | `Cheats\Single race all courses & all difficulty 1st place` | `Cheats\Single race all courses & all difficulty 1st place (Alternative)` |
| `SCPS-15010_C220951A.pnach` | `C220951A - Gran Turismo - Concept 2001 Tokyo SCPS-15010 SCPS-55005.pnach` | `Cheats\Unlock All Course Licenses Are Gold` | `Cheats\Unlock All Course Licenses Are Gold (Alternative)` |
| `SCPS-15010_C220951A.pnach` | `C220951A - Gran Turismo - Concept 2001 Tokyo SCPS-15010 SCPS-55005.pnach` | `Cheats\Unlock All Cars` | `Cheats\Unlock All Cars (Alternative)` |
| `SLES-53974_945FBF31.pnach` | `945FBF31 - Dragon Quest VIII SLES-53974.pnach` | `Cheats\Force au maximum` | `Cheats\Force au maximum (Alternative)` |
| `SLES-53974_945FBF31.pnach` | `945FBF31 - Dragon Quest VIII SLES-53974.pnach` | `Cheats\Endurance et défense au maximum` | `Cheats\Endurance et défense au maximum (Alternative)` |
| `SLPS-20470_CC0DFD32.pnach` | `CC0DFD32 - Simple 2000 Series Vol.103 - The Chikyuu Boueigun Tactics SLPS-20470.pnach` | `Cheats\Can Be Reactivated By Selecting An Ally Unit` | `Cheats\Can Be Reactivated By Selecting An Ally Unit (Alternative)` |
| `SLUS-20149_2B58234D.pnach` | `2B58234D - Tribes - Aerial Assault SLUS-20149.pnach` | `Cheats\Infinite Ammo` | `Cheats\Infinite Ammo (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Event Items\'e' Tablet-Slot 1` | `Event Items\'e' Tablet-Slot 1 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Anti-poison Ring-Slot 8` | `Accessory Codes\Anti-poison Ring-Slot 8 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Arctic Ring-Slot 11` | `Accessory Codes\Arctic Ring-Slot 11 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Aroma Earring-Slot 2` | `Accessory Codes\Aroma Earring-Slot 2 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Assassin Necklace-Slot 19` | `Accessory Codes\Assassin Necklace-Slot 19 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Relic Codes\Black Bishop-Slot 6` | `Relic Codes\Black Bishop-Slot 6 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Bloody Cape-Slot 6` | `Accessory Codes\Bloody Cape-Slot 6 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Orb Codes\Blue Orb-Slot 2` | `Orb Codes\Blue Orb-Slot 2 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Brisingamen-Slot 23` | `Accessory Codes\Brisingamen-Slot 23 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Cleric's Ring-Slot 9` | `Accessory Codes\Cleric's Ring-Slot 9 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Coin Of Happiness-Slot 4` | `Accessory Codes\Coin Of Happiness-Slot 4 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Relic Codes\Crystal Skull-Slot 5` | `Relic Codes\Crystal Skull-Slot 5 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Draupnir-Slot 1` | `Accessory Codes\Draupnir-Slot 1 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Armor Codes\Earth Plate-Slot 1` | `Armor Codes\Earth Plate-Slot 1 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Orb Codes\Green Orb-Slot 4` | `Orb Codes\Green Orb-Slot 4 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Heart Brooch-Slot 14` | `Accessory Codes\Heart Brooch-Slot 14 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Relic Codes\Invincible Jar-Slot 4` | `Relic Codes\Invincible Jar-Slot 4 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Jade Mask-Slot 25` | `Accessory Codes\Jade Mask-Slot 25 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Jewel Crush-Slot 20` | `Accessory Codes\Jewel Crush-Slot 20 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Relic Codes\Little Hammer-Slot 10` | `Relic Codes\Little Hammer-Slot 10 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Relic Codes\Lucifer's Sword-Slot 8` | `Relic Codes\Lucifer's Sword-Slot 8 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Magnetic Necklace-Slot 18` | `Accessory Codes\Magnetic Necklace-Slot 18 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Player Record Codes\Max Total Quick-Steps` | `Player Record Codes\Max Total Quick-Steps (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Relic Codes\Meditative Incense-Slot 3` | `Relic Codes\Meditative Incense-Slot 3 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Megingjord-Slot 21` | `Accessory Codes\Megingjord-Slot 21 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Member Plate-Slot 22` | `Accessory Codes\Member Plate-Slot 22 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Armor Codes\Meteor Plate-Slot 2` | `Armor Codes\Meteor Plate-Slot 2 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Mobius' Brooch-Slot 15` | `Accessory Codes\Mobius' Brooch-Slot 15 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Armor Codes\Moonlight Plate-Slot 3` | `Armor Codes\Moonlight Plate-Slot 3 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Perseus's Ring-Slot 7` | `Accessory Codes\Perseus's Ring-Slot 7 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Piko-piko Hammer-Slot 24` | `Accessory Codes\Piko-piko Hammer-Slot 24 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Piyo-Piyo Shoes-Slot 16` | `Accessory Codes\Piyo-Piyo Shoes-Slot 16 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Orb Codes\Purple Orb-Slot 5` | `Orb Codes\Purple Orb-Slot 5 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Qigong Belt-Slot 3` | `Accessory Codes\Qigong Belt-Slot 3 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Raccoon Charm-Slot 5` | `Accessory Codes\Raccoon Charm-Slot 5 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Orb Codes\Red Orb-Slot 1` | `Orb Codes\Red Orb-Slot 1 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Ring of Fire-Slot 10` | `Accessory Codes\Ring of Fire-Slot 10 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Ring of Thunder-Slot 12` | `Accessory Codes\Ring of Thunder-Slot 12 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Sacrificial Doll-Slot 17` | `Accessory Codes\Sacrificial Doll-Slot 17 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Relic Codes\Saisei Incense-Slot 2` | `Relic Codes\Saisei Incense-Slot 2 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Relic Codes\Savage Statue-Slot 9` | `Relic Codes\Savage Statue-Slot 9 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Armor Codes\Solar Plate-Slot 4` | `Armor Codes\Solar Plate-Slot 4 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Accessory Codes\Talisman-Slot 13` | `Accessory Codes\Talisman-Slot 13 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Weapon Codes\Vampire Killer-Slot 5` | `Weapon Codes\Vampire Killer-Slot 5 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Weapon Codes\Whip of Alchemy-Slot 1` | `Weapon Codes\Whip of Alchemy-Slot 1 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Weapon Codes\Whip Of Flames-Slot 2` | `Weapon Codes\Whip Of Flames-Slot 2 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Weapon Codes\Whip Of Ice-Slot 3` | `Weapon Codes\Whip Of Ice-Slot 3 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Weapon Codes\Whip Of Lightning-Slot 4` | `Weapon Codes\Whip Of Lightning-Slot 4 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Relic Codes\White Bishop-Slot 7` | `Relic Codes\White Bishop-Slot 7 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Orb Codes\White Orb-Slot 6` | `Orb Codes\White Orb-Slot 6 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Relic Codes\Wolf's Foot-Slot 1` | `Relic Codes\Wolf's Foot-Slot 1 (Alternative)` |
| `SLUS-20733_28211F7D.pnach` | `SLUS-20733_28211F7D.pnach` | `Orb Codes\Yellow Orb-Slot 3` | `Orb Codes\Yellow Orb-Slot 3 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Event Items\'e' Tablet-Slot 1` | `Event Items\'e' Tablet-Slot 1 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Anti-poison Ring-Slot 8` | `Accessory Codes\Anti-poison Ring-Slot 8 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Arctic Ring-Slot 11` | `Accessory Codes\Arctic Ring-Slot 11 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Aroma Earring-Slot 2` | `Accessory Codes\Aroma Earring-Slot 2 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Assassin Necklace-Slot 19` | `Accessory Codes\Assassin Necklace-Slot 19 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Relic Codes\Black Bishop-Slot 6` | `Relic Codes\Black Bishop-Slot 6 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Bloody Cape-Slot 6` | `Accessory Codes\Bloody Cape-Slot 6 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Orb Codes\Blue Orb-Slot 2` | `Orb Codes\Blue Orb-Slot 2 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Brisingamen-Slot 23` | `Accessory Codes\Brisingamen-Slot 23 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Cleric's Ring-Slot 9` | `Accessory Codes\Cleric's Ring-Slot 9 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Coin Of Happiness-Slot 4` | `Accessory Codes\Coin Of Happiness-Slot 4 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Relic Codes\Crystal Skull-Slot 5` | `Relic Codes\Crystal Skull-Slot 5 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Draupnir-Slot 1` | `Accessory Codes\Draupnir-Slot 1 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Armor Codes\Earth Plate-Slot 1` | `Armor Codes\Earth Plate-Slot 1 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Orb Codes\Green Orb-Slot 4` | `Orb Codes\Green Orb-Slot 4 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Heart Brooch-Slot 14` | `Accessory Codes\Heart Brooch-Slot 14 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Relic Codes\Invincible Jar-Slot 4` | `Relic Codes\Invincible Jar-Slot 4 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Jade Mask-Slot 25` | `Accessory Codes\Jade Mask-Slot 25 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Jewel Crush-Slot 20` | `Accessory Codes\Jewel Crush-Slot 20 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Relic Codes\Little Hammer-Slot 10` | `Relic Codes\Little Hammer-Slot 10 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Relic Codes\Lucifer's Sword-Slot 8` | `Relic Codes\Lucifer's Sword-Slot 8 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Magnetic Necklace-Slot 18` | `Accessory Codes\Magnetic Necklace-Slot 18 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Player Record Codes\Max Total Quick-Steps` | `Player Record Codes\Max Total Quick-Steps (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Relic Codes\Meditative Incense-Slot 3` | `Relic Codes\Meditative Incense-Slot 3 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Megingjord-Slot 21` | `Accessory Codes\Megingjord-Slot 21 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Member Plate-Slot 22` | `Accessory Codes\Member Plate-Slot 22 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Armor Codes\Meteor Plate-Slot 2` | `Armor Codes\Meteor Plate-Slot 2 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Mobius' Brooch-Slot 15` | `Accessory Codes\Mobius' Brooch-Slot 15 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Armor Codes\Moonlight Plate-Slot 3` | `Armor Codes\Moonlight Plate-Slot 3 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Perseus's Ring-Slot 7` | `Accessory Codes\Perseus's Ring-Slot 7 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Piko-piko Hammer-Slot 24` | `Accessory Codes\Piko-piko Hammer-Slot 24 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Piyo-Piyo Shoes-Slot 16` | `Accessory Codes\Piyo-Piyo Shoes-Slot 16 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Orb Codes\Purple Orb-Slot 5` | `Orb Codes\Purple Orb-Slot 5 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Qigong Belt-Slot 3` | `Accessory Codes\Qigong Belt-Slot 3 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Raccoon Charm-Slot 5` | `Accessory Codes\Raccoon Charm-Slot 5 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Orb Codes\Red Orb-Slot 1` | `Orb Codes\Red Orb-Slot 1 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Ring of Fire-Slot 10` | `Accessory Codes\Ring of Fire-Slot 10 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Ring of Thunder-Slot 12` | `Accessory Codes\Ring of Thunder-Slot 12 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Sacrificial Doll-Slot 17` | `Accessory Codes\Sacrificial Doll-Slot 17 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Relic Codes\Saisei Incense-Slot 2` | `Relic Codes\Saisei Incense-Slot 2 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Relic Codes\Savage Statue-Slot 9` | `Relic Codes\Savage Statue-Slot 9 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Armor Codes\Solar Plate-Slot 4` | `Armor Codes\Solar Plate-Slot 4 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Accessory Codes\Talisman-Slot 13` | `Accessory Codes\Talisman-Slot 13 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Weapon Codes\Vampire Killer-Slot 5` | `Weapon Codes\Vampire Killer-Slot 5 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Weapon Codes\Whip of Alchemy-Slot 1` | `Weapon Codes\Whip of Alchemy-Slot 1 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Weapon Codes\Whip Of Flames-Slot 2` | `Weapon Codes\Whip Of Flames-Slot 2 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Weapon Codes\Whip Of Ice-Slot 3` | `Weapon Codes\Whip Of Ice-Slot 3 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Weapon Codes\Whip Of Lightning-Slot 4` | `Weapon Codes\Whip Of Lightning-Slot 4 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Relic Codes\White Bishop-Slot 7` | `Relic Codes\White Bishop-Slot 7 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Orb Codes\White Orb-Slot 6` | `Orb Codes\White Orb-Slot 6 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Relic Codes\Wolf's Foot-Slot 1` | `Relic Codes\Wolf's Foot-Slot 1 (Alternative)` |
| `SLUS-20733_28270F7D.pnach` | `28270F7D - Castlevania - Lament of Innocence SLUS-20733.pnach` | `Orb Codes\Yellow Orb-Slot 3` | `Orb Codes\Yellow Orb-Slot 3 (Alternative)` |
| `SLUS-20833_3DB65A75.pnach` | `3DB65A75 - MegaMan Anniversary Collection SLUS-20833.pnach` | `Cheats\Megaman 5 Infinite Health` | `Cheats\Megaman 5 Infinite Health (Alternative)` |
| `SLUS-20833_3DB65A75.pnach` | `3DB65A75 - MegaMan Anniversary Collection SLUS-20833.pnach` | `Cheats\Megaman 5 Infinite Lives` | `Cheats\Megaman 5 Infinite Lives (Alternative)` |
| `SLUS-21907_EFE4448F.pnach` | `EFE4448F.pnach` | `60 FPS` | `60 FPS (Alternative)` |

### Conflict notes

- **Gran Turismo Concept 2001 Tokyo (`SCPS-15010_C220951A`)**: two legacy sources use different address sets for three identically named unlock/result cheats. Both implementations are retained; no claim is made here as to which address set is correct.
- **Tribes: Aerial Assault (`SLUS-20149_2B58234D`)**: the legacy file itself contains two case-only variants of “Infinite Ammo” with different implementations. Both are retained.
- **Jurassic: The Hunted (`SLUS-21907_EFE4448F`)**: two 60 FPS forms differ in PNACH patch type/address representation (`word` vs `extended`). Both remain available pending later technical-patch verification.

## Intentional BIOS filename exception

`00000000.pnach` is **not** an unresolved game file. It is intentionally retained as the PS2 BIOS special case:

- target metadata: **PS2 BIOS (USA 2.20)**
- CRC context: `00000000`
- serial: none
- filename: intentionally `00000000.pnach`
- group header: intentionally omitted so the four legacy patch lines auto-enable when cheats are globally enabled

Current PCSX2 has been confirmed to load the file during BIOS boot and report all four patch lines enabled. Because the BIOS context does not identify the selected BIOS revision by a game serial/CRC pair, this does **not** make the patch universal: its addresses are specifically labeled for USA BIOS v2.20.

## Haunting Ground NTSC-U correction

`Haunting Ground (NTSC-U).pnach`, previously unresolved, was user-confirmed as **SLUS-21075 / CRC `901AAC09`** and identified as a GameHacking.org export. It has now been merged into `SLUS-21075_901AAC09.pnach` and removed as a redundant unmarked file.

The current original `xs1l3n7x/pcsx2_cheats_collection` Haunting Ground file was also compared in full. Relative to the downloaded legacy copy used by this consolidation, upstream contained four additional logical gameplay cheats which have now been restored:

- Max Souls
- Infinite Item Usage (Need More Than 1)
- Always Max Items (On Pickup)
- Always Invisible

The user GameHacking.org export also contained the missing `Fiona Codes\Have Ultra Big Boobs` group, now restored. Five other groups in that unmarked export were already represented in the canonical file and were deduplicated.

The final canonical file preserves all logical groups from both the current original GitHub file and the user GameHacking.org export. It also retains additional local material not present in current upstream: `Master/Test`, `Fiona Codes\Gives Fiona More Natural Breasts`, and `Press L3 For Closer Camera, R3 Regular Camera`.

## Unresolved files retained unchanged

These **16** files could not be safely converted to `SERIAL_CRC.pnach` from the information currently available. They remain byte-identical under their original names rather than receiving a guessed CRC/serial. `00000000.pnach` is documented separately above as an intentional BIOS exception.

- `Star Ocean - Till the End of Time  - Director's Cut SCAJ 20070-1.pnach` — serial(s): SCAJ-20070
- `8966730F.pnach` — CRC: 8966730F
- `587693B - America's 10 Most Wanted SLES-52366.pnach` — serial(s): SLES-52366
- `Armored Core 2 (Playstation 2 The Best) SLPS-73403.pnach` — serial(s): SLPS-73403
- `Kiwame Mahjong DXII  (Athena Best Collection Vol. 2) SLPM-62521.pnach` — serial(s): SLPM-62521
- `DE37E046.pnach` — CRC: DE37E046
- `FF2F701F - The Dukes of Hazzard - Return of the General Le.pnach` — CRC: FF2F701F
- `Wild ARMs - Advanced 3rd PlayStation2 the Best SCPS-19323.pnach` — serial(s): SCPS-19323
- `4555974 - Tony Hawk's Proving Ground SLES-54964.pnach` — serial(s): SLES-54964
- `10B4DF0B - Winning Post 5 - Maximum 2003.pnach` — CRC: 10B4DF0B
- `Dragon Quest VIII - Sora to Umi to Daichi to Norowareshi Himegimi (Ultimate Hits) SLPM-66481.pnach` — serial(s): SLPM-66481
- `Samurai Kanzenban (PlayStation 2 the Best Reprint) SLPM-74222.pnach` — serial(s): SLPM-74222
- `AE0E098 - Indiana Jones und die Legende der Kaisergruft SLES-50838.pnach` — serial(s): SLES-50838
- `B91DC726 - Yu-Gi-Oh! - Capsule Monster Coliseumt SLPM-656130.pnach` — CRC: B91DC726
- `9BB9C9F9 - Magical Pachinko Cotton - Pachinko Juuki Simulation.pnach` — CRC: 9BB9C9F9
- `40E4D93 - Astro Kyuudan - Kessen!! Victory Kyuudanhen SLPS-25582.pnach` — serial(s): SLPS-25582

## Validation

- Structural/parser issues after consolidation: **0**
- Source groups missing from their mapped output target: **0**
- Single-source rename/copy files changed byte content: **0**
- Unresolved retained files changed byte content: **0**

