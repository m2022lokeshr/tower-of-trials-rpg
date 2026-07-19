# Game System Design Document (Draft v1)
*Inspired by Hell Mode's rank/talent structure — mockup format for extrapolation*

---

## 1. Core Loop
Appraisal (assign class) → Grind (kill monsters, gain EXP/skill XP) → Rank Up (class skill + dungeon rank) → Unlock Content (higher dungeons, monsters, gear) → Repeat with harder tiers.

---

## 2. Stat System
Six core stats, each shown as a **letter grade** (mapped from raw number):

| Grade | Range |
|---|---|
| E | 0–199 |
| D | 200–499 |
| C | 500–999 |
| B | 1000–1999 |
| A | 2000–3999 |
| S | 4000+ |

Stats: `ATK, AGI, DEF, INT, VIT (HP pool), MP`

Formula (mockup): `Stat = BaseStat + (GrowthRate × Level) + EquipmentBonus + BuffBonus`

---

## 3. Class Tiers (Talents)
Star rarity 1★–8★. Higher star = higher GrowthRate multiplier per level.

| Star | GrowthRate Multiplier | Example Classes |
|---|---|---|
| ★1 | 1.0x | Swordsman, Thief, Mage, Merchant |
| ★2 | 1.5x | Sword Master, Wizard, Archer Elite |
| ★3 | 2.2x | Berserker, Bow Master, Monk |
| ★4 | 3.5x | Sword King, Sage, Holy Knight |
| ★5 | 5.0x | Hero, Grand Spirit User |
| ★6–8 | 8x–20x | Demon King, Summoner-type (special mechanic classes) |

**Design rule:** All classes start with identical base stats (ATK 12, AGI 10, DEF 10, INT 4, VIT 100, MP 20). Only per-level GrowthRate and skill unlocks differ — this is what makes early game feel fair and late game feel unfair (the "garbage balance" hook).

### Full Class List (18)

**Figma skill tree diagrams:**
- Sage: https://www.figma.com/board/x9NfPg1xxCQpX7E3D4QAmV
- Swordsman: https://www.figma.com/board/MHnLyIzQfQyB4le1qTcuwx


| # | Class | Star | ATK/lvl | AGI/lvl | DEF/lvl | INT/lvl | VIT/lvl | Focus |
|---|---|---|---|---|---|---|---|---|
| 1 | Swordsman | ★1 | 3 | 2 | 2 | 0 | 5 | Balanced melee |
| 2 | Fighter | ★1 | 2 | 3 | 1 | 0 | 4 | Speed/melee |
| 3 | Thief | ★1 | 1 | 4 | 1 | 1 | 3 | Evasion/utility |
| 4 | Mage | ★1 | 0 | 1 | 1 | 4 | 3 | Basic caster |
| 5 | Archer | ★2 | 2 | 3 | 1 | 1 | 3 | Ranged DPS |
| 6 | Sword Master | ★2 | 5 | 3 | 3 | 0 | 6 | Melee upgrade path |
| 7 | Wizard | ★2 | 0 | 1 | 1 | 6 | 4 | Caster upgrade |
| 8 | Cleric | ★2 | 1 | 1 | 2 | 4 | 6 | Healer |
| 9 | Berserker | ★3 | 8 | 4 | 2 | 0 | 8 | Glass-cannon melee |
| 10 | Bow Master | ★3 | 4 | 6 | 2 | 1 | 5 | Ranged upgrade |
| 11 | Monk | ★3 | 5 | 5 | 4 | 2 | 7 | Martial hybrid |
| 12 | Saint | ★3 | 1 | 2 | 3 | 6 | 8 | Healer upgrade |
| 13 | Sword King | ★4 | 12 | 6 | 6 | 1 | 10 | Elite melee |
| 14 | Sage | ★4 | 2 | 3 | 3 | 12 | 8 | Elite caster |
| 15 | Holy Knight | ★4 | 10 | 5 | 10 | 3 | 14 | Tank/hybrid |
| 16 | Beast Astrologer | ★4 | 4 | 6 | 3 | 8 | 8 | Buff/debuff support |
| 17 | Hero | ★5 | 20 | 15 | 12 | 8 | 20 | Best all-round stats |
| 18 | Summoner | ★8 | 1 | 1 | 1 | 2 | 5 | No flat growth — power via summon sub-system (Section 6) instead |

