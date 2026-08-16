# tower-of-trials-rpg

A turn-based browser RPG where the Swordsman climbs the Tower of Trials.

## Features (v6 Update)

### Core Gameplay
- **Turn-based combat** with Attack, Slash, Guard, Potion, and Flee actions
- **30 dynamic floors** with escalating difficulty and unique monsters
- **Multi-enemy battles** (scaled chance per floor, 15% max, to spawn 2 enemies simultaneously)
- **Equipment system** with 5 slots (weapon, armor, helmet, boots, accessory)
- **Inventory management** with 5-slot bag and potion stacking
- **Shop system** for buying potions and gear
- **Camp hub** for resting and planning between floors
- **Full save/load** with localStorage persistence
- **Battle stage** with CSS-drawn monster sprites and combat animations (customizable)

### New in v6

#### Difficulty Rebalance
- Multi-enemy spawn chance now scales by floor instead of a flat 30%:
  - Floors 1-4: 8% | Floors 5-9: 10% | Floors 10-14: 12% | Floors 15+: 15% (hard cap)
- Slime splits now remove the dying Slime and spawn a Mini-Slime in its place, and are capped at 3 total monsters — no more 4-on-1 minislime swarms.

#### New Game Plus (optional)
- After clearing floor 30, you may choose **New Game +** — or return to camp and pick it anytime.
- Each NG+ tier gives a **permanent +10% boost to base stats** (stackable).
- Monsters scale too: **+15% HP and +10% ATK per tier**.
- Floor progress resets; adventure stats (kills, deaths, gold earned) carry over.
- NG+ tier shows in the stats panel and camp snapshot.

#### Battle Stage & Sprites
- A visual battle stage sits above the combat log with CSS-drawn placeholder sprites (no emoji).
- Animations: idle bob, attack lunge, hit flash/shake, death fade, spawn pop-in.
- **Fully customizable** — see [Customizing Monster Sprites](#customizing-monster-sprites).

### Existing Features

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
- Scaled chance to spawn 2 identical monsters per battle (non-boss only): 8% (F1-4), 10% (F5-9), 12% (F10-14), 15% (F15+)
- Both monsters attack each turn
- Cleave skill exploits this mechanic (hits both, or powers up against single)
- Monster health displayed per enemy and in the battle stage
- Slimes split into Mini-Slimes on death, but splits are capped at 3 total monsters

### Existing Features
- **Exp & leveling** with formula: `EXP = 10 × Level^2.2`
- **Skills**: Attack, Slash (with crit), Guard (30% damage reduction), Power Strike (Lv 10, 1.5x, 3-turn cooldown)
- **Potions**: 30 HP restore, 10% drop rate, stackable (max 10 per slot)
- **Death penalty**: Lose 10% gold, full HP restore, keep progress
- **Monster variety**: H, G, F, E, D rank enemies with increasing stats
- **Boss floors**: Ogre (5), Minotaur (10), Chimera (15) with guaranteed loot; the Minotaur also drops a Spectral Charm

## Save Format

**Version 6** (backward compatible from v3+):
- Player stats, exp, gold, equipment (with rarity), bag (with rarity/bonus)
- Monster combat state (array of active monsters)
- Tower state (floor, phase, unlock list, highest floor reached, NG+ cleared flag)
- **Skill choice** (cleave/parry/null)
- **Ultimate choice** (cleave:a/b, parry:a/b)
- **NG+ tier** (0 = normal run)
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
- **Wraith floors (11-12)**: Wraiths are immune to physical attacks. The floor-10 Minotaur always drops a Spectral Charm (magic), so you're guaranteed magic access first. If you enter without it anyway, a warning fires and the **Flee** action (available in every battle) returns you to floor selection with no penalty — but no healing either.

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
- **Flee**: Leaves any battle to floor selection — keeps gold and gear, keeps current HP (rest at the campfire to heal)
- **Griffin**: Dive attack ignores 50% DEF
- **Ogre Lord**: Enrages below 30% HP for +50% ATK
- **Basilisk**: 15% petrify chance skips player's next turn
- **Lesser Wyvern**: Fire breath every 3 turns (1.5x ATK, ignores DEF)
- **Parry Counter**: Automatic after Guard (only next attack)

## UI

- **Left Panel**: Health bar, equipped gear (colored by rarity), stats, EXP progress, NG+ tier
- **Center**: Battle stage (monster sprites with mini HP bars), combat log with color-coded messages
- **Right Panel**: Floor preview, monster health bars, floor status
- **Bottom**: Action buttons (Attack, Slash, Guard, Potion, Power Strike/Cleave, Ascend, Rest, Push Deeper, Retreat, Shop, Bag)

## Customizing Monster Sprites

The battle stage sprites are placeholder CSS shapes — designed to be swapped out for your own art. Everything lives in `rpg_combat.html`.

### How it works
- Each monster's look is **one CSS class** (`.sprite-slime`, `.sprite-goblin`, ...) plus optional animation classes.
- The `SPRITES` object in the JavaScript maps each `templateKey` to its base class and effects:
  ```js
  const SPRITES = {
    slime: { base: 'sprite-slime', split: 'fx-spawn' },
    ancientDragon: { base: 'sprite-dragon' },
    ...
  };
  ```
- `renderBattleStage()` builds one `.monster-card` per monster (sprite + name + mini HP bar) and the combat code calls `animateMonster(index, 'fx-hit')` etc. — animations are pure CSS classes, no rendering logic to touch.

### Quick changes (recommended start)
1. Open `rpg_combat.html` and find the block marked `CUSTOM SPRITE OVERRIDES` at the end of the `<style>` section.
2. Override any sprite there, e.g.:
   ```css
   .sprite-slime {
     background: url('my-slime-art.png') center/contain no-repeat;
     border-radius: 0; box-shadow: none;
   }
   ```
   Later rules win, so your overrides beat the defaults.
3. To swap the look for an entire monster, change its `base` in `SPRITES` — e.g. `'sprite-dragon'` for a recolored slime — or point it at a brand-new class you define.

### Using your own animation/frames
- **Static PNG/GIF**: use `background-image` as above. Animated GIFs play automatically in the browser.
- **Sprite sheets**: define a keyframe using `steps()`:
  ```css
  @keyframes my-slime-walk {
    from { background-position: 0 0; }
    to { background-position: -160px 0; }
  }
  .sprite-slime { animation: my-slime-walk 0.8s steps(4) infinite; }
  ```
  (In this game, `steps(N)` should equal your frame count minus 1.)
- **Custom FX**: the combat code triggers classes like `fx-hit`, `fx-spawn`, `fx-death` on the `.monster-card`, and `anim-attack` on the card. Redefine the matching `@keyframes` (or the `.monster-card.<fx> .sprite` rules) in the overrides block.
- **Per-monster attack animation**: give that monster's card its own rule, e.g.
  ```css
  .monster-card.anim-attack .sprite-slime { animation: my-slime-bite 0.35s; }
  ```
- If you build a full sprite-sheet system later, you only need to replace `renderBattleStage()` and the `SPRITES` config — the combat logic never touches the stage directly.

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
