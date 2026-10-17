# Runes of Yggdrasil

**A Norse-inspired turn-based tactics game about summoning creatures, breaking runic arrows, and capturing territory.**

> **Game design reference · v0.1 · October 2026**  
> This document describes the agreed gameplay and identifies provisional prototype rules and unresolved decisions. It is intended to be the gameplay source of truth for future development. Newer rules here override older brainstorming notes.

**Technical implementation:** [Unity 6 / URP development plan](docs/UNITY_TECH_STACK.md). This document is the source of truth for gameplay rules; the technical plan defines the engine, asset workflow, architecture, AI-assisted development, and mobile roadmap.

## 1. The core concept

Two rival **Summoners** conjure mythological creatures onto a grid battlefield. Every creature has **directional arrows** that simultaneously determine its **movement**, **attack directions**, **territory-capture reach**, and **survival**. Combat first wears down **armor**; when armor reaches zero, one arrow breaks and armor resets. Losing an arrow permanently reduces a creature's capabilities until a spell or ability explicitly restores it. Losing the final arrow kills the creature.

Players spend a fixed **action-point (AP)** budget on summoning, movement, rotation, attacks, capture, and eventually spells. Each Summoner has limited total **creature capacity**, creating a choice between numerous smaller units and a few powerful ones. The win condition is **cumulative points from owned tiles**, not wiping out an opposing army.

**Design priorities:** clear and predictable rules; meaningful movement and facing; damage visibly changing units; deliberate attack-versus-capture decisions; and no runaway economy from early territorial control. There are no hidden hit rolls or conventional attack/defense stat comparisons.

### Status of rules

- **Agreed:** A rule we explicitly selected and want preserved.
- **Prototype default:** An implementation-ready starting value or resolution not yet fully playtested.
- **Open:** An important choice that has not been finalized.
- **Deferred:** An idea intentionally excluded from the initial playable build.

## 2. Theme and creature identities

The setting is Norse-inspired mythic fantasy. Summoners channel the runes of Yggdrasil to call forest spirits, spectral animals, draugr, trolls, giants, and legendary beasts. It is **not** a historical Viking simulation.

The aesthetic features carved wood and stone, glowing runes, ritual circles, forests, frost, auroras, and mystic light. Units should have instantly legible arrow positions, clear damage states, and recognizable silhouettes. A destroyed arrow should visibly shatter or go dark. Captured tiles should change color or acquire the controlling Summoner's rune effect. Decorative detail must never obscure the tactical board.

Creature cost bands are **agreed conceptually**, while individual creature stats/abilities remain **open**:

| Capacity | Creature examples | Intended tradeoff |
| --- | --- | --- |
| 1–3, small | Sprites, ravens, wisps, woodland creatures | Numerous, useful for territory coverage |
| 4–7, medium | Spirit wolves, bears, boars, draugr, trolls | Flexible battlefield presence |
| 8–10, large | Jötnar, dragons, legendary beasts | Powerful, but much less numerous |

## 3. Game modes and starting a match

**First prototype:** single-player skirmish. The player selects a Summoner; the opponent is a **randomly selected Summoner controlled by AI**. Both use the same core rules. Start with a simple selectable roster; unique Summoner bonuses, deckbuilding, card drawing, and elaborate factions are **not decided**.

**Long-term vision:** asynchronous browser multiplayer with optional live games, player profiles, replays, and rankings. The first skirmish does not require accounts or a server.

A match begins on a grid battlefield with opposite starting sides. Most tiles are neutral, but **each player must begin with a small area of already-owned tiles** because creatures can only be summoned on owned tiles. The exact starting positions and number of starter tiles are **open**; use symmetrical layouts initially.

**Prototype map:** 7×7 tiles; 6×6 or 8×8 may be tested later. One creature occupies at most one tile. Special terrain, diagonal tiles, and multi-tile giant units are **deferred**.

Each tile has exactly one ownership state: **neutral, Player A, or Player B**. A tile's owner is independent of the creature currently standing there. Standing on a tile does **not** automatically capture it.

## 4. Capacity and action points

### Creature capacity

**Agreed:** Each Summoner has **10 total capacity** for active summoned creatures, not a fixed five-creature limit. Every unit consumes capacity according to its cost.

