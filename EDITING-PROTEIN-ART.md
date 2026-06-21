# Editing Protein Size & Orientation in the Code

This explains exactly what to change in `index.html` to resize, reposition, or reorient the hand-drawn protein images once they're loaded.

## The core mechanism: `drawImg()`

All protein art is drawn through one helper function (around line 277):

```js
function drawImg(img, cx, cy, targetH, glowColor, glowBlur, flipX, angle)
```

- `cx, cy` — the center point where the image is placed
- `targetH` — the **height in pixels** the image is scaled to. Width is computed automatically to preserve the drawing's original aspect ratio.
- `glowColor` / `glowBlur` — optional neon glow halo (leave `null`/omit for no glow)
- `flipX` — pass `true` to mirror the image horizontally
- `angle` — rotation in **radians** (`Math.PI` = 180°, `Math.PI/2` = 90°, `Math.PI/4` = 45°). Leave `0`/omit for no rotation.

**To resize any protein, change its `targetH` number.** You never need to touch the PNG file itself. `flipX` and `angle` can be combined on the same call.

## Where each protein's size/position/orientation is set

| Protein | Function | Line | What to edit |
|---|---|---|---|
| GPCR receptor (resting + active) | `drawGPCR` | 766 | `(bot-top)*4.5` — size is relative to membrane thickness, not a fixed pixel value |
| Gα subunit (GDP/GTP) | `drawAlpha` | 809 | `150` — target height in px |
| Gβ subunit | `drawBeta` | 815 | `150` |
| Gγ subunit | `drawGamma` | 820 | `150` |
| Adenylate Cyclase | `drawAC` | 855 | `(bot-top)*6` — relative to membrane thickness |
| Glucagon ligand | `drawLigands` | 890 | `16` |
| cAMP messenger (free-floating) | `drawCamp` | 904 | `9` |
| cAMP messenger (docked on PKA) | `drawPKA` | 947 | `14` |
| Phosphodiesterase (PDE) | `drawPDE` | 918 | `36` |
| PKA regulatory (R) subunits | `drawPKA` | 935–936 | `72` (appears twice, one per R subunit). The right-hand one (936) also passes `flipX=true` to mirror it |
| PKA catalytic (C) subunits, inactive | `drawPKA` | 937–938 | `72` (appears twice). Both pass `angle=Math.PI` (180° rotation); the right-hand one (938) also passes `flipX=true` |
| Free catalytic PKA (active, pink) | `drawCatPKA` | 960 | `72`, with `angle=Math.PI/2` (90° rotation) |

### Why GPCR and AC use `(bot-top)*N` instead of a fixed number

The membrane band's thickness (`bot-top`) scales with the browser window size, so the GPCR and AC images scale with it too — keeping them proportional to the membrane no matter the screen size. Increase `N` to make them bigger relative to the membrane; decrease to shrink. Everything else uses a flat pixel value since it floats freely in the cytoplasm and doesn't need to scale with the membrane.

## Position offsets

Some proteins are drawn at an offset from a parent anchor point rather than at `cx, cy` directly — e.g. β and γ sit near the GPCR's α subunit:

```js
// inside drawGPCR (~line 793)
if(gp.attached){
  drawBeta(g.x+35, gy+15);
  drawGamma(g.x+35, gy+15);
  drawAlpha(g.x+7, gy+10, gp.nucleotide);
} else {
  drawBeta(g.x+50, gy+15);
  drawGamma(g.x+50, gy+15);
}
```

The `+35`, `+7`, `+50`, `+15`, `+10` numbers are pixel offsets from the GPCR's anchor point (`g.x`, `gy`). Adjust them to reposition the subunits relative to the receptor. Same pattern applies to the four PKA subunits in `drawPKA` (offsets of `-9`/`+9`, `-8`/`+8`, `-6`/`-7`) and to the four cAMP docking slots a few lines below (`CAMP_SLOTS` array).

## Vertical placement relative to the membrane

The GPCR and G-protein vertical position is computed near the top of `drawGPCR` (~line 759):

```js
const top = H*REGION.membraneTop, bot = H*REGION.membraneBot;
const halfThickness = (bot-top)/2;
const midY = (top+bot)/2 + halfThickness;   // GPCR center
...
const gy = bot + halfThickness + 16 + Math.sin(gp.bobPhase)*2;  // G-protein center
```

To raise/lower the GPCR or G-protein relative to the membrane, adjust the `+ halfThickness` term or the trailing `+ 16` — these are added pixel offsets, not scale factors.

The overall membrane/cytoplasm/nucleus split is controlled separately by the `REGION` object near the top of the script (~line 329):

```js
const REGION = {
  membraneTop: 0.18,
  membraneBot: 0.245,
  cytoTop: 0.245,
  cytoBot: 0.84,
  nucleusTop: 0.84
};
```

Each value is a fraction of the canvas height. Shrinking the gap between two of these values shrinks that region; the freed fraction needs to go somewhere else in the list to keep the total at 1.0.

## Orientation / flipping / rotation

Both are supported directly on `drawImg` now — no extra wrapping needed:

```js
// mirror horizontally
drawImg(img, x, y, targetH, null, null, true);

// rotate 90°
drawImg(img, x, y, targetH, null, null, false, Math.PI/2);

// mirror AND rotate 180°
drawImg(img, x, y, targetH, null, null, true, Math.PI);
```

Internally, `drawImg` translates to `(cx, cy)`, applies `rotate(angle)` then `scale(-1,1)` if flipping, and draws the image centered on the new origin — so rotation and flip can be combined safely in either order. If a new protein needs to face a particular direction (e.g. lean the way it's traveling), pass an `angle` computed from its velocity (`Math.atan2(vy, vx)`) instead of a fixed constant.
