# GPCR Signaling Cascade — User Guide

A self-contained, interactive simulation of how adrenaline triggers the GPCR → cAMP → PKA → CREB signaling pathway inside a cell. Built with HTML5 Canvas, no dependencies, no build step.

## How to open it

Double-click `index.html`, or open it from your browser with `File → Open`. Works fully offline in any modern browser (Chrome, Edge, Firefox).

Live demo (once pushed to GitHub Pages): `https://sungminnkimm.github.io/gpcr_signaling_cascade/`

## The layout

The screen is a vertical cross-section of a cell:

| Region | What's there |
|---|---|
| Top band (membrane) | 4 wavy 7-transmembrane GPCR receptors, each anchored to a G-protein heterotrimer (α/β/γ), plus a stationary Adenylate Cyclase enzyme |
| Middle (cytoplasm) | Floating inactive PKA tetramers (2 regulatory + 2 catalytic subunits), plus small ambient PDE (phosphodiesterase) particles |
| Bottom (nucleus) | A dim glowing semicircle with a transcription meter |

## How to trigger the cascade

1. **Click anywhere above the membrane.** This releases a burst of gold "adrenaline" ligand dots that fall down.
2. **A ligand hits a GPCR** → the receptor glows cyan, its G-protein swaps GDP for glowing GTP, and the α subunit detaches and slides sideways along the membrane toward Adenylate Cyclase.
3. **α-GTP reaches Adenylate Cyclase** → the enzyme glows orange and starts spraying tiny cyan cAMP particles down into the cytoplasm.
4. **4 cAMP particles hit the same PKA tetramer** → its regulatory (R) subunits fall away and two neon-pink catalytic (C) subunits break free, racing down toward the nucleus.
5. **Catalytic PKA enters the nucleus** → fills the Transcription Meter (top-right) by ~9% each time.
6. **Meter hits 100%** → climax: golden wavy "mRNA" strands rise out of the nucleus and a flashing banner reads **"CELLULAR RESPONSE: GLUCOSE METABOLISM INITIATED."** The meter then resets to 0.

The system also cleans up on its own:
- **PDE particles** drift through the cytoplasm and occasionally dissolve nearby cAMP.
- **α-GTP** automatically hydrolyzes back to α-GDP after a few seconds and slides back to reattach to its β/γ pair, resetting that receptor.
- Spent PKA tetramers quietly respawn fresh ones after breaking, keeping the cytoplasm populated.

## Controls (top-right glass panel)

| Slider | Effect |
|---|---|
| **Adrenaline Dosage** | How many ligand particles spawn per click (1–10) |
| **Adenylate Cyclase Efficiency** | How fast/much cAMP is produced once AC is activated (1–10) |
| **Phosphodiesterase (PDE) Activity** | How aggressively cAMP gets cleared from the cytoplasm (1–10) |

Turning dosage and AC efficiency up while PDE is low produces a faster, more dramatic cascade toward the climax. Turning PDE up keeps cAMP levels low and slows things down — useful for seeing the steady-state balance between production and degradation.

## Reading the chart

The strip chart at the bottom plots cytoplasmic cAMP concentration (particle count) over time, like a lab assay trace. Spikes correspond to Adenylate Cyclase bursts; decay corresponds to PDE clearance and consumption by PKA.

## Tips

- Click several times in different spots above the membrane to activate multiple receptors at once and watch several α subunits race to Adenylate Cyclase simultaneously.
- If nothing seems to be happening, check the chart — a flat line near zero means no GPCR has fired yet (click above the membrane).
- The simulation runs indefinitely and resets itself after each climax, so you can trigger the cascade repeatedly.
