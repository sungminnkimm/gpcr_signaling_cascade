# GPCR Signaling Cascade — User Guide

A self-contained, interactive simulation of how glucagon triggers the GPCR → cAMP → PKA → CREB signaling pathway in a liver cell, driving glycogen breakdown. Built with HTML5 Canvas, no dependencies, no build step.

## How to open it

Double-click `index.html`, or open it from your browser with `File → Open`. Works fully offline in any modern browser (Chrome, Edge, Firefox).

Live demo (once GitHub Pages is enabled): `https://sungminnkimm.github.io/gpcr_signaling_cascade/`

## The layout

The screen is a vertical cross-section of a liver cell:

| Region | What's there |
|---|---|
| Top band (membrane) | 4 wavy 7-transmembrane GPCR receptors, each anchored to a G-protein heterotrimer (α/β/γ), plus a stationary Adenylate Cyclase enzyme |
| Middle (cytoplasm) | Floating inactive PKA tetramers (2 regulatory + 2 catalytic subunits), plus small ambient PDE (phosphodiesterase) particles |
| Bottom (nucleus) | A dim glowing semicircle with a transcription meter |

## How to trigger the cascade

1. **Click above the membrane.** This releases a burst of gold "glucagon" ligand dots that fall down.
2. **A ligand hits a GPCR** → the receptor glows cyan, its G-protein swaps GDP for glowing GTP, and the α subunit detaches and slides sideways along the membrane toward Adenylate Cyclase.
3. **α-GTP reaches Adenylate Cyclase** → the enzyme glows orange and starts spraying tiny cyan cAMP particles down into the cytoplasm.
4. **4 cAMP particles hit the same PKA tetramer** → each hit docks a visible cAMP particle onto the tetramer; once all 4 slots are filled, the regulatory (R) subunits fall away and two neon-pink catalytic (C) subunits break free, racing down toward the nucleus.
5. **Catalytic PKA enters the nucleus** → fills the Transcription Meter (top-right) by ~9% each time.
6. **Meter hits 100%** → climax: golden wavy "mRNA" strands rise out of the nucleus and a flashing banner reads **"CELLULAR RESPONSE: GLYCOGEN BREAKDOWN INITIATED."** The meter then resets to 0.

You can also **drag any unbroken PKA tetramer** directly into the cAMP plume below Adenylate Cyclase to speed up binding instead of waiting for random drift collisions — click and hold on a tetramer, move it, release to let it float freely again.

The system also cleans up on its own:
- **PDE particles** drift through the cytoplasm and occasionally dissolve nearby cAMP.
- **α-GTP** automatically hydrolyzes back to α-GDP after a few seconds and slides back to reattach to its β/γ pair, resetting that receptor.
- Spent PKA tetramers quietly respawn fresh ones after breaking, keeping the cytoplasm populated at your chosen density.

## Controls (top-right glass panel)

| Slider | Effect |
|---|---|
| **Glucagon Dosage** | How many ligand particles spawn per click (1–10) |
| **Adenylate Cyclase Efficiency** | How fast/much cAMP is produced once AC is activated (1–10) |
| **Phosphodiesterase (PDE) Activity** | How aggressively cAMP gets cleared from the cytoplasm (1–10) |
| **PKA Density** | How many PKA tetramers float in the cytoplasm at once (3–20) — more tetramers means more chances for cAMP to land a hit |

Turning dosage and AC efficiency up while PDE is low produces a faster, more dramatic cascade toward the climax. Turning PDE up keeps cAMP levels low and slows things down — useful for seeing the steady-state balance between production and degradation. Raising PKA Density (or dragging tetramers into the cAMP plume) is the fastest way to reach the climax if the cascade feels slow.

## Reading the chart

The strip chart at the bottom plots cytoplasmic cAMP concentration (particle count) over time, like a lab assay trace. Spikes correspond to Adenylate Cyclase bursts; decay corresponds to PDE clearance and consumption by PKA.

## Tips

- Click several times in different spots above the membrane to activate multiple receptors at once and watch several α subunits race to Adenylate Cyclase simultaneously.
- If nothing seems to be happening, check the chart — a flat line near zero means no GPCR has fired yet (click above the membrane).
- Drag a PKA tetramer right under Adenylate Cyclase right after it fires to catch the cAMP burst directly — much faster than waiting for drift.
- The simulation runs indefinitely and resets itself after each climax, so you can trigger the cascade repeatedly.
