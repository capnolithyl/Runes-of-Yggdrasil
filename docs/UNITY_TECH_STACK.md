# Unity Technical Plan

> **Runes of Yggdrasil · Technology decision · 2026-10-16**
>
> **Decision: Unity 6 + Universal Render Pipeline (URP) + C#**, targeting a visually rich 3D, angled top-down tactical game.
>
> **First playable:** offline single-player skirmish against an AI-controlled, randomly selected Summoner.
>
> This document describes **how we intend to build the game**, not new gameplay rules. The repository's [README](../README.md) remains the authoritative source for summoning, arrows, armor, exhaustion, capture, scoring, and unresolved game-design questions.

## 1. Why Unity

We want:

- **Actual 3D models** for creatures, ruins, forest scenery, ritual stones, and other environment pieces.
- A **top-down view at an angle** with the clarity of a tactical board and the atmosphere of a detailed fantasy diorama.
- **Minimal hand-modeling**: use licensed ready-made, rigged, animated assets where possible.
- **AI-assisted development ("vibe coding")**: spend most development time directing a coding agent and testing results rather than building every system manually.
- A practical route to **Android and iOS**, with desktop/editor builds for fast iteration.
- A codebase that can eventually support online multiplayer without throwing away game rules.

Unity is a good middle ground: a mature 3D renderer, major asset ecosystem, built-in mobile pipelines, C# gameplay scripting, and growing official tools to connect coding agents to the Editor.

### Considered alternatives

| Option | Reason not selected for the initial build |
| --- | --- |
| **Unreal Engine** | Powerful, but heavier editor/build workflow and more production complexity than a compact 7×7 tactics game needs. |
| **Godot** | Attractive open-source/code-friendly option, but a smaller ready-made 3D asset ecosystem and less direct Unity-style asset/editor integration for this project. |
| **PlayCanvas / TypeScript** | Excellent browser-first and VS Code workflow, but the current preference is polished native mobile 3D and access to Unity's asset ecosystem. |
| **Phaser / 2D** | Suitable for a rules-only or 2D prototype but no longer the planned main 3D game frontend. |

**Decision status:** Unity is **chosen**. The alternatives remain fallbacks, not parallel implementation targets. Do not maintain multiple versions of the game engine prematurely.

## 2. Selected stack and provisional conventions

| Layer | Choice | Status / rationale |
| --- | --- | --- |
| **Game engine** | **Unity 6**, using a supported release and pinning its exact Editor version when the project is created | **Chosen** |
| **Rendering** | **URP**, not HDRP | **Chosen**: scalable graphics for mobile and desktop |
| **Gameplay programming** | **C#** | **Chosen**: primary runtime and rule implementation |
| **Core game logic** | Headless, testable C# rules layer with minimal/no reliance on UnityEngine objects | **Architectural decision** |
| **Unit and spell authoring** | ScriptableObjects for **static definitions**; plain C# data for live match state | **Recommended** |
| **3D representation** | Prefabs with meshes, materials, Animator components and optional VFX | **Recommended** |
| **Input** | Unity Input System with mouse and touch input using the same game commands | **Recommended** |
| **Runtime UI** | Start with **UI Toolkit** for menus/HUD if it meets requirements; uGUI if specific world-space/UI needs make it simpler | **Prototype choice** |
| **Camera** | Orthographic, tilted approximately **40–55°** downward, fixed initial framing | **Visual starting point**, tune during testing |
| **Development tools** | Unity Hub + Unity Editor + VS Code or another C# IDE + GitHub + Codex | **Recommended** |
| **Agent integration** | Unity's official coding-agent plugin and/or Unity CLI/MCP Editor integration, where available | **Optional accelerator**, requires setup and capability review |
| **Tests** | Unity Test Framework: EditMode tests for rules, PlayMode/integration tests for interaction | **Recommended** |
| **Storage** | Local settings/save files; no account/server in the MVP | **Prototype decision** |
| **Initial platform** | Windows desktop/editor for iteration; **Android test builds** next; iOS later | **Delivery sequence** |

A compatible, stable Unity 6 release should be pinned in the repository's eventual \`ProjectSettings/ProjectVersion.txt\`. **Do not automatically jump to the newest major Unity release** or swap render pipelines mid-project.

