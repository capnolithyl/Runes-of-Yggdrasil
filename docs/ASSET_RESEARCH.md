# Asset Research: Nordic / Fantasy / Winter 3D

> **Research date:** October 16, 2026.  
> **Status:** Candidate shortlist only. No purchases or approved production assets.  
> **Game:** [Runes of Yggdrasil](../README.md) | [Unity technology plan](UNITY_TECH_STACK.md)  
> **Currency:** USD, before tax. Asset Store prices, regional pricing, discounts, and licenses may change; **open each product page again before purchase**.
>
> Goal: make a compact, polished **7×7 Unity 6 URP diorama** with Nordic/fantasy/winter elements and animated creatures **without learning 3D modeling first**.

## Recommended art directions

### A. Stylized Viking winter: easiest coherent MVP

**Primary reference:** [POLYGON - Vikings Pack](https://syntystore.com/products/polygon-vikings-pack) (**$29.99**).

Stylized snow-covered village buildings, Nordic props, characters, icy terrain, and mountains. Works for an approachable fantasy strategy look with relatively low pressure on mobile hardware. The publisher indicates Unity + URP support, but note that **character animations are not included**. Use animated animals/monsters from separately licensed packs for units and unify their materials/light/colors.

Suggested starting stack:
- [POLYGON - Vikings](https://syntystore.com/products/polygon-vikings-pack): **$29.99**.
- [Quaternius Ultimate Monsters](https://quaternius.com/packs/ultimatemonsters.html): **Free** (50 animated monsters).
- [Quaternius Ultimate Animated Animal Pack](https://quaternius.com/packs/ultimateanimatedanimals.html): **Free** (12 animals with numerous animations).
- [Kenney Holiday Kit](https://kenney.nl/assets/holiday-kit): **Free**, decorative winter assets if style matches.

**Estimated pack cost: $29.99 before tax.** A coherent art direction will still need material, scale, lighting, and animation QA.

### B. More atmospheric, semi-realistic northern winter

Build the grid from original modular stone tiles; surround it with detailed natural assets:

- [Winter Environment - Nature Pack](https://assetstore.unity.com/packages/3d/environments/winter-environment-nature-pack-142750): **$50**.
- [FROST - Frozen Environment Kit](https://assetstore.unity.com/packages/3d/environments/landscapes/frost-frozen-environment-kit-107401): **$24.99**.
- Optional focal landmark: [Frozen Norse Environment](https://assetstore.unity.com/packages/3d/environments/fantasy/frozen-norse-environment-303623): **$69.99**.

**Estimated base (without landmark): $74.99 before tax.** Use restrained portions of these high-resolution packs; their large source textures and shader complexity warrant an Android profile before buying too many.

### C. Fully free proof of concept

- [Quaternius Ultimate Monsters](https://quaternius.com/packs/ultimatemonsters.html): **Free**.
- [Quaternius Ultimate Animated Animal Pack](https://quaternius.com/packs/ultimateanimatedanimals.html): **Free**.
- [Quaternius Ultimate Modular Ruins](https://quaternius.com/packs/ultimatemodularruins.html): **Free**.
- [Quaternius Ultimate Stylized Nature](https://quaternius.com/packs/ultimatestylizednature.html): **Free**.
- [LowPoly WinterPack Lite](https://assetstore.unity.com/packages/3d/environments/fantasy/lowpoly-winterpack-lite-352926): **Free**.
- [Kenney Holiday Kit](https://kenney.nl/assets/holiday-kit): **Free**.

**Estimated asset cost: $0.** Best for validating creature controls, movement, arrow effects, and the 3D board before selecting paid art.

---

## Environment and Nordic/winter packs

**Prices captured October 16, 2026; all are candidacy, not compatibility approvals.**

| Pack (seller) | Listed USD price | Purpose | Caveat |
| --- | ---: | --- | --- |
| [POLYGON - Vikings Pack](https://assetstore.unity.com/packages/3d/environments/fantasy/polygon-vikings-pack-art-by-synty-85664) (Synty) | **$29.99** | Most complete inexpensive stylized Nordic starting point; snowy buildings, props, characters, terrain | Rigged characters but **no included character animations** per publisher. Restricted Single Entity license on Unity Store. |
| [Winter Assets Pack](https://assetstore.unity.com/packages/3d/environments/landscapes/winter-assets-pack-13195) (Daelonik Artworks) | **$19.50** | Ice floes, rocks, snow walls, crystals and frost details | Older package; URP support needs additional import steps. Described by publisher as a *lite* predecessor of Winter Lands. |
| [FROST - Frozen Environment Kit](https://assetstore.unity.com/packages/3d/environments/landscapes/frost-frozen-environment-kit-107401) (Simon Kratz) | **$24.99** | Detailed frozen rocks, snow and ice, atmosphere | 1.4 GB source and 4K textures; adjust texture sizes for mobile, test URP import. |
| [Fantasy Forest Environment](https://assetstore.unity.com/packages/3d/environments/fantasy/fantasy-forest-environment-34245) (TriForge) | **$49.00** | Hand-painted/stylized trees and foliage, non-winter map variation | Not specifically Nordic; visually different from PBR realism. |
| [Low Poly Fantasy Bundle](https://assetstore.unity.com/packages/3d/environments/fantasy/low-poly-fantasy-bundle-258580) (JustCreate) | **$49.99** | 10 stylized packs, 1,700+ prefabs including forests, medieval village and arenas | Broad value, but probably a lot of irrelevant content; check Unity 6 URP before adopting. |
| [Winter Environment - Nature Pack](https://assetstore.unity.com/packages/3d/environments/winter-environment-nature-pack-142750) (ANGRY MESH) | **$50.00** | Realistic snow-covered pines, vegetation, props, winter ground | 1.7 GB and foliage shaders; confirm mobile budgets and URP behavior. |
| [Medieval Nordic Village Environment](https://assetstore.unity.com/packages/3d/environments/historic/medieval-nordic-village-environment-279881) (BEFOUR STUDIOS) | **$64.99** | Detailed Nordic houses, terrain and village landmarks | 4.4 GB; publisher documents a separate URP scene import; more useful for scenery than a small board. |
| [Frozen Norse Environment](https://assetstore.unity.com/packages/3d/environments/fantasy/frozen-norse-environment-303623) (Leartes Studios) | **$69.99** | Norse stone architecture, rune-carved frozen ruins | 4.0 GB and few public reviews; only purchase after sample/import/performance review. |
| [Winter Lands](https://assetstore.unity.com/packages/3d/environments/landscapes/winter-lands-247672) (Daelonik Artworks) | **$79.50** | Higher-end modular snow, ice, frozen structures | 1.6 GB; significantly more expensive than Winter Assets Pack. |
| [Viking Village Environment Megapack](https://assetstore.unity.com/packages/3d/environments/fantasy/viking-village-environment-megapack-modular-with-interiors-223025) (Leartes) | **$199.99** | Detailed modular Norse village with interiors | 5 GB and likely overkill for a tiny fixed-camera tactics board. |

### Free environment candidates

| Pack | Price | Notes |
| --- | ---: | --- |
| [Quaternius Ultimate Modular Ruins](https://quaternius.com/packs/ultimatemodularruins.html) | **Free** | 90 modular ruin/dungeon models; good for ancient runestones/arena perimeter. |
| [Quaternius Ultimate Stylized Nature](https://quaternius.com/packs/ultimatestylizednature.html) | **Free** | 63 nature models; useful to dress forest edges. |
| [Quaternius Stylized Nature MegaKit](https://quaternius.com/packs/stylizednaturemegakit.html) | **Free tier** | 110+ total nature assets described; page notes only **60–70% free**, with more content/engine-ready versions in paid/source tiers. Don't treat all 116 models as the free download. |
| [Quaternius Medieval Village MegaKit](https://quaternius.com/packs/medievalvillagemegakit.html) | **Free tier** | Modular grid-compatible medieval architecture; free version is a **subset**; complete preconfigured URP project belongs to source/premium tier. |
| [LowPoly WinterPack Lite](https://assetstore.unity.com/packages/3d/environments/fantasy/lowpoly-winterpack-lite-352926) | **Free** | 7 prefabs; Unity 6-compatible listing; convenient winter decoration. |
| [Kenney Holiday Kit](https://kenney.nl/assets/holiday-kit) | **Free (CC0)** | 100 winter/holiday 3D files; some pieces may feel too Christmas-specific. |

## Animated creatures and Summoner models

| Pack | Price | Why it fits |
| --- | ---: | --- |
| [Quaternius Ultimate Monsters](https://quaternius.com/packs/ultimatemonsters.html) | **Free** | **50 animated monsters**, strong placeholder collection for giants, beasts and fantasy enemies. |
| [Quaternius Ultimate Animated Animal Pack](https://quaternius.com/packs/ultimateanimatedanimals.html) | **Free** | **12 animals with 12+ animations each**, including attack, walk, and death; good test of the wolf/bear-style unit workflow. Verify which species are actually present after download. |
| [Quaternius Ultimate Animated Character Pack](https://quaternius.com/packs/ultimatedanimatedcharacter) | **Free** | 50+ animated humanoid characters, plausible Summoner placeholders. |
| [Low Poly Animated Fantasy Creatures](https://assetstore.unity.com/packages/3d/animations/low-poly-animated-fantasy-creatures-292803) (polyperfect) | **$100 regular listing** | **44 rigged/animated fantasy creatures**, including snow bear, frost stag, undead, skeletons, dragon and others; 224 animations and URP support. Sales were observed but are not a reliable budget assumption. |
| [Wolf - animated animal](https://assetstore.unity.com/packages/3d/characters/animals/wolf-animated-animal-343894) (Mixall) | **$19.99 regular; $9.99 promotion observed** | Standalone rigged PBR wolf. Test rig and material before depending on it. |
| [Enemy Low Poly #02 - Wolf Variation](https://assetstore.unity.com/packages/3d/characters/animals/enemy-low-poly-02-wolf-variation-327021) (Sr_bodinho) | **$4.99** | Four stylized wolf variants and 8 animations; inexpensive option if they match art direction. |

**Important:** \`Low Poly Animated Fantasy Creatures\` has historically been shown discounted (one indexed listing was $50), but a newer official page displayed **$100**. Budget at $100 and verify the current checkout price.

## Licensing / legal warnings: important because this repository is public

1. **Unity Asset Store commercial packs:** Usually licensed for incorporation into a finished game. Do **not** publish source FBX files, purchased asset packages, materials/textures, or extractable third-party content in this public repo without explicit redistribution rights. Check the actual EULA and any publisher-specific restriction for each pack.
2. **Quaternius conflict (2026):** Older individual pack pages still identify some assets as **CC0**, while [Quaternius Asset License (QAL), updated Aug 28, 2026](https://quaternius.com/license.html), prohibits redistributing standalone assets. The license itself says changes do not retroactively apply to assets already obtained under an earlier license. **Do not assume newly downloaded assets can be checked into a public repo until the applicable license is verified or publisher clarifies**. Commercial use inside the game is supported by the QAL.
3. **Kenney:** Individual packs listed here explicitly advertise **CC0**. Confirm licenses packaged with any download.
4. **Assets labeled free** may have optional **paid Source tiers** or limited included models. A source-code/game-engine configured pack may be a separate purchase.
5. Store a source and license record for **every chosen asset**: creator, URL, price/date, download date, applicable license, compatible pipeline, and what files the public repository may include.
6. Keep purchased/raw imported assets outside the public repo or in an appropriately controlled workspace. Use placeholders and import instructions as needed.
7. **Unity 6 URP + Android check** required before buying a heavy 4K environment pack, even if the Unity listing says "URP compatible."

## Decision guidance: what we'd actually acquire first

### First pass: $0

Use Unity primitives to generate the tile grid, plus free animated Quaternius placeholder creatures and ruins/nature. Import **one wolf-like creature** and validate idle/move/attack/death, arrow overlays, selection, exhaustion, Capture, and score. No paid purchases until the gameplay vertical slice is real.

### First art upgrade: roughly $30

Get [Synty POLYGON - Vikings](https://syntystore.com/products/polygon-vikings-pack) at **$29.99**, especially if its polygonal art direction is appealing after looking at its screenshots. It is a more cohesive Nordic shortcut than mixing unrelated PBR packs. The actual combat creatures can use free animated placeholders while the custom runic look is built.

### Higher detail later: roughly $75+ before creature purchases

Try [Winter Environment - Nature Pack](https://assetstore.unity.com/packages/3d/environments/winter-environment-nature-pack-142750) ($50) and [FROST - Frozen Environment Kit](https://assetstore.unity.com/packages/3d/environments/landscapes/frost-frozen-environment-kit-107401) ($24.99). These could make a superb winter arena backdrop, but detailed PBR art with mismatched low-poly monster models can look incoherent. Build a small test scene *before* buying multiple realistic packs.

### What not to buy first

- The $199.99 village megakit: too broad and heavy for a 7×7 board.
- Large creature libraries before evaluating animation quality in-engine.
- Expensive photorealistic packs before testing Android performance and choosing an art direction.
- Packs that only provide an Unreal-specific project without Unity-ready formats or a clear source-file license.

## Sources for prices and pack claims

Each link above points to the originating publisher or Unity Store product listing. Notable source references verified on October 16, 2026:

- [Synty Vikings original publisher and $29.99 price](https://syntystore.com/products/polygon-vikings-pack)
- [Unity Winter Assets Pack $19.50](https://assetstore.unity.com/packages/3d/environments/landscapes/winter-assets-pack-13195)
- [Unity FROST $24.99](https://assetstore.unity.com/packages/3d/environments/landscapes/frost-frozen-environment-kit-107401)
- [Unity Winter Environment Nature $50](https://assetstore.unity.com/packages/3d/environments/winter-environment-nature-pack-142750)
- [Unity Frozen Norse $69.99](https://assetstore.unity.com/packages/3d/environments/fantasy/frozen-norse-environment-303623)
- [Unity Low Poly Animated Fantasy Creatures $100](https://assetstore.unity.com/packages/3d/animations/low-poly-animated-fantasy-creatures-292803)
- [Quaternius current licensing text](https://quaternius.com/license.html)

---

**Conclusion:** Pick **one art direction** first, not every pretty asset pack. Prove one animated creature plus one 7×7 game board and use curated asset packs to enhance the resulting game, not to bury the gameplay under unused models.