Valid examples: five 2-cost sprites; a 7-cost creature plus a 3-cost creature; or one 10-cost giant.

**Prototype default:** Occupied capacity is released when a creature dies and leaves the board. A player may summon again as long as sufficient capacity and AP are available.

### Action points (AP)

**Agreed direction, prototype figures:** Players have a deterministic **10 AP per turn**. Territory does **not** increase AP income. AP costs:

| Action | AP cost |
| --- | ---: |
| Summon creature | Equal to the creature's capacity cost |
| Move one tile | 1 |
| Rotate 90° | 1 |
| Initiate attack | 1 |
| Capture tiles | 1 |
| Cast spell / activate ability | Listed cost, not yet finalized |

**Prototype default:** AP refreshes to 10 at the start of each player's turn, unused AP does not carry over, and a player may end their turn early. Multiple different creatures may act in one turn.

Capacity governs **how much army is deployed**; AP governs **what happens this turn**. Territory is for **scoring and summoning locations**, not extra AP.

## 5. Arrows, movement, and rotation

**Agreed:** An intact arrow provides movement, attacking, and capture reach in its direction, and is also one segment of a creature's life.

**Initial prototype:** four cardinal directions (**North, East, South, West**). Earlier eight-direction patterns, diagonal arrows, and extra arrow slots are **deferred**.

For example, a wolf with N/E/W arrows may move, attack, or capture in those three directions but not South. If its E arrow breaks, that direction is no longer available. It still has all other intact arrows.

- A creature **dies immediately when it loses its last arrow**.
- Lost arrows **do not regenerate naturally**. Only specified spells or abilities may restore them.
- Arrow destruction **does not remove ownership of tiles captured earlier**.
- Intact arrows are **capture permissions**, not a passive aura that automatically flips or maintains tiles.

### Movement

**Agreed:** Units only move in directions of their current intact arrows. Each tile moved costs 1 AP.

**Prototype default:** Each step goes to one empty orthogonally adjacent tile; a creature can move multiple steps if each step is legal and paid for. Units cannot share a tile or pass through an occupied tile. Movement into neutral or enemy-owned **empty** tiles is allowed and **does not change tile ownership**.

### Rotation

**Agreed:** Rotating a creature **90° costs 1 AP**. All remaining arrows change their facing together. Missing arrows stay missing. Thus a creature with only N can rotate clockwise to have only E.

Movement and rotation can happen before a creature attacks or captures. They do not, by themselves, exhaust the creature.

## 6. Exhaustion: one decisive action per creature

**Agreed, latest rule:** A creature becomes **exhausted** when it **attacks once** or performs **one Capture action**. That creature cannot take another voluntary action for the rest of its Summoner's turn: no more movement, rotation, attack, or capture. A creature therefore chooses **fight OR claim land**, not both.

- A creature **may move and rotate first**, as AP permits.
- **Newly summoned creatures begin exhausted**. They cannot immediately move, attack, or capture.
- Exhaustion ends at the **start of that creature owner's next turn**.
- An exhausted creature can **still defend and counterattack normally** when attacked. This avoids making acting first an automatic defensive penalty.
- **Prototype default:** Automatic counterattacks do not consume AP or change a creature's voluntary exhaustion state. Spells cast by the Summoner are separate actions unless a spell says otherwise.

Example: A wolf moves twice (2 AP), rotates (1 AP), and captures tiles (1 AP). It is exhausted even though its Summoner has 6 AP left for other units.

## 7. Summoning onto owned territory

**Agreed, latest rule:** A creature may only be summoned onto an **empty tile currently owned by its Summoner**. The Summoner must have enough unused capacity and AP. Pay AP equal to the creature's capacity cost; the unit occupies the tile and starts exhausted.

**Unrestricted forward summoning is intentional:** Any empty owned tile is valid, **even deep in enemy territory or beside an enemy creature**. There is no proximity restriction, no fixed home deployment zone once the game begins, and no requirement for a contiguous path of owned tiles.

Example: A small sprite captures an isolated tile near the enemy side. On a later turn, the Summoner could summon an 8-cost giant there if the tile remains owned and empty, and 8 AP and 8 capacity are available.

**Not a rule:** Requiring newly captured tiles to be adjacent to already-owned tiles was considered, but **do not add it yet**. Same for restrictions on summoning close to enemies. Test unrestricted play first.

