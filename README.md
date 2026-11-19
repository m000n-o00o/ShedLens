# ShedLens

Classifying bloodstain pattern mechanisms (gunshot vs. impact) with transfer learning.

## Why

Bloodstain Pattern Analysis helps investigators reconstruct what happened at a scene. Telling a gunshot spatter from an impact (beating) spatter is one of its basic questions, and it is still mostly done by eye. ShedLens explores whether a deep learning model can learn the difference from scanned spatter patterns.

## Data

Two public datasets from Attinger et al., published in *Data in Brief*:

| Class | Source | Patterns |
|---|---|---|
| Gunshot (backspatter) | Attinger et al. (2019) | 68 patterns, 71 images |
| Impact (beating) | Attinger et al. (2018), doi: [10.1016/j.dib.2018.02.070](https://doi.org/10.1016/j.dib.2018.02.070) | 61 patterns |

The images are licensed CC BY-NC-ND, so they are **not stored in this repo**. They are kept on Google Drive and loaded into Colab.

## Status

Data collection and exploration (in progress).

- [x] Download both datasets and load them in Colab
- [x] Explore the images: sizes up to 40000 x 30000 pixels, patterns that vary a lot in scale
- [ ] Handle text, scale marks and grid lines on image edges
- [ ] Check for artifacts that appear in one class only, so the model does not learn them instead of the pattern
- [ ] Preprocess and split the data
- [ ] Train and compare transfer learning models

## Data observations so far

1. Patterns vary a lot. In some, the stains are so small they almost disappear after resizing.
2. Some images have text, scale marks or grid lines on the edges, which need to be removed before training.
3. Some scans show long dark streaks that look like blood running down rather than spatter. If they appear in one class only, the model could learn the streak instead of the pattern.

## Project log

Each work session is recorded in [LOG.md](LOG.md): the goal, what happened, what I learned, and what comes next.

## Tools

Python, Google Colab, Pillow, matplotlib, pathlib
