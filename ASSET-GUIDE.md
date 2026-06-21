# Protein Artwork Upload Guide

How to supply hand-drawn protein/molecule images for the GPCR signaling simulation (`index.html`).

## Where to put files

Drop your images into:

```
assets/proteins/
```

Use the exact filenames below so the simulation can find them automatically.

## File list

| Filename | Entity | Suggested canvas size | Notes |
|---|---|---|---|
| `gpcr.png` | 7-TM receptor (resting) | 60×140 px | Vertical orientation — drawn as if crossing the membrane top-to-bottom |
| `gpcr_active.png` | 7-TM receptor (glowing/activated) | 60×140 px | Same pose, lit up. Optional — falls back to faking a glow over the resting art if skipped |
| `subunit_alpha.png` | Gα subunit, **GDP-bound** (resting) | 40×40 px | Roughly circular/blob, square canvas. Include a small dark "GDP" marking baked into the art — no separate nucleotide file needed |
| `subunit_alpha_gtp.png` | Gα subunit, **GTP-bound** (active) | 40×40 px | Same pose, with a small glowing "GTP" marking baked in. Optional — falls back to a vector glow overlay if skipped |
| `subunit_beta.png` | Gβ subunit | 36×36 px | |
| `subunit_gamma.png` | Gγ subunit | 30×30 px | |
| `adenylate_cyclase.png` | AC enzyme (resting) | 70×120 px | Vertical, spans membrane like the GPCR |
| `adenylate_cyclase_active.png` | AC enzyme (active/orange) | 70×120 px | Optional |
| `pka_regulatory.png` | PKA regulatory (R) subunit | 36×36 px | |
| `pka_catalytic.png` | PKA catalytic (C) subunit, inactive | 36×36 px | |
| `pka_catalytic_active.png` | Free catalytic PKA (neon pink) | 36×36 px | |
| `ligand_glucagon.png` | Glucagon ligand | 24×24 px | Small, simple |
| `camp.png` | cAMP messenger | 16×16 px | Tiny dot/molecule |
| `pde.png` | Phosphodiesterase | 30×30 px | |

You don't need all of them at once — any file that's missing just falls back to the current vector-drawn shape, so artwork can be swapped in incrementally.

## Image requirements

- **Format:** PNG with a transparent background (alpha channel) — no white/colored background boxes, since these sit on a near-black canvas.
- **Orientation:** draw upright, front-facing, no baked-in rotation — the code handles rotation/flipping in software (e.g. when the alpha subunit slides).
- **Centering:** keep the subject centered with a little padding — images are positioned by their center point.
- **Color/contrast:** the background is `#050810` (near-black). Avoid very dark fills with no outline, or the art will disappear into the background. A thin light or neon-colored outline reads best.
- **Resolution:** draw at roughly 2-3x the sizes listed above (e.g. `gpcr.png` at ~180×420px) — gives crisper rendering on high-DPI screens, since the code downscales to the target size.

## After uploading

Once files are in `assets/proteins/`, let the assistant know — the vector-drawn shapes (`drawSubunit`, `drawGPCR`, `drawAC`, etc.) will be swapped for image rendering while keeping all existing physics/animation logic unchanged.