### Cost / availability notes

- **Unity Personal** is currently free under Unity's published eligibility conditions (in 2026, up to **$200,000** in annual revenue/funding); re-check licensing before release.
- Core coding with C# and an existing third-party coding agent does **not** require purchasing every optional Unity AI feature.
- Unity's **in-editor AI assistant and some generative features may use trials, credits, or paid plans**. Do not assume unlimited use.
- Third-party art, plugins, cloud builds, and app-store accounts can cost money independently of the Unity license.

## 3. Visual direction: a 3D tactical diorama

The battlefield should look like a compact physical/magical tabletop built in a Norse forest:

- **7×7 logical grid** for the first prototype; board size remains a gameplay tuning value.
- Stone or rune-etched tiles, subtle height variation, moss, pine trees, carved pillars, ruins, distant fog, snow or aurora as appropriate.
- A **fixed angled top-down orthographic camera**. Players should see most or all of the board clearly without constant camera work.
- Use **URP** lighting, fog, emissive rune materials, lightweight effects, shadows, and modest post-processing.
- Blue/orange (or other deliberately contrasted) **territory ownership** communicated on the tile surface, not just by ambient light.
- Units should have **clear silhouettes** and not block selection of neighboring tiles.
- Distinct magical feedback for **summoning, movement, attacks, armor loss, arrow shattering, capture, exhaustion, death, and victory**.

**Gameplay readability overrides realism.** The user must always be able to identify a creature's arrows, missing arrows, current armor, combat style, owner, and exhaustion state despite the camera angle and fancy materials.

### Crucial rendering decision: arrows are gameplay UI

Do **not** depend on a wolf mesh literally facing north to represent which arrows it has. The creature's decorative facing and the game's arrow directions are different concerns.

Render the creature's intact arrows as world-space rune markers, rings, glyphs, or tile-edge indicators that:

1. Stay aligned to the **logical North/East/South/West directions**.
2. Are readable from the angled camera.
3. Rotate when the unit rotates.
4. Visibly shatter or disappear when destroyed.
5. Can be highlighted independently for movement, attack, and capture previews.

The **model is the fantasy**; the **arrow overlay is the authoritative information**.

**Researched candidate asset packs and price snapshots:** [Nordic / Fantasy / Winter Asset Research](ASSET_RESEARCH.md). All prices and license terms should be rechecked before purchase.

## 4. Avoid the modeling trap: asset-first production

**The initial prototype does not require any custom creature modeling.**

Preferred workflow:

1. Find a **cohesive visual style**, ideally stylized/high-quality fantasy rather than mismatched photorealism and low-poly cartoon assets.
2. Source ready-made **animated** creatures (wolves, birds, bears, undead, giants) and environment kits.
3. Verify each model has compatible file formats, rigs/animations, materials, and license terms.
4. Import one asset into Unity and prove it can **idle, move, attack, take damage, and die**.
5. Turn the imported model into a reusable **creature prefab** connected to the independent rules system.
6. Use **materials and effects** to unify the art style: glowing runic symbols, spirit translucency, custom colors, outlines, particle effects, and summoning circles.
7. Commission/model/generate something bespoke **only when a core creature cannot be convincingly represented with sourced assets**.

Possible sources to evaluate individually:

