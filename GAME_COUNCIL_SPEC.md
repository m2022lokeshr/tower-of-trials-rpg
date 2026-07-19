# Tower of Trials — Council-Guided Development Spec

This document is the authoritative roadmap and constraint list for the Tower of Trials browser RPG. All AI-generated code must adhere to these directives. The game is being developed incrementally, with each phase building on the last. **Never skip phases. Never add features not listed in the current phase.**

---

## 1. Core Game Overview

A solo, text-based, dark-themed tower-climbing RPG played in a single HTML file. The player controls a single Swordsman, ascending floors of increasing difficulty, earning gear and gold, making permanent skill choices, and returning to a Camp hub to prepare.

**Core Loop:** Camp → Enter Tower → Floor Select → Ascend → Die/Retreat → Camp → re-gear/re-shop → re-enter.

---

## 2. Current State (Phase 3 Complete)

- Swordsman class (★1) with skills: Attack, Slash, Guard, Power Strike (unlocked Lv10).
- Tower floors 1–15 fully implemented with monsters (H–D rank), bosses every 5th floor.
- Camp dashboard with Enter Tower, Shop, Bag, Rest at Campfire.
- 5 equipment slots (Weapon, Armor, Helmet, Boots, Accessory) with loot drops and shop.
- 5-slot inventory with potion stacking (max 10) and selling at 20% shop price.
- Lv30 skill branch (Cleave vs. Parry) implemented with permanent choice.
- Item rarity system: Common (white), Uncommon (green), Rare (blue) with small stat bonuses.
- Floor selection with retreat and death penalty (10% gold loss).
- Save/load via localStorage covering all progress.
- UI: dark theme, fixed left stats/equipment panel, single scrollable combat log, bottom action bar.

---

## 3. Council's Overarching Directives

1. **No new classes until the Swordsman is fully tested up to floor 30+ and the Lv60 ultimate skill is implemented.** The Swordsman is the vertical slice. Adding more classes multiplies testing and balance work.
2. **No party system.** The game is a solo climber.
3. **No multiplayer, no PvP, no guilds.** Stay local, single-player, browser-based.
4. **No crafting or complex sub-systems (like Summoner cards) until Phase 7+.** Keep inventory and shop simple.
5. **Balance is priority over content volume.** If a new feature trivializes the game or makes it too grindy, the numbers must be tuned immediately.
6. **UI always follows the player’s minute-to-minute experience.** Keep it clean, readable, and dark. One scrollable log, fixed stats panel.

---

## 4. Phased Development Roadmap

### Phase 4 (NEXT — implement this now)

**Goal:** Reach the Lv60 ultimate skill and floors up to 30. Introduce meaningful endgame chase.

**Features to add:**
1. **Extend tower to floor 30** with C-rank and B-rank monsters and bosses.
   - Floors 16–20: C-rank (Chimera, Stone Golem, Dark Elf Assassin, etc.)
   - Floors 21–25: B-rank (Wyvern, Lich, Armored Behemoth)
   - Floors 26–30: A-rank monsters introduced, culminating in a floor 30 Dragon boss (epic encounter).
2. **Implement Lv60 ultimate skill choice** (permanent, builds on Lv30 branch).
   - Cleave path → Whirlwind (hits all enemies, AoE)
   - Parry path → Riposte (guaranteed counter + stun)
   - Show a skill tree preview on the Camp screen after the choice is available.
3. **Add a simple "Prestige" or "Ascension" teaser:** After beating floor 30, the player sees a message: "The tower's true depths await in a future update. Your journey is far from over." This sets long-term motivation.
4. **Introduce "Set Bonuses"** for rare equipment combos. Phase 4 adds two set pieces that drop from floors 26–30: `Dragonscale Plate` (armor) and `Dragon Helm` (helmet). Equipping both grants +10% fire resist (separate from the +30% stat cap). This rewards collecting specific endgame items.
5. **Add a basic stats page** (accessible from Camp) showing total monsters killed, total gold earned, deaths, etc. Feeds the player's sense of progression.

**Monster/Balance guidelines:**
- C-rank monsters: HP ~800–1200, ATK ~80–120, EXP ~4000–6000.
- B-rank monsters: HP ~1600–2400, ATK ~140–200, EXP ~15,000.
- A-rank dragon: HP 4000, ATK 300, EXP 60,000. This should be a major milestone.
- Ensure the EXP formula (BaseEXP=10, exponent 2.2) keeps levelling challenging but not insane. Players reaching Lv60 by floor 30 should be possible with some grinding.
- Before locking Lv60 as reachable by floor 30, run a spreadsheet EXP-yield pass (per §5 EXP formula) across floors 1–30 to confirm players are not walled under-leveled at floor 26+. If the curve is too steep, permit an overlevel-EXP penalty (reduced EXP when player level >> monster rank) as an exception to the exponent-lock rule — spreadsheet-validated first.

**UI Updates:**
- Skill tree preview in Camp (static image or text tree showing the chosen path).
- Boss intro text for floor 30: "The Dragon awakens..."
- Color-coded rarity for set items (maybe purple for set pieces).

**Do NOT add:**
- New classes
- Summoner or pet system
- Crafting
- Multiplayer features
- Respec tokens (keep choices permanent)

