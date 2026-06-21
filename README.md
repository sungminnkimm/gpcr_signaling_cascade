# BIOL360 - Cell Biology, Dr. Barbara Ehlting, Artistic Assignment
## Paul Kim (V00910115), University of Victoria

# GPCR Signaling Cascade — Liver / Glucagon

A BIOL360 (Cell Biology) art project: an interactive, hand-drawn visualization of how the hormone **glucagon** triggers a G-protein-coupled receptor (GPCR) signaling cascade in liver cells, ultimately driving **glycogen breakdown**.

## Live demo

**[View the demo on GitHub Pages](https://sungminnkimm.github.io/gpcr_signaling_cascade/)**

No installation needed — it runs entirely in the browser. Just click the link above.

## What's happening

The simulation shows a vertical cross-section of a liver cell, from the extracellular space down to the nucleus:

1. **Click above the membrane** to release glucagon ligand particles.
2. A ligand binds a **GPCR**, activating its associated **G-protein** — the α subunit swaps GDP for GTP, detaches from the βγ pair, and slides along the membrane to **Adenylate Cyclase**.
3. Adenylate Cyclase activates and produces **cAMP**, the second messenger, which diffuses into the cytoplasm.
4. cAMP molecules bind **PKA (Protein Kinase A)** tetramers. Once enough cAMP has bound, PKA's catalytic subunits break free and head toward the nucleus.
5. Catalytic PKA entering the nucleus drives **CREB-mediated transcription**, visualized as glowing strands of mRNA — representing the cell's commitment to **glycogen breakdown**, the major physiological response of liver tissue to glucagon.

Phosphodiesterase (PDE) particles continuously degrade free cAMP in the background, and activated G-proteins automatically hydrolyze back to their resting state — so the whole system self-regulates, just like the real pathway.

The control panel lets you adjust ligand dosage, Adenylate Cyclase efficiency, PDE activity, and the number of PKA tetramers in the cytoplasm. You can also drag PKA tetramers directly into the cAMP plume to speed up the cascade.

## Project files

| File | Purpose |
|---|---|
| `index.html` | The complete simulation (HTML + CSS + JS, no build step) |
| `assets/proteins/` | Hand-drawn protein/molecule artwork used in the simulation |

## Running it locally

Just open `index.html` in any modern browser — no server or build tools required.