- [Unity Asset Store](https://assetstore.unity.com/)
- [Fab](https://www.fab.com/)
- [Quaternius](https://quaternius.com/)
- [KayKit](https://kaylousberg.itch.io/)

**Do not assume any specific asset pack is approved or acquired.** Asset selection is a separate task; audit licensing and quality before import.

### Legal and repository precautions

- The GitHub repository is **public**. Do **not** commit commercially licensed raw asset packages/models to a public repo unless the license **explicitly permits redistribution of those source assets**.
- Unity Asset Store licenses commonly permit incorporating assets into a finished game, **not** giving out extractable raw assets in a public source repository.
- Keep an \`ASSET_SOURCES.md\` inventory (to be created when assets are chosen): filename/pack, creator, store URL, license, purchase record where relevant, permitted uses, and attribution.
- For unredistributable assets, use local-only storage and document the import/setup steps. A private assets repository is an option only if rights allow collaborators to access it.
- Do not feed third-party asset files into a model-training workflow without the rights to do so.
- Prefer CC0/open-license placeholder meshes in the public prototype wherever possible.

### Animation plan

The prototype should not rely on hand-crafted spritesheets. Use rigged 3D meshes with short animation sets: **Idle, Move, Attack, Hit, Death**, plus optional Summon/Defend. If an asset only provides idle/walk, use simple temporary timing/VFX and replace later.

A wolf, a raven, and one humanoid enemy are enough to validate different model/animation types. **Avoid populating an eight-creature roster before one full match works.**

## 5. Architecture: the rules never depend on the graphics

This is the single most important software constraint.

Build the rules as a **pure, headless C# domain layer** that could run in tests or, later, inside a server. Unity scene objects **visualize** state; they do **not own** game truth.

### Recommended layering

| Layer | Responsibility | Examples |
| --- | --- | --- |
| **Core rules/domain** | All game state, action validation, changes, deterministic outcomes, scoring | Board, tiles, players, units, armor, arrows, AP, capacity, turn state, capture, exhaustion |
| **Commands / game application** | Convert player or AI intents into validated actions and state transitions | \`Summon\`, \`Move\`, \`Rotate\`, \`Attack\`, \`Capture\`, \`EndTurn\` |
| **AI** | Evaluate **legal** commands produced by the same rules engine | Territory gain, counterattack danger, advantageous attacks, deployment choices |
| **Unity presentation** | Position 3D prefabs, draw tile owners/arrows, play effects/audio/animations | \`BoardView\`, \`UnitView\`, \`ArrowView\`, \`CombatVFX\` |
| **UI/input** | Human selections, action previews, button/touch interactions | Summoner selection, HUD, unit detail, legal destination highlights |
| **Persistence/platform** | Save/load and build/platform integrations | Local save, settings, Android/iOS builds |

**Never implement the real rules twice** (once in a UI button and again in an AI script). Both player and AI submit the same command types to the same validator/resolver.

### Model the state explicitly

Recommended state concepts:

- \`MatchState\`: turn/round, active player, scores, board, units, AP, seed/history.
- \`BoardState\`: tile ownership + occupancy as independent attributes.
- \`UnitState\`: id, owner, grid coordinate, **intact arrow directions**, max/current armor, combat style, exhaustion, capacity cost.
- \`SummonerState\`: id, roster/available definitions, currently used capacity, current AP.
- \`ActionCommand\`: explicit, serializable player/AI intent.
- \`ActionResult\`: legality, new state and a list of presentation events (e.g. moved, arrow destroyed, tile captured, unit died).

Use grid integer coordinates rather than floats for all tactical decisions. The **XZ plane** can represent the board in Unity, with Y used only for height and visual effects. For example, grid \`(x,z)\` maps to world \`(x * tileSize, 0, z * tileSize)\`. Define one canonical mapping between world-space rotation and North/East/South/West and test it.

**Avoid physics-driven gameplay.** Raycasting may identify which 3D tile was tapped, but game-state validation, occupancy, movement, and targeting are computed from the logical grid, not colliders or the actual positions of meshes.

### Unit definitions versus unit instances

- Store **authored static data** (name, capacity cost, max armor, style, base arrows, prefab reference, audiovisual hints) in **ScriptableObjects** or another inspectable data format.
- Store each **live unit's changing state** (armor, arrows, owner, grid position, exhaustion) in plain serializable C# objects.
- A unit's actual health/arrows must **not** be maintained only in an Animator, UI script, or ScriptableObject asset.
- Keep code data-driven: a new wolf or troll should require authoring a definition and prefab, not copy/pasting a special-case combat class.

### Determinism and reproducibility

- Combat resolution is rule-driven: no hidden hit rolls.
- **Berserker's random arrow loss** is the explicitly allowed randomness. Use a **seeded, state-owned random source** and record the selection in action results for replays/debugging.
- If the AI chooses a random Summoner, that choice is made at **match creation** and recorded in the match setup/seed.
- Build an append-only **action/event log** suitable for deterministic replay and eventually authoritative online play.
- Animations must not be able to change the result of an action; they play **after the command has been resolved**.

**Future multiplayer note:** Unity is the first client's presentation and local rule execution environment, not the permanent security authority. A server-authoritative architecture would reuse or port the independent rule logic and validate every move. We are **not building multiplayer now**.

## 6. Project layout and GitHub workflow

**Recommendation:** Put the Unity project at the **repository root** so Unity Hub opens this repository directly:

~~~text
Runes-of-Yggdrasil/
├── README.md                 # Game design (authoritative rules)
├── docs/
│   └── UNITY_TECH_STACK.md   # This file
├── Assets/
│   └── Runes/
│       ├── Art/
│       ├── Audio/
│       ├── Data/
│       ├── Prefabs/
│       ├── Scenes/
│       │   ├── Boot.unity
│       │   ├── SummonerSelect.unity
│       │   ├── Skirmish.unity
│       │   └── Results.unity
│       ├── Scripts/
│       │   ├── Core/
│       │   ├── Application/
│       │   ├── AI/
│       │   ├── Presentation/
│       │   ├── UI/
│       │   └── Platform/
│       ├── Tests/
│       └── UI/
├── Packages/
│   ├── manifest.json
│   └── packages-lock.json
├── ProjectSettings/
├── .gitignore
└── .gitattributes           # Add Git LFS patterns when appropriate
~~~

This layout is a **proposal**, not a claim those files have been created.

### Source-control rules

- Commit \`Assets/\`, \`Packages/\`, \`ProjectSettings/\` and every applicable **\`.meta\` file**.
- Set **Visible Meta Files** and text asset serialization for sensible diffs/merging.
- Ignore generated \`Library/\`, \`Temp/\`, \`Obj/\`, \`Logs/\`, \`UserSettings/\`, \`Builds/\`, IDE caches and platform output.
- Use a standard **Unity-specific .gitignore**. Do **not** globally ignore \`*.meta\`.
- Consider **Git LFS** for large redistributable FBX, audio, textures, and binary assets, after checking GitHub storage limits and license permissions.
- Pin package versions and Unity editor version; **avoid random Asset Store packages as dependencies**.
- Never commit license keys, signing keys, tokens, or other credentials.
- Prefer small pull requests with descriptions, tests, and screenshots/videos of any visual changes.

## 7. AI-assisted / "vibe coding" workflow

The intention is to **direct the development through tasks**, not to generate a complete game blindly from one enormous prompt.

### Tools to evaluate

1. **Codex in VS Code** to edit and refactor C#, tests, data definitions, and docs.
2. Unity's **official third-party agent plugin**, where supported, to supply Unity-specific context and workflows.
3. Unity's **CLI/MCP tools** to let supported agents inspect scenes and logs, create or modify project objects, and run Editor actions.
4. Unity's **in-editor assistant** as an optional convenience, mindful of pricing, availability, and credit limits.

The official Unity tooling was evolving in 2026. **Check versions, integration prerequisites, trial terms, and what operations each agent can actually perform before promising fully automated editor work.** An AI-generated script may still need an open Editor, a working compile, an imported model, scene references, and human verification.

### Preferred agent work cycle

1. **State one small outcome**: e.g. "Click a creature; show only legal move tiles and remaining arrows."
2. Point the agent to **this file and README.md**, including explicit "do not invent rules" constraints.
3. Ask it to update **only relevant scripts/scenes/prefabs**, and to write or extend **tests** for logic.
4. Let the Editor **compile**, run tests, and test with mouse/touch. Read console errors.
5. **Visually inspect** the outcome in the running scene and verify it matches the README.
6. Review the code/diffs. Commit a coherent, verified change.
7. Repeat with the next small slice.

Suggested task prompt:

> Work in the existing Unity 6 URP project. Read README.md (game rules) and docs/UNITY_TECH_STACK.md (architecture). Implement [one feature] with data-driven, headless C# rules, Unity presentation separated from state, and EditMode tests. Do not invent missing game mechanics or alter other systems. Compile, test, and report exactly what changed and what could not be verified.

### AI-related traps to avoid

- Code compiles but the **scene has no references**, so nothing appears or works.
- Scripts rely on \`Update()\` side effects and create desynchronization between UI and logic.
- An agent "improves" the game by **passively claiming tiles** or allowing attacks after Capture. **README is the rules authority.**
- Unity Editor changes aren't saved, or agents create duplicate prefabs/scripts.
- Importing random fantasy packs leads to **inconsistent art styles**, massive builds, or licensing mistakes.
- Assuming an image generator's "spritesheet" is a properly rigged 3D asset.
- Turning a small, deterministic board game into a complicated physics/ECS/networking problem.

**Human responsibilities remain:** making the art-direction choice, verifying user experience, approving purchases/licenses, playtesting balance, and reviewing agent changes.

## 8. Mobile plan

**Long-term target:** Android and iOS. Use the Windows Unity Editor for primary development and desktop tests.

### Android

- Install **Android Build Support**, **Android SDK & NDK Tools**, and **OpenJDK** through Unity Hub.
- Start with development **APK** builds on a real Android device.
- Prepare signed **AAB** bundles and store compliance only once preparing a release.
- Test touch size/selection accuracy, screen cutouts/safe areas, texture memory, scene loading, thermals, and performance on real hardware.

### iOS

- Install Unity's **iOS Build Support** module.
- For a **local iOS build**, Unity generates an Xcode project; **Xcode/macOS is required** to compile/package the app locally.
- If developing only on Windows, evaluate **Unity Build Automation / a macOS cloud build environment** rather than assuming Windows can produce final iOS apps directly.
- App Store distribution requires the relevant Apple developer setup.
- Do not defer all iOS testing until release; screen ratios, safe areas, touch, and device rendering need validation.

### Camera, orientation, and performance

- **Landscape first** is a practical UI starting point for showing a tactical grid and action buttons. Test portrait feasibility, but do not commit to two wholly separate layouts immediately.
- Start with a compact fixed camera. Add pinch zoom/panning **only if needed** for readability.
- Aim for **60 FPS on capable devices** with an acceptable tested lower-end fallback.
- Use modest geometry, sensible animation budgets, texture atlases/compression, baked or economical lighting, URP quality levels, and restrained transparent particles/post-processing.
- Profile on **real Android devices** before concluding that PC editor performance is sufficient.
- Avoid HDRP, unnecessary real-time GI, excessive dynamic shadows, cinematic camera systems, and expensive effects for the first 7×7 battlefield.

## 9. Implementation roadmap

The MVP must prove the **real turn-based game is fun**, not merely display a cool fantasy board.

### Milestone 0: Create the project

- [ ] Install Unity Hub and a supported **Unity 6** release.
- [ ] Create a **3D URP** Unity project inside this Git repository.
- [ ] Install Android build dependencies in Unity Hub.
- [ ] Set up the Unity .gitignore, visible meta files, text serialization, exact Editor and package versions.
- [ ] Set up C# editor/agent integration and a working compile.
- [ ] Confirm a basic scene launches in the Editor and Git tracks the project correctly.

### Milestone 1: Headless playable rules, no real art

- [ ] Define tile ownership, occupancy, turn, player and unit state.
- [ ] Implement AP (10 per turn) and capacity (10 active).
- [ ] Legal moves based on intact arrows; rotation costing AP.
- [ ] Summon only onto an **empty, owned tile**; no distance/frontline restriction; newborn unit exhausted.
- [ ] Combat, automatic eligible counterattacks, 1/2/4 damage, armor reset, arrow loss and death.
- [ ] Attack or Capture **exhausts** the acting creature; defensive counterattacks still work.
- [ ] Capture **all eligible adjacent arrow-directed tiles in one action**; enemy-occupied targets are protected.
- [ ] Persistent ownership; round scoring and win detection.
- [ ] EditMode tests for the above and useful action/event logging.

### Milestone 2: Ugly but complete 3D skirmish

- [ ] Build a **7×7** clickable board using simple primitive meshes.
- [ ] Add ownership coloring, logical arrow overlays, legal-action highlights and basic 3D placeholders.
- [ ] Summoner selection; randomly select an opposing AI Summoner.
- [ ] Build functional HUD, action buttons, attack previews, Controller arrow-choice interface and result screen.
- [ ] Add a basic legal-move, weighted-evaluation AI.
- [ ] Play a **whole match** from selection to victory/defeat with no developer shortcuts.

### Milestone 3: Visual vertical slice

- [ ] Replace placeholders with **one consistent, licensed** 3D asset style.
- [ ] Import a few animated creature archetypes and build reusable prefabs.
- [ ] Establish reusable stone tile, forest-border, lighting, rune and particle setups.
- [ ] Animate summoning, moving, combat reactions, arrow-breaking and capturing.
- [ ] Improve readability of missing arrows, exhaustion, scores, and possible actions.
- [ ] Make attacks and capture visually satisfying without delaying response to input.

### Milestone 4: Android usability and performance

- [ ] Produce and install the first real **Android APK**.
- [ ] Test landscape UI, touch selection, safe-area layouts, and readability.
- [ ] Test a full AI skirmish on-device.
- [ ] Profile and optimize rendering, memory and battery usage.
- [ ] Confirm local saves and resume behavior if implemented.

### Milestone 5: Expand carefully

- [ ] Increase the creature roster after validating different arrow patterns and style matchups.
- [ ] Introduce a small, explicitly specified spell set.
- [ ] Improve AI difficulty and deployment tactics.
- [ ] Add alternate maps/visual themes.
- [ ] Resolve open balance/tie/stalemate rules in README.
- [ ] Evaluate iOS test builds.
- [ ] Only then revisit asynchronous multiplayer and other game modes.

### First vertical-slice definition of done

A person can:

1. Launch the game and select a Summoner.
2. Fight a random AI-controlled Summoner.
3. Summon a model onto an **owned tile**, move and rotate it.
4. Attack an enemy with directional arrows, armor breaks, and conditional counterattacks.
5. Capture all eligible pointed-at tiles in one action without capturing **enemy-occupied** targets.
6. See exhaustion prevent the creature from attacking/capturing twice.
7. Watch score accumulate based on **persistent tile ownership**.
8. Reach a deterministic win/lose result, then play again.
9. Complete all of the above **without internet or a running backend**.

If that works with temporary models, the game already exists. Better art can follow.

## 10. What we explicitly are NOT building yet

- Unreal/Godot/PlayCanvas alternative clients.
- Online accounts, authoritative game servers, lobbies, live PvP, matchmaking or rankings.
- A complex deckbuilder/collection economy (not designed in README).
- A custom 3D modeling/rigging pipeline or procedural AI-generated creature factory.
- RPG-style combat stats, physics-based movement, arbitrary equipment systems.
- Massive open worlds, free-moving characters, complex navigation meshes.
- Unfinalized chain combos, diagonal arrows, spell subsystems, or elaborate Summoner powers.
- An expensive backend, cloud database, or live-ops platform for an **offline skirmish**.

## 11. Reference documentation (checked October 2026)

These links are provided for future setup/verification. Always verify contemporary Unity documentation and license terms when installing/upgrading.

- [Unity's official AI tools and agent integrations](https://unity.com/features/ai)
- [Unity AI onboarding: plugin, CLI, and MCP](https://unity.com/onboardingsuccess/unity-ai)
- [Unity Universal Render Pipeline](https://docs.unity.com/en-us/engine/6000.6/manual/render-pipelines/universal-render-pipeline/urp-introduction)
- [Unity 6 runtime UI Toolkit](https://docs.unity.com/en-us/engine/6000.6/manual/uitoolkits/uielements/uie-support-for-runtime-ui)
- [Unity Android project setup](https://docs.unity.com/en-us/engine/6000.6/manual/platform-specific/android/getting-started)
- [Unity Android APK/AAB builds](https://docs.unity.com/en-us/engine/6000.3/manual/platform-specific/android/building-and-delivering/build-process)
- [Unity iOS build process and macOS requirements](https://docs.unity.com/en-us/engine/6000.5/manual/platform-specific/iphone/ios-building-and-delivering/build-process)
- [Unity plans and current eligibility](https://unity.com/products)
- [Unity Asset Store licensing FAQ](https://assetstore.unity.com/browse/eula-faq)
- [Unity source-control ignore recommendations](https://docs.unity.com/unity-version-control/ignore-files)

---

**Implementation principle:** **Code the rules; let Unity display them.** A beautiful board without correct arrows, exhaustion, capture and scoring is not a playable prototype. A playable rules engine with ugly models can become a beautiful game.