## 8. Combat: when can creatures attack?

**Agreed:** Combat is a deliberate action, **not** an automatic result of arrow contact. An attacking creature must be directly adjacent to an enemy and have an intact arrow pointing toward it. Initiating an attack costs 1 AP and exhausts the attacker.

Three situations:

1. **A points to B; B does not point back:** A can attack. B takes damage and cannot counterattack.
2. **A and B point toward each other:** A can attack. If B survives and retains an arrow pointing back, it counterattacks.
3. **A does not point to B; B points toward A:** A cannot initiate an attack. B may attack on its own turn.

An exhausted defender may still counterattack. Attacks never automatically capture territory, make the attacker advance, or convert ownership of an enemy creature.

**Prototype combat order:** (1) validate adjacent target, ownership, arrow, AP, and exhaustion; (2) spend 1 AP and exhaust attacker; (3) calculate and apply attack damage; (4) if defender survives and has a return arrow, resolve exactly one automatic counterattack; (5) finish. Reactive spells and alternative counterattack timing are **open**.

## 9. Armor and arrow destruction

**Agreed:** Armor is the game's single core numerical durability stat. Every creature has **maximum armor** and **current armor**. A hit reduces current armor but does **not** break an arrow unless armor is reduced to zero or less.

When that threshold is crossed:

1. Destroy **one intact arrow** on the defender, determined by the **attacking creature's combat style**.
2. Reset defender armor to **full maximum**.
3. Apply the new missing-arrow movement/attack/capture restrictions immediately.
4. If no arrows remain, the creature dies and its capacity is freed.

**Prototype default:** A single hit destroys **at most one arrow**. Excess damage is discarded rather than carrying over into the freshly reset armor. This detail is subject to playtesting.

**Example:** A bear has max/current armor 5 and four arrows. Three neutral hits deal 2 damage each:

| Hit | Result | Arrow lost? |
| --- | --- | --- |
| First (2) | Armor 5 → 3 | No |
| Second (2) | Armor 3 → 1 | No |
| Third (2) | Armor 1 → 0; reset to 5 | **Yes, one** |

The bear remains alive but has fewer legal movement, attack, and capture directions. A spell may restore an arrow only if its effect explicitly says so.

## 10. Combat styles: matchups and arrow selection

**Agreed:** Each creature has exactly one of three combat styles:

- **Berserker beats Controller.**
- **Controller beats Tactician.**
- **Tactician beats Berserker.**

The attacker's matchup determines predictable **armor damage**:

| Matchup | Damage |
| --- | ---: |
| Advantage | **4** |
| Neutral (same style) | **2** |
| Disadvantage | **1** |

Advantage never guarantees an instant kill. A weaker style can still damage and defeat any opponent through sustained attacks and positioning.

**The style of the attacking creature** (including when counterattacking) also determines which of the target's arrows breaks:

| Style | Arrow-breaking rule |
| --- | --- |
| **Controller** | The attacker chooses **which remaining intact enemy arrow** to break. |
| **Berserker** | One remaining intact enemy arrow is selected **randomly**. |
| **Tactician** | Break the first available arrow in **North → East → South → West** order, skipping absent arrows. |

**Prototype default:** Tactician order refers to the target's **current world-facing** directions after rotation. Controller selection requires an actual UI choice. Berserker's arrow selection is the **only planned randomness in baseline combat**. No hidden hit chances, unshown damage variance, or extra attack/defense stats.

## 11. Capturing territory: an active action

**Agreed, latest revision; this supersedes earlier passive influence drafts.**

A non-exhausted creature may spend **1 AP** to perform **Capture**. It then attempts to claim **every adjacent tile toward which it has an intact arrow, simultaneously**. Each eligible tile becomes owned by its Summoner, whether previously neutral or enemy-owned. The creature is then **exhausted**.

**A tile occupied by an enemy creature cannot be captured.** The enemy must move away or be defeated before the tile can be captured in a **later Capture action**. Enemy occupation blocks **that tile only**, not all other directions of the capturing creature.

