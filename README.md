# Verdant Drift 🪦

**An unsupervised vegetation-disturbance detector that worked, then tried to lie to everyone. So I stopped it.**

[![Open the postmortem](https://img.shields.io/badge/read-the%20full%20postmortem-c4533f)](https://www.lesselgeospatial.com/pages/null-island/verdant-drift/)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jerrod-lessel/verdant-drift/blob/main/verdant_drift_annotated.ipynb)

This is a stopped project. Most of my portfolio shipped. This one didn't, and it might be the most useful thing in it. The repo is here as the receipts.

---

## The idea 💡

Detect where Central Coast California vegetation got knocked around (fire, clearing, drought, slow die-off), then track whether it was coming back. No training labels, zero dollars.

The trick: Google's AlphaEarth embeddings give every 10-meter pixel a 64-number "fingerprint" per year. Build a stable fingerprint from 2017 to 2019, then measure how far each later year drifts from it using cosine distance. Big drift means something happened. No labels, no training data, just "this pixel stopped looking like itself." Hence the name.

- **Study area:** San Luis Obispo plus Santa Barbara counties
- **Stack:** Google Earth Engine, AlphaEarth embeddings, Sentinel-2, MTBS, USDA CDL, Python, Colab
- **Budget:** a free tier and some optimism

## The part that worked ✅

Validated against four known 2020 fires (Soda, Branch, Pond, Scorpion) and benchmarked head-to-head against classical dNBR. With **zero training labels**, the embedding approach hit roughly 0.5 recall and beat automated dNBR on every metric at every threshold, often by 2 to 3x on recall and IoU.

Low precision was expected and, honestly, correct. The fires are tiny needles in a two-county haystack, and the method flags all disturbance, so real drought stress and clearing got counted as "wrong" against a fire-only answer key.

## Why it got stopped 🔪

I extended the method to the full 2017 to 2025 record to classify recovery, and it told on itself. A sanity check claimed about **83% of undisturbed wild vegetation had recently changed** in 2025. It hadn't. The whole map had drifted from the wet 2017 to 2019 baseline because 2025 was dry.

The flaw: distance from a fixed baseline doesn't answer "did this pixel get disturbed." It answers "how different is this year from 2017 to 2019." A dry year makes everything look different. My detector was, in part, an expensive drought thermometer.

That flaw is fixable. But the headline feature was a **recovery** metric, measured at the end of the record, which was 2025, the single most poisoned year in the dataset. I could have shipped a gorgeous map that told landowners their forests weren't recovering when some absolutely were. So I stopped. A wrong answer that looks right is worse than no answer.

## What's in here 📓

- **`verdant_drift_annotated.ipynb`** — the full working notebook, narrated section by section, with outputs.

Fair warning: it will not run start to finish for a stranger. It points at a personal Earth Engine project and an exported asset, so treat it as evidence, not a tutorial. GitHub renders it inline, or open it in Colab with the badge above.

## What this demonstrates 🎯

Unsupervised disturbance detection on frontier satellite embeddings, a validation design that holds up against real fire perimeters, honest benchmark reasoning, and the judgment to stop a feature that looked good and was wrong.

The map died so the method could stay trustworthy.

---

Part of **Null Island**, the corner of my portfolio for projects I stopped on purpose. More at [lesselgeospatial.com](https://www.lesselgeospatial.com).

*Cause of death: interannual climate variability, and good judgment.*