**Notes:**
- Stars 1–2: near-identical curves, differentiate mainly via skill kit (feel of a "starter" tier).
- Stars 3–4: growth gap becomes visible; players notice power difference by mid-game.
- Star 5 (Hero): best flat stats in the game, no gimmick needed.
- Star 8 (Summoner): intentionally weak flat growth — all power comes from an external system, so it needs the sub-system in Section 6 to be viable at all. This is the "garbage balance" tradeoff class.

**Skill unlock pattern (apply to every class, reskin names):**
```
Lv1: 2 starting skills (basic attack + 1 utility)
Lv10: 1st unlock (signature skill)
Lv30: 2nd unlock (AoE or buff)
Lv60: 3rd unlock (ultimate/finisher)
```

---

## 4. Monster Rank System
Shared H→S scale for monsters AND dungeons.

| Rank | Lvl Range | HP Mult | ATK Mult | EXP Yield (base) |
|---|---|---|---|---|
| H | 1–5 | 1x | 1x | 5 |
| G | 6–15 | 2x | 1.5x | 20 |
| F | 16–30 | 4x | 2.5x | 80 |
| E | 31–50 | 8x | 4x | 300 |
| D | 51–70 | 15x | 7x | 1,000 |
| C | 71–90 | 30x | 12x | 4,000 |
| B | 91–99 | 60x | 20x | 15,000 |
| A | 99+ (endgame) | 120x | 35x | 60,000 |
| S | Raid-tier | 300x | 70x | 250,000 |

### Full Monster Roster (3 per rank, 27 total)

