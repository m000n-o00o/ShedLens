# ShedLens Log

## 2026-11-06 / 2026-11-07
- **Goal:** Set up the repo, load the gunshot dataset into Drive, and display 10 images in Colab.
- **Done when:** 10 images appear in the notebook, each titled with its folder name.
- **What happened:**
  - Created the ShedLens repo with a Python .gitignore and added `data/` (dataset license is CC BY-NC-ND, so images stay out of the repo).
  - Data source: Attinger et al. (2019), Data in Brief, gunshot backspatters.
  - "Download all supplementary files" returned truncated zips; downloading each file separately worked.
  - The Google Drive link inside mmc5 is dead (404).
  - Renamed mmc2/3/4/6/7 to gunshot_part1-5 and stored them in Drive under ShedLens/data/gunshot.
  - Unzipped to Colab's local disk (/content/data/gunshot): 68 pattern folders, 71 images (some folders have more than one image).
  - Folder prefixes: H = handgun (confirmed in a txt file, 41 patterns), R = probably rifle (27, not yet confirmed).
  - Largest image is 40000 x 30000 (1.2B pixels), so I set Image.MAX_IMAGE_PIXELS to 1.3B.
  - Displayed 10 random images (seed 42) in a 2x5 grid.
- **Learned:**
  - How to unzip files in Colab with `!unzip`: quote the `*` so unzip sees it, and create the target folder first with `mkdir -p`.
  - How to find an image's size in pixels (width x height) without loading the whole image, and why PIL needs a higher pixel limit for very large scans.
  - Shrinking an image is not the same as compressing it. JPEG quality makes the file smaller, while `thumbnail()` reduces the number of pixels, which is what matters for memory and display.
  - Some `pathlib` tools look only at the first level (`iterdir`, `glob`), and `rglob` goes into every folder. When pulling files from deep folders, I need a filter, because the folders also hold txt files and extra images.
  - `matplotlib` is not only for charts. It draws images too, and `axis('off')` hides the axes around them.
  - Display numbers (`figsize`, `fontsize`, thumbnail size) are my choice. I try one and adjust. `figsize=(15, 6)` is the whole figure in inches, so each of the 2x5 cells gets about 3x3.
  - Where an error shows is not always where it comes from. The `seek` error came from inside PIL, but the cause was a comma in my code that put `None` into `sample`. Printing the variable found it.
- **Observations:**
  1. Patterns vary a lot; in some, the stains are so small they almost disappear after resizing.
  2. Some images have text and scale marks on the edges. These need to be checked or removed before training.
  3. Some scans show a long dark streak (e.g. H-a0-D200-1 (93), top of the image) that looks like blood running down, not spatter. Open question: does it appear in only one class? If so, the model could learn the streak instead of the spatter pattern.
- **Next:**
  - Check how many images have text or scale marks on the edges, and decide how to handle them.
  - Download the impact beating spatter dataset (61 patterns).