- A single Capture action can claim **multiple tiles** for the same 1 AP cost.
- Arrows pointing off the map produce no capture target.
- Enemy-occupied tiles are skipped; other legal targets are captured normally.
- Only **enemy** creatures block capture. **Prototype default:** Friendly-occupied tiles can be captured.
- Capturing an already-owned tile has no additional effect.
- A creature does **not** automatically claim the tile under its own feet: Capture reaches **adjacent arrow-pointed tiles**.
- A creature need not start on friendly territory and **does not need to capture adjacent to previously owned land**.
- **Prototype default:** If no eligible tile would change ownership, disable Capture rather than spending AP uselessly.

**Example:** A wolf has N/E/S/W arrows and chooses Capture. The North tile is neutral, East is empty enemy territory, South has an enemy bear, and West contains a friendly creature on neutral ground. One 1-AP Capture claims **North, East, and West**, but not South. The wolf is then exhausted. Even if the bear is killed later, South does not flip until another Capture action reaches it.

### Ownership is persistent

**Agreed:** Once a tile is captured, it stays owned until an opponent actively recaptures it. A creature moving, rotating, dying, or losing an arrow **does not** retroactively unclaim previous captures. Merely occupying an opponent-owned tile does not change its owner. **There is no passive per-frame influence recomputation, ongoing aura contest, or automatic territory flip.**

An owned tile can immediately become a legal summoning location if empty and the Summoner can pay its costs. Unrestricted, disconnected forward summoning is currently allowed.

## 12. Spells and abilities

Spells are part of the intended game and consume AP, but exact cards and costs are **not finalized**. Possible effects include:

- Restore a destroyed arrow to a living unit.
- Shield a specific arrow from destruction.
- Deal armor damage.
- Rotate or reposition allies or enemies.
- Push, pull, or temporarily disable a directional ability.
- Disrupt territory or unit capture abilities.

Abilities should modify existing mechanics rather than add a large parallel system of stats. There is **no automatic healing**. A small fixed spell set can be added after the basic combat/capture loop works.

## 13. Scoring and victory

**Agreed:** Players earn **cumulative victory points** based on **currently owned tiles**. Territory never generates extra AP. The goal is not annihilation and not instantly owning 51% of the map.

**Prototype defaults:** At the **end of each full round** (after both Summoners have completed turns), each player gains **1 point per currently owned tile**. Neutral tiles score zero. The **first to 100 cumulative points** wins, pending balance testing.

Example: Blue owns 17 tiles and Red 12 at the end of a round. Blue gains 17 points, Red gains 12, and both players still get the same fixed AP next turn.

**Open:** Tie resolution if both pass 100 together, stalemate/turn limits, and player-order balance. Do not quietly invent ranked-game tie breakers.

## 14. A sample turn

1. **Start:** Blue has 10 AP. Its creatures recover from exhaustion; it has some remaining capacity.
2. **Move:** Blue moves its wolf to a legal empty adjacent tile for 1 AP.
3. **Rotate:** Blue rotates the wolf clockwise to face more useful tiles for 1 AP.
4. **Capture:** Blue spends 1 AP to capture all eligible adjacent tiles indicated by the wolf's surviving arrows. An enemy-occupied tile remains protected. The wolf is **exhausted**.
5. **Summon:** One newly captured empty tile is now friendly territory. If Blue has enough unused capacity, it may spend 4 AP to summon a 4-cost creature directly there. That creature enters **exhausted**.
6. **Other units act:** Blue can spend remaining AP moving, rotating, attacking, or capturing with **different non-exhausted creatures**.
7. **End:** Red starts its turn with the same 10 AP budget and plays by the same rules.
8. **Score:** After Red finishes, both gain points for their owned tiles. If the score target is met, apply the eventual agreed win/tie rule; otherwise begin another round.

The central choice each turn: **expand your army, reposition it, damage the enemy, or commit creatures to capturing territory**.

## 15. AI and UX requirements for skirmish

The AI must use the same AP, capacity, summoning restrictions, arrow patterns, exhaustion, combat, and capture rules as the player. Start with simple legal-action generation and weighted evaluation; give value to tile gains, good forward summons, favorable attacks, avoiding counterattacks, preserving critical arrows, and threatening the opponent's scoring.

