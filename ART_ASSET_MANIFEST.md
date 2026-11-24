# The Villainess's Second Life — Art Integration Manifest

This document defines the artwork slots for the visual-novel engine. **No generated images are included yet.** Gameplay remains usable without artwork. Scenes already carry `visuals` metadata and the `renderSceneArt()` function supports backgrounds, two-position portraits, event CGs, and UI mood cues.

## Installation rules

1. Generate or draw the artwork at the filename listed below (WebP, optimized for web). Keep character identity, outfits, loc details, and style consistent across expressions.
2. Upload the WebP file at the exact repository path.
3. Enable its ID in the `ART_READY` Set near the engine art registry in `index.html`. Examples: `'background:chamber'`, `'portrait:sera:neutral'`, `'cg:ch1_mirror'`. **Only** enable after the file exists.
4. Backgrounds should contain no baked-in text, dialogue boxes, menus, day counters, or choices. Those are provided by HTML/CSS.
5. If an asset is absent or fails to load, the game falls back to the existing clean gradient; it does not display placeholder drawings.
6. Route scenes contain portrait and background metadata automatically. Individual scenes can override `visuals` when specific artwork/expression/lighting is ready. The optional fields `music` and `sfx` are reserved for future use; they do not trigger playback yet.

## Core background library

| Art ID | File location | Visual brief |
| --- | --- | --- |
| chamber | `assets/backgrounds/vale_estate/vale_bedroom_morning.webp` | Velvet, dawn light, carved dressing mirror, lived-in noble bedroom |
| hall | `assets/backgrounds/palace/palace_reception_hall.webp` | Majestic Asterian court, gold and marble, open view for portrait composition |
| garden | `assets/backgrounds/palace/moonrose_garden.webp` | Manhwa-style royal gardens and glass conservatory |
| library | `assets/backgrounds/tower/forbidden_library.webp` | Floating books, ancient magical stacks |
| tower | `assets/backgrounds/tower/star_observatory.webp` | Astral observatory, silver moonlight and constellations |
| ballroom | `assets/backgrounds/palace/imperial_ballroom.webp` | Masquerade ballroom, violet and warm gold chandeliers |
| north | `assets/backgrounds/north/winter_duchy.webp` | Severe snowy manor, black stone and distant mountains |
| trial | `assets/backgrounds/palace/royal_tribunal.webp` | Intimidating tribunal, high ceiling and judicial dais |
| sareth | `assets/backgrounds/sareth/sareth_harbor.webp` | Foreign coastal kingdom, distinct warm architecture and market harbor |
| ship | `assets/backgrounds/sareth/ashwind_ship.webp` | Foreign mercenary ship and deck |
| blank | `assets/backgrounds/meta/unwritten_room.webp` | Author route's unnatural empty white space |

Recommended: landscapes around **1920×1080**, WebP. Compose the focal area above the dialogue panel and allow room for standing character sprites.

## Character portrait library

For each character below, the engine expects six expressions under `assets/portraits/<folder>/<folder>_formal_<expression>.webp`. The expressions are **neutral, shocked, guarded, soft, angry, worried**.

| Character ID | Folder | Visual consistency requirement |
| --- | --- | --- |
| sera | seraphina | Black heroine with gorgeous, intricate locs — keep exact skin tone, loc style, accessories and facial features consistent |
| cassian | cassian | Crown prince; elegant, politically formidable |
| elian | elian | Northern duke; dangerous and unrepentant, not visibly “cured” by romance |
| lucien | lucien | Independent archmage; neutral toward imperial succession |
| kael | kael | Mercenary captain from Sareth; distinguish his clothing and cultural environment from Asteria |
| aurelian | aurelian | Author / deity; inhuman composure and unsettling affection |
| mira | mira | Original saintess, emotionally nuanced |
| tessa | tessa | Seraphina's maid and confidante, not interchangeable with Mira |

Recommended: transparent WebP cutouts (half or three-quarter body) around **900×1400**, transparent background. Portraits should show expressions, not in-image captions or text.

## Priority event CGs

| CG ID | Repository path | Story moment |
| --- | --- | --- |
| ch1_mirror | `assets/cg/shared/ch1_mirror_reveal.webp` | Seraphina's first recognition in the mirror |
| ch1_first_court | `assets/cg/shared/ch1_mira_wine.webp` | Mira and the wineglass confrontation |
| trial | `assets/cg/shared/ch5_tribunal.webp` | Seraphina facing accusation at court |
| duke_collar | `assets/cg/elian/duke_collar_gift.webp` | Elian offers the bracelet and locked velvet collar |
| prince_contract | `assets/cg/cassian/prince_contract.webp` | Cassian's marriage contract |
| mage_lens | `assets/cg/lucien/memory_lens.webp` | Lucien reveals memories with a magical lens |
| mercenary_departure | `assets/cg/kael/sareth_departure.webp` | Leaving the empire with Kael |
| author_glitch | `assets/cg/aurelian/author_go_home_glitch.webp` | Aurelian's duplicated 'Go home' options |
| winter_ending | `assets/endings/elian_horrible.webp` | Duke's horrible ending (reserved; ending-specific art display to be connected later) |
| author_ending | `assets/endings/aurelian_horrible.webp` | Author's horrible ending (reserved; ending-specific art display to be connected later) |

Recommended: CGs around **1920×1080**, with the lower 35% safe for readable dialogue overlay. Never place clickable choices inside generated images.

## Decorative UI art

- `assets/ui/ornaments/gold_flower_left.webp` — transparent hand-illustrated gold floral corner
- `assets/ui/ornaments/gold_flower_right.webp` — matching transparent right corner
- `assets/ui/ornaments/title_divider.webp` — delicate gold flourish
- `assets/ui/frames/dialogue_crest.webp` — independent ornament for narrator/nameplate

These paths are registered but *not automatically rendered yet*: wait until the approved high-quality art assets exist, then size and position them visually against the current UI.

## Scene contract

```js
{
  chapter: 4,
  bg: 'ballroom',
  speaker: 'Narrator',
  text: '...',
  choices: [...],
  visuals: {
    background: 'ballroom',
    portraits: [
      { character: 'sera', expression: 'guarded', side: 'left' },
      { character: 'cassian', expression: 'neutral', side: 'right' }
    ],
    cg: null,              // optional named CG key
    hidePortraits: false,  // CG events hide portraits
    uiMood: 'court',
    music: null,           // reserved for future sound integration
    sfx: null              // reserved for future sound integration
  }
}
```

The art registry and scene metadata are authored in `index.html`. The scene can explicitly set its own `visuals` override for poses, CGs, and other exceptions.

## Recommended art generation sequence

1. Lock down **Seraphina's reference design** (locs, facial features, dresses, skin tone) and test neutral + shocked cutouts.
2. Create **Vale bedroom** and **mirror reveal CG** to test Chapter 1 visual integration.
3. Create **Mira and Tessa**, court backgrounds, then the wine incident and tribunal CGs.
4. Create the five love interests' reference sheets and essential expressions.
5. Complete each route's distinctive environments and ending/event CGs.
6. Create consistent, transparent, *illustrated* gold flowers, frames and flourishes last, then integrate carefully without recreating them in CSS.

## Current limits

The existing game is a substantial branching draft, not a complete illustrated 20+ hour title. The latest story pass improves emotional connective tissue in Chapters 2–5; the individual 18-scene romance routes still need careful writing edits, scene sequencing, and additional subplots to approach full manhwa-season depth. No generated images are uploaded yet.
