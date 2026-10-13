# PNACH Malformation Audit and Repair

This pass is the final structural/source-cleanup stage before filename and same-target PNACH consolidation. The goal is to prevent malformed or half-converted codes from skewing group deduplication.

- PNACH files audited: **5,798**
- Files changed versus the semantic-group baseline: **103**
- Finalized Silent Hill 2 US files are already merged into this tree: `SLUS-20228_8E8E384B.pnach` and `SLUS-20228_FE06A030.pnach`.

## Validation result

- Active malformed `patch=` lines: **0**
- Active patches outside a group: **0**
- Unknown active assignment keys: **0**
- Malformed active group headers: **0**
- Duplicate active group names within a file: **0**
- Active placeholder-valued patch lines: **0**
- Raw `ADDRESS VALUE` lines left active as text: **0**
- Code fragments embedded in active group headers: **0**
- Malformed patch-like active directives: **0**
- Numeric address/data overflow findings: **0**

## Placeholder / modifier-template review

Non-hex letters were reviewed individually instead of treating every occurrence as the same kind of corruption. Ready-to-use PNACH commands must contain concrete hexadecimal values; unresolved human-editable modifier notation is preserved in the PNACH but commented out.

| File | Pattern | Decision | Reason |
|---|---|---|---|
| `722EC0E2 - Ripple no Tamago SLPM-62340.pnach` | `000000x64` | **repaired** | 6 lines corrected to 00000064; seventh favorability entry already used 00000064 and 0x64 = decimal 100. |
| `D2063214 - Simple 2000 Series Vol.074 - The Oujisama to Romance - Ripple no Tamago SLPM-62579.pnach` | `000000x64` | **repaired** | 6 lines corrected to 00000064; same code set as the other Ripple no Tamago release. |
| `FC0F8A5B - Simple 2000 Series Vol.114 - The Jo'okappichi Torimonochou - Oharuchan GOGOGO! SLPS-20489.pnach` | `??FFFFFFFF` | **repaired** | Corrected to FFFFFFFF. Adjacent writes FFFFFFFF / [malformed] / FFFF form a contiguous all-ones unlock bitfield. |
| `9A51B627 - Fatal Frame II - Crimson Butterfly SLUS-20766.pnach` | `0000000?` | **commented template** | Original source uses ? as user-replaceable Chapter Select modifier; not PNACH wildcard syntax. |
| `951555A0 - Disgaea 2 - Cursed Memories SLUS-21397.pnach` | `3402????` | **commented template** | Original code sources explicitly publish ???? modifier forms; requires a concrete immediate value. |
| `9027FB4C -  Taito Legends SLUS-21122.pnach` | `240200??` | **commented template** | Weapon and level modifier groups explicitly use ?? as the user-selected value; helper blocks remain commented together. |
| `721DEBE4 - Sengoku Basara X SLPM-55008.pnach` | `zz000001` | **commented template** | Published raw-code source itself contains zz000001; no verified mapping for zz was located. |
| `354B02C6 - Only You Liber Cross SLPS-25150.pnach` | `0000xxxx` | **commented template** | Likely attack-power modifier. 270F is contextually plausible from neighboring max-stat codes but not verified, so no value was invented. |
| `E263BC4B - Demento SLPM-65913.pnach` | `xxxx / xxxxxxxx` | **commented templates** | Modifier templates and their value notes are preserved in-file; helper lines that had leaked active are now commented with their templates. |

### Fatal Frame II test result

`Chapter Select` remains commented. The original source uses `?` as a human-replaceable chapter modifier rather than wildcard syntax. A direct test of the distributed/incomplete form caused a softlock when confirming Gallery; music continued until the track ended and the game remained frozen.

### Ripple no Tamago `000000x64`

The twelve malformed favorability lines (six in each of two releases) were corrected to `00000064`. This is not being treated as a modifier template: the seventh favorability entry already uses `00000064`, the same files use `00000064` for a value of 100 elsewhere, and `0x64` is decimal 100—matching “Max Favorability.”

### Oharuchan GOGOGO `??FFFFFFFF`

This was restored rather than disabled. The affected “All items on sale” group writes `FFFFFFFF` at `20207608`, then the malformed value at the next 32-bit word `2020760C`, followed by `FFFF` at `10207610`. The intended contiguous all-ones bitfield is therefore `FFFFFFFF / FFFFFFFF / FFFF`; the middle value is repaired to `FFFFFFFF`.

## 9-digit / overflow address repairs

A final numeric-width pass found six active address fields made entirely of hexadecimal digits, so they had escaped the earlier syntax-only checks, but they exceeded a 32-bit PNACH address. All six were recoverable deterministically from their own surrounding code:

| File | Malformed | Repaired | Basis |
|---|---:|---:|---|
| Shin Contra SLPM-62264 | `120912410` | `20912410` | 32-bit Combat Training unlock write; extra leading `1` |
| Shin Contra SLPM-62247 | `120912410` | `20912410` | Same code in the other release |
| J-League WE10 v2.01 | `D0B888E74` | `D0B88E74` | Identical condition appears correctly twice immediately below |
| J-League WE10 v1.03 | `D0B888E74` | `D0B88E74` | Identical condition appears correctly twice immediately below |
| Hoshigari Empusa | `2003B7308` | `003B7308` | Every Intelligence/Health pair is a consecutive byte write `...07` / `...08` |
| DDR Festival | `2015A3F00` | `2015A3F0` | Same toggle group restores the original instruction at `2015A3F0` |

## Preservation policy

- Incomplete modifier/template groups stay **inside their PNACH files but commented out**, with an explanation next to them.
- Valid helper lines belonging to an incomplete template are commented together so they cannot execute partially.
- Unrecoverable malformed groups likewise remain visible as comments instead of being silently deleted.
- Deterministic corruption is repaired only when the intended code can be established from surrounding structure, duplicate releases, or a source record.
- Technical/gameplay semantics are not inferred from malformed data alone.

## Notable converter/source corruption repaired

- Two active `[patch=` typo lines were restored to valid `patch=` directives (Open Season and Rogue Galaxy).
- Several raw code words accidentally swallowed into group headers were restored to their preceding groups (including Growlanser VI, Shinobido, Taito Legends 2, Machi-ing Maker 2, The Oneechan 2 Special Edition, and Medal of Honor JP).
- Concatenated cheat groups/values were split where the source structure was deterministic (including Pop'n Music 14 and Crescent Suzuki Racing).
- A foreign Aliens vs. Predator: Extinction map cheat accidentally embedded in Alone in the Dark PAL was removed from the active database; the correct Alone God Mode code remains.
- Malformed directive keys such as `gametittle=` and `g-sinterlacemode=` were corrected, and a BOM embedded into a metadata key was removed.
- Demento modifier helper lines that remained active beneath commented template headers were commented together with those templates.

## Source notes for reviewed templates

- Fatal Frame II: bungholio's original Chapter Select code publishes `002BCF50 0000000?` and describes replacing the final digit with a chapter value.
- Disgaea 2: historical CodeBreaker lists explicitly publish `????` forms for EXP and other modifiers.
- Sengoku Basara X: the surviving raw-code source itself prints `202922E4 zz000001` for Reinforcements Level Rise Rate; no reliable `zz` table was found.
- Taito Legends / Only You / Demento: the labels and surrounding notes identify the non-hex fields as value/modifier templates; where no verified concrete value exists they remain commented.

