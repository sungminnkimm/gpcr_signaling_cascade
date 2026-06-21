# Protein Artwork Upload Guide

How to supply hand-drawn protein/molecule images for the GPCR signaling simulation (`index.html`).

## Where to put files

Drop your images into:

```
assets/proteins/
```

Use the exact filenames below so the simulation can find them automatically.

## File list

| Filename | Entity | Current on-screen height | Notes |
|---|---|---|---|
| `gpcr.png` | 7-TM receptor (resting) | ~4.5× membrane thickness | Vertical orientation — drawn as if crossing the membrane top-to-bottom. Scales with window size since it's relative to the membrane band, not a fixed pixel size |
| `gpcr_active.png` | 7-TM receptor (glowing/activated) | same as above | Same pose, lit up. Optional — falls back to faking a glow over the resting art if skipped |
| `subunit_alpha.png` | Gα subunit, **GDP-bound** (resting) | 150 px | Roughly circular/blob. Include a small dark "GDP" marking baked into the art — no separate nucleotide file needed |
| `subunit_alpha_gtp.png` | Gα subunit, **GTP-bound** (active) | 150 px | Same pose, with a small glowing "GTP" marking baked in. Optional — falls back to a vector glow overlay if skipped |
| `subunit_beta.png` | Gβ subunit | 150 px | |
| `subunit_gamma.png` | Gγ subunit | 150 px | |
| `adenylate_cyclase.png` | AC enzyme (resting) | ~6× membrane thickness | Vertical, spans membrane like the GPCR. Also scales with window size |
| `adenylate_cyclase_active.png` | AC enzyme (active/orange) | same as above | Optional |
| `pka_regulatory.png` | PKA regulatory (R) subunit | 72 px | |
| `pka_catalytic.png` | PKA catalytic (C) subunit, inactive | 72 px | Drawn rotated 180° in the tetramer |
| `pka_catalytic_active.png` | Free catalytic PKA (neon pink) | 72 px | Drawn rotated 90° while racing to the nucleus |
| `ligand_glucagon.png` | Glucagon ligand | 16 px | Small, simple. Not yet uploaded — currently falls back to a vector dot |
| `camp.png` | cAMP messenger | 9–14 px | Tiny dot/molecule |
| `pde.png` | Phosphodiesterase | 36 px | |

You don't need all of them at once — any file that's missing just falls back to the current vector-drawn shape, so artwork can be swapped in incrementally. See `EDITING-PROTEIN-ART.md` for exactly which line to edit if any of these sizes need adjusting.

## Image requirements

- **Format:** PNG with a transparent background (alpha channel) — no white/colored background boxes, since these sit on a near-black canvas.
- **Orientation:** draw upright, front-facing, no baked-in rotation — the code handles rotation/flipping in software (e.g. the alpha subunit when it slides, or the PKA catalytic subunits which are now rotated by code).
- **Centering:** keep the subject centered with a little padding — images are positioned by their center point.
- **Color/contrast:** the background is `#050810` (near-black). Avoid very dark fills with no outline, or the art will disappear into the background. A thin light or neon-colored outline reads best.
- **Resolution:** draw at roughly 2-3x the on-screen heights above in your own aspect ratio (the code scales to fit while preserving it) — gives crisper rendering on high-DPI screens. Sizes have grown since the original upload, so favor higher native resolution, especially for the GPCR and Adenylate Cyclase art which scale up with window size.

## After uploading

Once files are in `assets/proteins/`, let the assistant know — the vector-drawn shapes (`drawSubunit`, `drawGPCR`, `drawAC`, etc.) will be swapped for image rendering while keeping all existing physics/animation logic unchanged.