The UI should always show current player, turn, AP, used/total capacity, scores, tile owners, creature arrows (including missing ones), current/max armor, combat style, and exhaustion. Selecting a creature should highlight legal movement, attack targets, and all eligible Capture tiles. Previews should show expected **1/2/4 armor damage**, arrow-break possibility, and whether a defender can counterattack. Controller arrow selection must be player-controlled; Berserker randomness should be explicit.

Mobile-first controls should support touch without hover or tiny hotspots.

## 16. Deliberate non-rules and open decisions

| Topic | Status | Guidance |
| --- | --- | --- |
| Passive influence automatically capturing tiles | **Replaced** | Only the **Capture action** changes ownership. |
| Only capturing beside already-owned land | **Deferred** | Do **not** implement yet. |
| Restrictions on forward summoning near enemies | **Deferred** | Do **not** implement yet. |
| Taking control of enemy creatures after combat | **Rejected for baseline** | Kill removes units; ownership capture concerns **tiles**. |
| Chain combos | **Open/deferred** | Desired as an idea, but no finalized trigger or effect. Do not improvise automatic chain flips. |
| Diagonal arrows / eight-direction boards | **Deferred** | Begin with cardinal directions. |
| Excess damage beyond an armor break | **Prototype default** | Max one arrow per hit; discard overflow. |
| Counterattack timing | **Prototype default** | Defender only counters after surviving attack and still pointing back. |
| Unit cards, hands, deck draw, roster replenishment | **Open** | Start with a selectable pool. |
| Unique Summoner passives and faction rules | **Open** | Do not assume mechanical differences yet. |
| Map size, pre-owned tile locations, first-player advantage | **Prototype/open** | Start 7×7 with symmetric owned launch zones. |
| Exact unit list, armor/capacity/arrows, movement range | **Open** | Examples are not final cards. |
| Spells, shields, buffs, targeting, ability AP costs | **Open** | Build only explicitly specified effects. |
| Match length, ties, stalemates | **Open** | Test target 100; decide tie-breaker before ranked play. |
| Multi-tile giants, terrain, special tiles | **Deferred** | Keep the initial board simple. |

### Questions for playtesting

- Can swarms overwhelm large creatures by covering too much ground? Is a giant worth 8–10 capacity?
- Is 1 AP to capture *all* eligible arrow-facing tiles too efficient?
- Is unrestricted deep-territory summoning tactical or oppressive?
- Does exhaustion make the attack-versus-capture choice genuinely interesting?
- Does armor-reset arrow destruction create enough attrition without dragging matches out?
- Is the target score and board size appropriate for short skirmishes?
- What specific, constrained combo mechanic would be fun without an unfair cascade?

## 17. Prototype acceptance checklist

- [ ] Select a Summoner, assign a random AI-controlled opponent, and start with predefined territory.
- [ ] Summon only on an **empty owned tile** with sufficient AP and free capacity.
- [ ] Allow forward summoning on isolated owned tiles, **including beside enemies**.
- [ ] Newly summoned creatures begin exhausted; death releases capacity.
- [ ] Move and attack only in directions of intact arrows. Rotation changes facing but never heals arrows.
- [ ] Attack costs AP and exhausts the attacker; a defender can counterattack only if alive and pointing back.
- [ ] Non-lethal armor damage does **not** break arrows; crossing zero breaks exactly one arrow and resets armor.
- [ ] Apply combat style damage (1 / 2 / 4), Controller choice, Berserker random selection, and Tactician fixed selection.
- [ ] Kill units when their last arrow breaks.
- [ ] Capture spends **1 AP**, flips **all eligible arrow-directed adjacent tiles**, and exhausts the creature.
- [ ] Enemy-occupied tiles are not captured; other eligible tiles still are.
- [ ] Ownership persists when creatures move, rotate, die, or lose arrows.
- [ ] Capture does **not** require already-owned adjacent territory.
- [ ] No passive auto-capture or AP income from territory.
- [ ] Score current owned tiles at round end and detect the victory threshold.
- [ ] AI follows identical legality checks and resource limits.

---

### Source-of-truth note

**This README reflects the newest agreed design.** Never silently substitute previously discussed passive influence, enemy-monster conversion, automatic arrow recovery, unrestricted spawning on neutral tiles, or territory-generated AP. When an item is marked *open*, log a design decision before building it as a permanent rule.

**Every arrow matters. Every tile matters.**