# tower-of-trials-rpg

A turn-based browser RPG where the Swordsman climbs the Tower of Trials.

> **Scope note:** This prototype is a small single-player subset of the larger
> design vision in [`game_system_design_doc.md`](./game_system_design_doc.md).
> The doc describes a much bigger multiplayer game (18 classes, ranks H–S,
> 6 equipment slots, guilds/PvP, etc.). The current build implements only:
> **1 class (Swordsman), ranks H–D, 15 floors, 5 equipment slots, 3 rarity
> tiers, and localStorage-only saves.** Known intentional differences from the
> doc: base VIT is 50 (doc suggests 100), base DEF is 8 (doc suggests 10), and
> the EXP exponent is `2.2` (the doc's stated `1.8` was flagged as under-scaled
> and recommends `2.2`–`2.4`). Treat the doc as aspirational reference, not a
> 1:1 spec for this file.

## Features (v5 Update)

### Core Gameplay
- **Turn-based combat** with Attack, Slash, Guard, and Potion actions
- **15 dynamic floors** with escalating difficulty and unique monsters
- **Multi-enemy battles** (30% chance per floor to spawn 2 enemies simultaneously)
- **Equipment system** with 5 slots (weapon, armor, helmet, boots, accessory)
- **Inventory management** with 5-slot bag and potion stacking
- **Shop system** for buying potions and gear
- **Camp hub** for resting and planning between floors
- **Full save/load** with localStorage persistence

### New in v5

#### Floors 11-15
- **Floor 11-12**: E-rank deadly monsters (Wraith, Griffin, Minotaur)
  - Wraith: Immune to physical attacks (requires magic or counters)
  - Griffin: Dive attack ignores 50% DEF
- **Floor 13-14**: D-rank infernal monsters (Ogre Lord, Basilisk, Lesser Wyvern)
  - Ogre Lord: Enrages below 30% HP (+50% ATK)
  - Basilisk: 15% petrify chance (skip turn)
  - Lesser Wyvern: AoE fire breath every 3 turns (1.5x ATK, ignores DEF)
- **Floor 15 Boss**: Chimera (1000 HP, 100 ATK, 42 DEF, 4000 EXP, 300 gold)

#### Level 30 Skill Branch
Player chooses one permanent skill at level 30:
- **A: Cleave** – Hit 2 enemies (1.0x per enemy), or single enemy for 1.3x damage
- **B: Parry** – Guard + auto-counter for 50% ATK on next enemy attack

Choice is permanent and cannot be respecced. Replaces or modifies skill bar accordingly.

#### Rarity System
All equipment drops have random rarity tiers with stat bonuses:
- **Common (70%)**: White text, base stats
- **Uncommon (20%)**: Green text, +10% to one random stat
- **Rare (10%)**: Blue text, +20% to one stat OR +10% to two stats

Items display rarity color in UI. Bonuses are recalculated on load from stored tier.

#### Multi-Enemy Combat
- 30% chance to spawn 2 identical monsters per floor (non-boss only)
- Both monsters attack each turn
- Cleave skill exploits this mechanic (hits both, or powers up against single)
- Monster health displayed per enemy

### Existing Features
- **Exp & leveling** with formula: `EXP = 10 × Level^2.2`
- **Skills**: Attack, Slash (with crit), Guard (30% damage reduction), Power Strike (Lv 10, 1.5x, 3-turn cooldown)
- **Potions**: 30 HP restore, 10% drop rate, stackable (max 10 per slot)
- **Death penalty**: Lose 10% gold, full HP restore, keep progress
- **Monster variety**: H, G, F, E, D rank enemies with increasing stats
- **Boss floors**: Ogre (5), Minotaur (10), Chimera (15) with guaranteed loot

## Save Format

**Version 5** (backward compatible from v3+):
- Player stats, exp, gold, equipment (with rarity), bag (with rarity/bonus)
- Monster combat state (array of active monsters)
- Tower state (floor, phase, unlock list, highest floor reached)
- **Skill choice** (cleave/parry/null)
- **Equipment rarity tiers** for recalculation on load

## Gameplay Tips

- **Floors 1-5**: Grind for basic gear (Leather items, Iron items)
- **Level 10**: Unlock Power Strike (1.5x damage, 3-turn cooldown)
- **Floors 6-10**: Collect Iron-tier gear (Iron Sword, Chainmail, Helm, Greaves)
- **Level 15+**: Recommended for floors 11-12 (E-rank deadly)
- **Level 20+**: Recommended for floors 13-14 (D-rank infernal)
- **Level 25+**: Recommended for floor 15 (Chimera boss)
- **Level 30**: Choose permanent skill branch (Cleave or Parry)
- **Replaying floors**: Earlier floors grant steady EXP for grinding

## Combat Mechanics

### Damage Formula
```
Player damage = (ATK × multiplier) − (Enemy DEF / 2), min 1
Enemy damage = (ATK − Player DEF / 2), min 1
Guard reduces enemy damage by 30%
```

### Turn Order
1. Player action resolves
2. Monster attacks (if alive, not petrified)
3. Monster special effects trigger (enrage, petrify, fire breath)
4. Monster death handling (exp, gold, drops, level up, next monster)
5. Cooldown tick-down

### Special Mechanics
- **Wraith**: Immune to physical (Attack, Slash, Power Strike, normal Cleave)
- **Griffin**: Dive attack ignores 50% DEF
- **Ogre Lord**: Enrages below 30% HP for +50% ATK
- **Basilisk**: 15% petrify chance skips player's next turn
- **Lesser Wyvern**: Fire breath every 3 turns (1.5x ATK, ignores DEF)
- **Parry Counter**: Automatic after Guard (only next attack)

## UI

- **Left Panel**: Health bar, equipped gear (colored by rarity), stats, EXP progress
- **Center**: Combat log with color-coded messages
- **Right Panel**: Floor preview, monster health bars, floor status
- **Bottom**: Action buttons (Attack, Slash, Guard, Potion, Power Strike/Cleave, Ascend, Rest, Push Deeper, Retreat, Shop, Bag)

## File Structure

- **rpg_combat.html**: Single-file game (HTML + CSS + JavaScript)
- **SCHEMA.txt**: Complete game specification
- **README.md**: This file

## How to Play

1. Open `rpg_combat.html` in a modern web browser
2. Click "Enter Tower" to select a floor
3. Choose an unlocked floor (start with Floor 1)
4. Click "Ascend" to enter combat
5. Use Attack, Slash, Guard, or Potion to fight
6. After victory, Rest or Push Deeper
7. Build gear and level up to reach Floor 15

## Browser Compatibility

Requires:
- Modern JavaScript (ES6)
- localStorage (save/load)
- Web Audio API (sound effects)
- Flexbox (layout)

Tested on Chrome, Firefox, Safari (2024+).

## License & Credits

Tower of Trials RPG — Singleplayer text RPG prototype.
All code contained in single HTML file for easy distribution.
