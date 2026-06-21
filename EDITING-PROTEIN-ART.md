# Editing Protein Size & Orientation in the Code

This explains exactly what to change in `index.html` to resize, reposition, or reorient the hand-drawn protein images once they're loaded.

## The core mechanism: `drawImg()`

All protein art is drawn through one helper function (around line 273):

```js
function drawImg(img, cx, cy, targetH, glowColor, glowBlur)
```

- `cx, cy` — the center point where the image is placed
- `targetH` — the **height in pixels** the image is scaled to. Width is computed automatically to preserve the drawing's original aspect ratio.
- `glowColor` / `glowBlur` — optional neon glow halo (leave `null`/omit for no glow)

**To resize any protein, change its `targetH` number.** You never need to touch the PNG file itself.

There is currently no rotation parameter — every image is drawn upright, exactly as drawn. See [Orientation](#orientation--flipping) below for how to add rotation/flipping if needed.

## Where each protein's size/position is set

| Protein | Function | Line | What to edit |
|---|---|---|---|
| GPCR receptor (resting + active) | `drawGPCR` | 708 | `(bot-top)*4.2` — size is relative to membrane thickness, not a fixed pixel value |
| Gα subunit (GDP/GTP) | `drawAlpha` | 751 | `52` — target height in px |
| Gβ subunit | `drawBeta` | 757 | `40` |
| Gγ subunit | `drawGamma` | 762 | `36` |
| Adenylate Cyclase | `drawAC` | 797 | `(bot-top)*5.2` — relative to membrane thickness |
| Adrenaline ligand | `drawLigands` | 832 | `16` |
| cAMP messenger | `drawCamp` | 846 | `9` |
| Phosphodiesterase (PDE) | `drawPDE` | 860 | `36` |
| PKA regulatory (R) subunits | `drawPKA` | 876–877 | `36` (appears twice, one per R subunit) |
| PKA catalytic (C) subunits, inactive | `drawPKA` | 878–879 | `36` (appears twice) |
| Free catalytic PKA (active, pink) | `drawCatPKA` | 892 | `44` |

### Why GPCR and AC use `(bot-top)*N` instead of a fixed number

The membrane band's thickness (`bot-top`) scales with the browser window size, so the GPCR and AC images scale with it too — keeping them proportional to the membrane no matter the screen size. Increase `N` to make them bigger relative to the membrane; decrease to shrink. Everything else uses a flat pixel value since it floats freely in the cytoplasm and doesn't need to scale with the membrane.

## Position offsets

Some proteins are drawn at an offset from a parent anchor point rather than at `cx, cy` directly — e.g. β and γ flank the GPCR's α subunit:

```js
// inside drawGPCR (~line 736)
drawBeta(g.x-12, gy);
drawGamma(g.x+12, gy);
drawAlpha(g.x, gy-10, gp.nucleotide);
```

The `-12`, `+12`, `-10` numbers are pixel offsets from the GPCR's anchor point (`g.x`, `gy`). Increase the magnitude to spread the subunits further apart; decrease to cluster them tighter. Same pattern applies to the four PKA subunits in `drawPKA` (offsets of `-9`/`+9` and `-7`/`+7`).

## Vertical placement relative to the membrane

The GPCR and G-protein vertical position is computed near the top of `drawGPCR` (~line 701):

```js
const top = H*REGION.membraneTop, bot = H*REGION.membraneBot;
const halfThickness = (bot-top)/2;
const midY = (top+bot)/2 + halfThickness;   // GPCR center
...
const gy = bot + halfThickness + 16 + Math.sin(gp.bobPhase)*2;  // G-protein center
```

To raise/lower the GPCR or G-protein relative to the membrane, adjust the `+ halfThickness` term or the trailing `+ 16` — these are added pixel offsets, not scale factors.

## Orientation / flipping

There's no rotation support yet. If you need a protein mirrored (e.g. so it visually "leans" the direction it's sliding) or rotated:

1. Wrap the `drawImg` call in `sctx.save()` / `sctx.translate(cx,cy)` / `sctx.scale(-1,1)` (mirror) or `sctx.rotate(angle)`, then call `drawImg` with `cx=0, cy=0`, then `sctx.restore()`.
2. Alternatively, extend `drawImg` itself to accept an `angle` parameter and call `sctx.rotate(angle)` before `sctx.drawImage(...)` — cleaner if multiple proteins need rotation.

Ask the assistant to make this change if/when a specific protein needs to rotate or flip; it's a small, localized edit once you know which protein and what triggers the rotation (e.g. direction of travel).