---

### Phase 5 (After Phase 4 complete)

**Goal:** Polish and expand replayability.

**Features:**
1. **New Game Plus mode:** After floor 30, the player can restart with a permanent 10% stat boost (stackable) and higher monster difficulty (Hardcore modifier). This creates infinite replayability without new content.
2. **Achievement system:** 10–15 simple achievements (e.g., "Reach floor 10 without dying", "Kill 100 Goblins") with minor permanent rewards (small stat boosts or cosmetic titles).
3. **Equipment reforge:** Allow spending gold to reroll the rarity bonus on an item (Common→Uncommon→Rare with increasing costs). This is a gold sink.
4. **Daily tower quests:** Simple random objectives ("Kill 3 Ogres today, reward: 200 gold") to encourage varied floor replay.

---

### Phase 6 (Future)

**Goal:** Introduce the first new class, but only after the entire Swordsman experience is polished.

1. Add the **Mage (★1)** class, with its own skill tree and equipment affinity (Magic vs. Physical). This validates the multi-class system.
2. Start on the Summoner (★8) only after at least two base classes are fully implemented, and only with heavy tutorialization.

---

## 5. Balance Rules (Council-Verified)

- **EXP formula:** `EXP needed = 10 × (Level ^ 2.2)`. Rounded to integer. Never change exponent without spreadsheet check across 1–99.
- **HP:** `HP = VIT`. Base VIT 50, +3 per level. At Lv60, HP ≈ 227. That’s enough for high-end content if armor is decent. Do not increase VIT growth without adjusting monster damage.
- **Damage formula:** Player dealt = `max(1, Player ATK - Enemy DEF/2)`. Incoming = `max(1, Enemy ATK - Player DEF/2)`. Keep this simple. Monster special-damage effects (Dragon %max-HP, Dark Elf Assassin dodge/DEF-ignore, Wyvern DEF-ignore, Stone Golem crit-immunity, Lich summons) are explicit exceptions to the "keep simple" rule and may be implemented in Phase 4 using the existing `diveDef`/`fireBreathe`/`multiAttack` patterns already in code.
- **Sell price:** Always 20% of shop buy price. Never increase unless a gold sink is added.
- **Potion drop:** 10% from non-boss monsters. Bosses have separate loot tables.
- **Rarity bonuses:** Common (0%), Uncommon (+10% to one random stat), Rare (+20% one or +10% two). Bonuses apply to base item stats. Total additive bonus per item capped at +30% for ATK/DEF/AGI/CRIT stats (only Rare reaches +20%/+10%). **Set-bonus resistances (e.g., fire resist) are tracked separately and not subject to this cap.** No Legendary/Mythic until Phase 6+. **Epic-tier drops are permitted from B-rank (floors 21–25) and A-rank (floors 26–30) monsters in Phase 4** to support the endgame chase. Epic items use the existing rarity-bonus model scaled up (e.g., +35–50% to one/two stats) and a distinct color (purple).

---

## 6. UI/UX Constitution

- Dark background (#1a1a1a or similar), white/gray text, monospace font for logs.
- Left panel: fixed, no scroll. Shows HP, EXP (with bar), ATK, DEF, gold, potions, equipment list (colored by rarity), skill choice.
- Right panel: sole scrolling combat log. Messages color-coded: damage dealt (yellow), damage taken (red), healing (green), loot (gold), level-up (cyan), system (gray).
- Bottom bar: fixed, with action buttons.
- All overlays (shop, inventory, floor select, camp) should be modals with semi-transparent backgrounds.
- Mobile: stack vertically? Keep same layout but ensure left panel height is limited; use flexbox.

---

## 7. Summary of Council Verdicts (for AI context)

- Star tier permanence: In future multi-class release, all players start at ★1 and unlock higher stars through achievements. Not yet implemented.
- Summoner is a trap without onboarding; defer.
- Crafting failure chance removed; only reforge in Phase 5.
- No separate farming zones; floor replay is the farming loop.
- Inventory limited to 5 slots to avoid hoarding and keep decisions meaningful.
- Camp screen is the emotional hub; always return there after death/retreat.
- Lv30/Lv60 choices are permanent; they define character identity.
- Phase 4 relaxations (verified): Epic drops allowed from B-rank (21–25) and A-rank (26–30) floors; +30% stat cap excludes set-bonus resistances; monster special-damage effects exempt from the "keep simple" rule; set pieces Dragonscale Plate + Dragon Helm added (drop floors 26–30, +10% fire resist); Lv60-by-floor-30 requires an EXP spreadsheet pass with optional overlevel-EXP penalty as a documented exception.

---

## 8. How to Use This Spec with AI

When prompting an AI to modify the game, always include the following preamble:

> "You are working on the Tower of Trials RPG. Read and follow the directives in GAME_COUNCIL_SPEC.md exactly. Do not propose features outside the current phase. Do not add new classes, party systems, multiplayer, or any mechanics not listed in the allowed features for the current phase. Focus solely on the requested implementation."

Update this spec file as each phase is completed, moving the goalposts forward.

---

*Last updated: Phase 3 complete. Proceed to Phase 4.*