| Rank | Name | HP | ATK | DEF | EXP | Archetype | Special |
|---|---|---|---|---|---|---|---|
| H | Goblin | 30 | 8 | 3 | 5 | Physical | — |
| H | Slime | 40 | 5 | 6 | 5 | Tank | Splits on death (1 extra spawn, 50% HP) |
| H | Giant Rat | 20 | 9 | 2 | 5 | Fast | High evasion |
| G | Wolf | 60 | 14 | 4 | 20 | Fast/Pack | +10% ATK per nearby pack member |
| G | Orc | 80 | 12 | 7 | 20 | Tank | — |
| G | Harpy | 50 | 13 | 3 | 20 | Flying/Ranged | Immune to melee-only attacks |
| F | Ogre | 150 | 22 | 10 | 80 | Tank | Stun on hit (10%) |
| F | Giant Spider | 100 | 24 | 6 | 80 | Poison | DoT 5%HP/turn, 3 turns |
| F | Skeleton Knight | 130 | 20 | 12 | 80 | Undead | Resist poison/DoT |
| E | Minotaur | 280 | 35 | 18 | 300 | Physical | Charge attack (2x dmg, 1 turn windup) |
| E | Wraith | 200 | 38 | 10 | 300 | Magic | Immune to physical, weak to holy |
| E | Griffin | 260 | 33 | 14 | 300 | Flying | Dive attack (ignore 50% DEF) |
| D | Ogre Lord | 500 | 60 | 28 | 1,000 | Tank/Boss-lite | Enrage below 30% HP (+50% ATK) |
| D | Basilisk | 420 | 55 | 30 | 1,000 | Debuff | Petrify chance 15% (skip target's turn) |
| D | Lesser Wyvern | 480 | 58 | 22 | 1,000 | Flying/Breath | AoE fire breath, cooldown 3 turns |
| C | Chimera | 1,000 | 100 | 42 | 4,000 | Multi-element | 3 attack types (fire/poison/physical) |
| C | Stone Golem | 1,200 | 80 | 60 | 4,000 | Tank | Immune to crits |
| C | Dark Elf Assassin | 800 | 120 | 25 | 4,000 | Evasion | 30% dodge, ignores 20% target DEF |
| B | Wyvern | 2,000 | 170 | 75 | 15,000 | Flying/Boss | AoE breath, ignores 20% DEF |
| B | Lich | 1,600 | 190 | 50 | 15,000 | Summoner-type | Summons 2 skeleton adds on spawn |
| B | Armored Behemoth | 2,400 | 140 | 100 | 15,000 | Tank/Boss | Reflects 10% physical dmg taken |
| A | Dragon | 4,000 | 300 | 130 | 60,000 | Raid boss | AoE breath ignores DEF, 20% max HP dmg |
| A | Ghost King | 3,200 | 330 | 80 | 60,000 | Magic-only vuln | Immune to physical entirely |
| A | Ancient Armor | 4,500 | 250 | 200 | 60,000 | Extreme tank | 90% physical dmg reduction |
| S | Elder Dragon | 10,000 | 600 | 250 | 250,000 | World boss | Server-wide raid, multi-phase |
| S | Demon Lord's General | 8,500 | 650 | 200 | 250,000 | World boss | Death triggers Rank-Up Surge event (Sec 7) |
| S | Leviathan | 12,000 | 550 | 300 | 250,000 | World boss | Aquatic-only zone, tidal AoE |

**Drop table pattern (apply per rank):** Magic Stone (matching rank, ~50-60% chance) + Gold (scaled to EXP value) + rare cosmetic/gear (1-5% chance, higher at boss tiers).

---

## 5. EXP / Leveling Formula
`EXP to next level = BaseEXP × (Level ^ 1.8) × HardcoreModifier`

- `HardcoreModifier = 1` for normal classes
- `HardcoreModifier = 100` for "Hell Mode"-style optional hardcore class flag (higher risk/reward game mode toggle, no level cap)
- Normal classes cap at Level 99; Hardcore-flagged classes: no cap.
- Assumed `BaseEXP = 10` for the test below.

### Balance Test (sample progression, using Section 4 monster EXP)
Assumes ~20 sec average time per kill (combat + travel).

| Level | EXP Needed (Normal) | Recommended Monster Rank | EXP/Kill | Kills Needed (Normal) | Time (Normal) | EXP Needed (Hardcore) | Kills Needed (Hardcore) | Time (Hardcore) |
|---|---|---|---|---|---|---|---|---|
| 10 | 631 | G | 20 | 32 | ~11 min | 63,100 | 3,155 | ~17.5 hrs |
| 30 | 4,560 | F | 80 | 57 | ~19 min | 456,000 | 5,700 | ~31.7 hrs |
| 60 | 15,880 | D | 1,000 | 16 | ~5 min | 1,588,000 | 1,588 | ~8.8 hrs |
| 99 | 39,000 | B | 15,000 | 3 | ~1 min | 3,900,000 | 260 | ~1.4 hrs |

**⚠️ Balance issue found:** kills-needed *drops* at high level for Normal mode (57 kills at Lv30 → 3 kills at Lv99), because monster EXP yield (Section 4) scales faster than the EXP curve's exponent (1.8). This makes late-game trivially fast for Normal players — likely not intended.

**Fixes to consider (pick one before building):**
1. Raise the level-curve exponent (try 2.2–2.4) so EXP needed scales closer to monster EXP yield growth.
2. Reduce monster EXP yield growth at high ranks (flatten Section 4's EXP column past rank C).
3. Add a level-vs-monster-rank penalty: EXP earned drops sharply if player is overleveled for the monster's rank (common MMO pattern, discourages farming low content).

Recommend running this same table again with your actual chosen numbers once decided — this is a 10-minute spreadsheet check that avoids discovering "endgame takes 1 minute" after full build.

---



## 6. Special Mechanic Class Example (Summoner-type)
Use as **one optional class**, not the core game.

- Resource: **Summon Cards** stored in inventory (10 slots base, +10 per skill level)
- Card ranks: H→S, gated by Summon Skill Level 1–9
- Actions: `Create` (spend MP+Magic Stone), `Delete`, `Synthesize` (2 same-rank → 1 higher rank), `Strengthen` (permanent flat stat buff to a card)
- Passive: active summons grant flat stat bonus to owner (Beast-type→ATK/AGI, Insect-type→DEF/VIT)

This is a good **template for any "collection/pet" subsystem** (companions, gacha units, etc.) — swap "Summon" for "Recruit/Tame/Craft" as needed.

---

## 7. Random Event System
| Event Type | Trigger | Effect (mockup) |
|---|---|---|
| Rank-Up Surge | Server-wide, scheduled or boss-kill triggered | All monsters in a zone gain +1 rank for 24h, EXP/drops scaled up |
| Rare Spawn | % chance per monster kill (e.g. 0.5%) | Elite variant spawns with 5x stats, guaranteed rare drop |
| Dungeon Collapse | Timer-based per dungeon instance | Forces exit, resets floor, bonus EXP if cleared before trigger |
| Double EXP Weekend | Scheduled live-ops | Global EXP_Multiplier × 2 |
| World Boss | Community threshold (total kills/resources) | Spawns rank-S boss, server-wide loot event |

---

## 8. Character/NPC Archetype Templates
Don't copy the source's named cast directly — use these role slots and reskin:

```
Role: Rival/Prodigy
Star: High (4-5)
Personality Hook: Naturally gifted, looks down on grinders early, respects earned power later
Story Function: Benchmark for player power growth
```
```
Role: Mentor/Guild Leader
Star: N/A (retired hero)
Story Function: Gatekeeper for rank-up quests, dialogue exposition
```
```
Role: Support/Healer Companion
Star: Low-Mid
Story Function: Party utility, humanizes grind with relationship/loyalty mechanic
```
```
Role: Final Antagonist
Star: 8+ / Boss-tier
Story Function: Endgame raid target, may have "world rank-up" power (ties to Section 7 Rank-Up Surge event)
```

---

## 9. Skill Trees Per Class

**Structure (applies to all 18 classes):** 4 tiers, branching from Lv30 onward.

```
Lv1   : 2 starting skills (fixed, no choice)
Lv10  : 1 unlock (fixed, signature skill)
Lv30  : Choose 1 of 2 (branch A: offense / branch B: utility)
Lv60  : Choose 1 of 2 (locked to branch chosen at Lv30 — deepens specialization)
```
This gives each class 2 viable "builds" without needing separate sub-classes.

### Example 1 — Swordsman (★1)
**Figma visual:** https://www.figma.com/board/9dWmaTTcd1DYVp7jv4gCms
```
Lv1:  Slash (basic ATK) | Guard (–30% dmg taken, 1 turn)
Lv10: Power Strike (1.5x dmg, single target)
Lv30: [A] Cleave (hits 2 enemies)  vs  [B] Parry (counter-attack on block)
Lv60: [A→] Whirlwind (hits all enemies, AoE)  vs  [B→] Riposte (guaranteed counter + stun)
```

### Example 2 — Sage (★4)
```
Lv1:  Firebolt (magic ATK) | Focus (+20% INT, 3 turns)
Lv10: Chain Lightning (hits 3 targets)
Lv30: [A] Meteor (heavy single-target nuke)  vs  [B] Mana Shield (absorb dmg with MP)
Lv60: [A→] Apocalypse (AoE nuke, long cooldown)  vs  [B→] Arcane Ward (party-wide shield)
```

### Example 3 — Summoner (★8, sub-system class)
Skill tree governs the *Summon skill itself*, not combat spells directly (ties to Section 6):
```
Lv1:  Summon | Create | Delete
Lv10 (Summon skill Lv2): Synthesis unlocked
Lv30 (Summon skill Lv4): Storage unlocked (infinite item storage)
Lv60 (Summon skill Lv6): Awakening unlocked → summons can become "Generals" (squad buff aura)
```
No branching choice — the whole class *is* the branch (which summon types you invest Magic Stones into is the real build decision).

**Extrapolation pattern for remaining 15 classes:** reuse the Lv1/10/30/60 skeleton, reskin skill names to match archetype (Tank → block/taunt/reflect chain; Healer → heal/cleanse/revive chain; Ranged → snipe/multi-shot/execute chain), and always give a Lv30 offense-vs-utility fork.

### Remaining 15 Classes

```
Fighter (★1)
Lv1:  Punch | Sprint (+AGI 3 turns)
Lv10: Combo Strike (2-hit)
Lv30: [A] Rush (gap-close+dmg) vs [B] Counter Stance
Lv60: [A→] Flurry (4-hit) vs [B→] Perfect Counter (auto-parry)

Thief (★1)
Lv1:  Stab | Hide (evasion +30%, 1 turn)
Lv10: Backstab (2x dmg from stealth)
Lv30: [A] Poison Blade vs [B] Steal (item/gold)
Lv60: [A→] Assassinate (execute <20% HP) vs [B→] Vanish (reset stealth mid-fight)

Mage (★1)
Lv1:  Spark | Barrier (–20% magic dmg)
Lv10: Fireball
Lv30: [A] Ice Lance (slow) vs [B] Mana Regen
Lv60: [A→] Frost Nova (AoE freeze) vs [B→] Overcharge (double MP pool)

Archer (★2)
Lv1:  Shot | Aim (+ crit 1 turn)
Lv10: Multi-Shot (2 targets)
Lv30: [A] Piercing Arrow vs [B] Trap (immobilize)
Lv60: [A→] Rain of Arrows (AoE) vs [B→] Snipe (guaranteed crit, long CD)

Sword Master (★2)
Lv1:  Slash+ | Guard+
Lv10: Iaido (first-strike bonus dmg)
Lv30: [A] Cross Slash vs [B] Deflect (reflect projectile)
Lv60: [A→] Blade Storm vs [B→] Perfect Guard (0 dmg 1 turn)

Wizard (★2)
Lv1:  Bolt+ | Barrier+
Lv10: Chain Bolt (2 targets)
Lv30: [A] Firestorm vs [B] Mana Shield
Lv60: [A→] Meteor vs [B→] Arcane Barrier (party shield)

Cleric (★2)
Lv1:  Heal | Bless (+DEF 1 turn)
Lv10: Group Heal
Lv30: [A] Smite (holy dmg) vs [B] Cleanse (remove debuff)
Lv60: [A→] Judgment (AoE holy nuke) vs [B→] Revive (1x/battle)

Berserker (★3)
Lv1:  Rage Strike | Bloodlust (heal on hit)
Lv10: Reckless Swing (high dmg, self-dmg)
Lv30: [A] Frenzy (atk speed up) vs [B] Iron Skin (temp DEF)
Lv60: [A→] Bloodbath (AoE + lifesteal) vs [B→] Unbreakable (immune to stun/CC 3 turns)

Bow Master (★3)
Lv1:  Power Shot | Focus Aim
Lv10: Triple Shot
Lv30: [A] Explosive Arrow vs [B] Root Arrow (immobilize)
Lv60: [A→] Arrow Storm (AoE) vs [B→] Perfect Shot (ignore DEF)

Monk (★3)
Lv1:  Palm Strike | Meditate (MP regen)
Lv10: Flurry of Blows (3-hit)
Lv30: [A] Chi Blast (ranged) vs [B] Iron Body (DEF buff)
Lv60: [A→] Dragon Kick (execute) vs [B→] Inner Peace (party-wide CC immunity)

Saint (★3)
Lv1:  Heal+ | Bless+
Lv10: Mass Heal
Lv30: [A] Holy Bolt vs [B] Guardian Angel (shield ally)
Lv60: [A→] Divine Wrath (AoE holy) vs [B→] Miracle (full revive + heal)

Sword King (★4)
Lv1:  Royal Slash | Kingly Guard
Lv10: Blade Rush (gap-close, high dmg)
Lv30: [A] Execution Strike vs [B] Aura of Command (party ATK buff)
Lv60: [A→] Thousand Cuts vs [B→] Sovereign's Shield (party dmg reduction)

Holy Knight (★4)
Lv1:  Smite Strike | Holy Guard
Lv10: Consecrate (zone heal+dmg reduction)
Lv30: [A] Judgment Blade vs [B] Taunt (aggro pull)
Lv60: [A→] Divine Retribution vs [B→] Bulwark (immune 1 turn, party)

Beast Astrologer (★4)
Lv1:  Star Mark (debuff) | Beast Call (temp pet buff)
Lv10: Constellation Bind (root)
Lv30: [A] Meteor Shower vs [B] Lunar Blessing (party buff)
Lv60: [A→] Celestial Collapse (AoE) vs [B→] Guiding Star (party-wide crit buff)

Hero (★5)
Lv1:  Hero Strike | Valor (party morale buff)
Lv10: Heroic Charge
Lv30: [A] Excalibur Slash vs [B] Rallying Cry (party-wide buff)
Lv60: [A→] World Ender (ultimate nuke) vs [B→] Last Stand (party-wide revive+shield)
```

---

## 10. Equipment/Gear & Item System

### Slots
`Weapon, Armor, Helmet, Boots, Accessory x2` — 6 total.

### Rarity Tiers (mapped to monster rank drop source)
| Rarity | Source Rank | Stat Bonus Mult | Sockets |
|---|---|---|---|
| Common | H–G | 1x | 0 |
| Uncommon | F–E | 1.5x | 1 |
| Rare | D–C | 2.5x | 2 |
| Epic | B | 4x | 3 |
| Legendary | A | 7x | 4 |
| Mythic | S | 12x | 5 |

### Item Template
```
Name: Iron Sword
Slot: Weapon
Rarity: Common
Base Bonus: +5 ATK
Source: Goblin/Orc drops (H-G rank)
Sockets: 0
```
```
Name: Dragonscale Plate
Slot: Armor
Rarity: Legendary
Base Bonus: +50 DEF, +20 VIT
Source: Dragon (A rank) drop, 3% chance
Sockets: 4
Set: "Dragonslayer" (2pc: +10% fire resist, 4pc: reflect 15% dmg)
```

### Upgrade System (reuses Magic Stones from Summoner economy)
- **Enchant**: consume Magic Stone (matching item's source rank) → +5% stat bonus, max +50% (10 enchants)
- **Socket Gems**: flat stat gems (ATK/DEF/INT/etc.), dropped separately, slot into empty sockets
- **Reforge**: reroll an item's secondary stat line, costs gold + lower-rank Magic Stone

### Formula
`Final Stat = BaseStat + Σ(Equipment Bonus × RarityMult × (1 + Enchant%)) + Socket Gems`

### Item Count Targets (~150 total)
| Rarity | Count | Per Slot (6 slots) |
|---|---|---|
| Common | 40 | ~7 each |
| Uncommon | 40 | ~7 each |
| Rare | 35 | ~6 each |
| Epic | 20 | ~3 each |
| Legendary | 10 | ~2 each |
| Mythic | 5 | 1 each (unique, boss-only) |

Design effort concentrated on Rare+ (35+20+10+5 = 70 hand-tuned items); Common/Uncommon are scaled reskins.

### Class Affinity
Each item tagged: `Physical / Magic / Hybrid / Any`
- Match bonus: **+15% stat bonus** if item affinity matches class type (e.g. Sage=Magic, Swordsman=Physical)
- Mismatched items still equip and function, just underpowered — encourages itemization choice over one guaranteed best-in-slot.

```
Name: Sage's Focus Rod
Slot: Weapon
Rarity: Rare
Affinity: Magic
Base Bonus: +25 INT (→ +28.75 INT if worn by Magic-affinity class)
```

### Crafting System
**Recipe formula:** `Base Material (monster drop) + Magic Stone (matching rank) + Gold`

- Crafted item rarity = rarity tier of materials used
- Player **chooses the stat roll** within that tier's range (vs. RNG on drops) — crafting = guaranteed/controlled path, drops = gambling path. Keeps both loops relevant.
- Gate access: unlocked via Blacksmith NPC + player level or reputation threshold (not available at hour one)
- Failure chance at higher tiers (optional): Epic+ crafts have 20–30% fail chance (consumes materials, no item) — adds risk/reward and a gold/material sink

```
Recipe: Dragonscale Plate (Legendary Armor)
Materials: Dragon Scale x3 (A-rank drop) + Magic Stone A x2 + 50,000 Gold
Craft Success Rate: 70%
Stat Roll Range: DEF 45-55, VIT 15-25 (player picks within range at craft time)
```

### Drop Rate Guidance (ties to Section 4 drop tables)
- Common/Uncommon: 40–60% per kill
- Rare: 5–10%
- Epic: 1–3%
- Legendary/Mythic: 0.1–1%, boss-gated only

---

## 11. Dungeon Structure & Progression Gates

### Access Gating
- Dungeon rank (H–S) must be ≤ player/party's **Adventurer Rank**
- Adventurer Rank raised via: total monster kills + cleared dungeons + guild quests (not just player level, to prevent overleveled solo rushing)

### Floor Structure (per dungeon)
| Dungeon Rank | Floors | Time/Floor | Monsters |
|---|---|---|---|
| H–G | 3–5 | 5 min | rank-matched trash only |
| F–E | 6–10 | 10 min | trash + 1 mini-boss (floor 5) |
| D–C | 10–15 | 15 min | trash + mini-boss every 5 floors |
| B | 15–20 | 20 min | trash + mini-boss + final boss |
| A–S | 20+ | 24 hr (raid-paced) | scripted encounters, world-boss finale |

### Progression Gate Rule
`Enter Dungeon Rank X` requires:
1. Adventurer Rank ≥ X
2. Party avg. equipment rarity ≥ Uncommon for X≥D, ≥ Rare for X≥B
3. Solo entry allowed H–D only; C+ requires party (min 2, recommend 4)

### Rewards Curve
- Floor clear: small EXP + gold
- Mini-boss: guaranteed Rare+ drop chance boost (Section 10 drop table ×1.5)
- Final boss: guaranteed rank-matched Magic Stone + chance at dungeon-exclusive item
- First clear bonus: one-time large EXP/gold reward (encourages exploring new dungeons over farming one)

### Dungeon Collapse (ties to Section 7 random events)
- Timer per floor; failing to clear in time = forced exit, no boss reward, partial floor-clear rewards kept
- Adds urgency without full permadeath — good for casual + hardcore balance

---

## 12. Multiplayer/Economy

### Guilds
- Formed by any player at level threshold (e.g. Lv20+), gold cost to found
- Guild Rank (separate H–S scale) rises via member kills/dungeon clears — unlocks guild perks (shared storage, EXP buff, dungeon discounts)
- Guild Hall: shared storage (materials/gold pool), bulletin board for party-finding

### Trading
- **Player-to-player direct trade**: item/gold swap window, both confirm
- **Auction House**: list item, gold-only, small % tax (sink) on sale — main gold sink besides crafting/enchanting
- Bind-on-equip for Legendary/Mythic (Section 10) to prevent endgame item flooding the AH; Common–Rare freely tradeable

### PvP
- **Arena (1v1/party)**: separate matchmaking pool, stats normalized/scaled to remove pure gear-check (keeps it skill-based, avoids pay-to-win feel)
- **Open-world PvP**: optional flag, only in designated zones (e.g. contested dungeons), loot-drop-on-death optional toggle per server ruleset
- **Guild Wars**: scheduled, territory/resource-node control between guilds, ties into Section 7 world events

### Economy Sinks (to counter inflation)
- Enchanting/reforging costs (Section 10)
- AH tax
- Guild founding/upkeep costs
- Dungeon re-entry fees at higher ranks (optional)

### Economy Faucets (to balance)
- Monster gold drops (Section 4)
- Dungeon clear rewards (Section 11)
- Daily/weekly quest gold rewards

**Balance rule of thumb:** faucets should slightly exceed sinks early game (keeps new players funded), sinks should dominate late game (prevents endgame gold hyperinflation from farming).

---

## 13. Pre-Full-Scale Development Checklist

### A. Before you start prompting AI to build
- **Convert this doc into structured data files** (JSON/CSV for classes, monsters, items) — AI codes far more reliably from schemas than from prose.
- **Build one vertical slice first**: 1 class, 3 monsters, 1 dungeon floor, fully working — before generating all 18/27/150. Catches structural mistakes early, cheaply.
- **Decide your stack now** — pick DB schema for characters/inventory before generating game logic, so AI isn't guessing your data model each session.
- **Write formulas separately** (stat growth, EXP curve, drop rates) in a spreadsheet and sanity-check the numbers yourself first — AI implements your formula exactly, including math errors.

### B. Error-proofing / edge cases to explicitly check
- Level 1 stats can't be 0/negative; level cap behavior at Lv99
- Division-by-zero in any % formula (crit, dodge, drop rate stacking)
- Inventory/socket overflow
- Concurrent trade/AH exploits — server must be source of truth, never trust client-sent gold/item values
- Dungeon disconnect handling (progress save, party continuation)
- Negative gold prevention on failed crafts/trades
- Race conditions on Auction House (2 buyers, same item, same second)

### C. Content still missing (beyond systems already in this doc)
- Class descriptions & flavor text (lore blurb, "who is this for" — needed for class-select UI)
- Tips & tricks per class (branch choices aren't intuitive without guidance)
- Tutorial/onboarding flow (first 10 minutes)
- World lore/setting (why dungeons exist, factions, starting town, NPC backstories)
- UI/UX copy: tooltips, error messages, confirmation dialogs
- Sound/art direction (even placeholder)
- Localization scope

### D. Game mechanics likely still needed for "full-blown"
- Character creation flow (name, appearance, starting class pick)
- Quest system (beyond dungeon clears)
- Chat/social system
- Death/failure penalty (EXP loss? item durability? nothing?)
- Mobile responsiveness / control scheme

### E. Research further before committing
- Server authority model for a browser game (Node/WebSockets vs. serverless — latency/cost tradeoffs)
- Anti-cheat basics (rate limiting, server-side stat validation)
- Realtime sync if combat is live (Socket.io, Supabase Realtime) vs. turn-based/async (cheaper to build solo)
- Balance simulation: script 1000 simulated battles between classes at matched levels, check win-rate isn't skewed, before building UI

### F. Suggested playtesting checkpoint
Get the vertical slice (A) played by 2–3 people who aren't you before scaling content further. Solo dev + AI-generated numbers are usually wrong the first time — cheaper to catch at 1 class than at 18.

---

## Status

**Done:**
- ✅ Core loop, stat system, EXP formula
- ✅ 18 classes (full star-tier spread, growth rates)
- ✅ 27 monsters (3 per rank, H–S)
- ✅ Random event system (5 event types)
- ✅ NPC/character archetype templates
- ✅ Skill trees for all 18 classes (Lv1/10/30/60 branching)
- ✅ 2 Figma skill tree visuals (Sage, Swordsman)
- ✅ Equipment/gear & item system (rarity tiers, sockets, enchanting, crafting, affinity)
- ✅ Dungeon structure & progression gates
- ✅ Multiplayer/economy (guilds, trading, PvP, sinks/faucets)

**Remaining:**
- ⬜ Figma diagrams for remaining 16 classes (if wanted)

## Suggestions

**Design/balance:**
- Your Summoner (★8, near-zero flat growth) is a big risk/reward outlier — playtest it separately before balancing other classes around it.
- Lv30/60 branch choices are currently permanent — consider a paid "respec" item as a monetization-light option.
- 27 monsters may be thin for ranks C–S if players spend a long time there; add 2–3 more per high rank before launch.

**Build priority (suggested order):**
1. Equipment/item system next — it interacts with both class stats and monster drop tables you already have.
2. Dungeon structure — needed to actually gate content using your rank system.
3. EXP balance pass — do this once equipment exists, since gear changes effective grind speed.
4. Multiplayer/economy — last, since it depends on items + dungeons being finalized first.
