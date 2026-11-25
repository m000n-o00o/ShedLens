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



## 2026-11-09
- **Goal:** Download the impact beating dataset and unzip it in Colab.
- **Done when:** The impact folder count prints 61.
- **What happened:**
  - Source: Attinger et al. (2018), Data in Brief, impact beating spatters (doi 10.1016/j.dib.2018.02.070).
  - Downloaded mmc2 (221.5 MB) and mmc3 (256 MB) one by one; sizes matched the page.
  - Stored in Drive as ShedLens/data/impact/impact_part1-2.zip.
  - Unzipped to /content/data/impact: 61 pattern folders.
  - Split paths into gunshot_dir and impact_dir in the setup cell.
  - A hidden NameError (img_dir) showed up after renaming paths.
  - Fixed the random sample with sorted() before random.sample. New gunshot sample: Hf72, H-a0-D300-1, Hr35, Hr31m, Hr28m, Hf77, H-a60-D150-3, H-a60-D150-1, Hr70, Hr27.
- **Learned:**
  - A notebook keeps old variables in memory, so a cell can work only because of a variable I already deleted from the code (img_dir). Restart session and run all is the real test.
  - random.seed fixes which positions get picked, not the list itself. rglob can return files in a different order after unzipping again, so I sort the list before random.sample. Running a cell twice in the same session doesn't prove the sample is fixed; a restart does.
- **Observations:**
  - H-a0-D300-1 shows grid lines and marks on the left edge (more support for observation 2).
- **Next:**
  - Measure the largest impact image; raise the pixel limit if needed.
  - Show 10 impact images and compare them with the gunshot grid.


 
## 2026-11-25
- **Goal:** Measure the largest impact image and compare 10 impact images with the gunshot grid.
- **Done when:** A 2x5 impact grid prints with folder names, and I write two lines on the difference between the classes.
- **What happened:**
  - Checked file types in impact_dir: {'.txt', '.jpg'}. The current exts already covers it.
  - Largest impact image: 888,360,000 pixels (~0.89B). Under the 1.3B limit, so no change needed.
  - Cleaned the copied comments in the impact measurement cell.
  - Impact grid sample (seed 42): HP_34, C5, C10, HP_54, HP_13, HP_11, HP_10, C6, HP_63, C4.
- **Learned:**
  - Set comprehension follows the same order as the loop: {what to add for item in where to look if condition}. A set keeps unique values only, so it answers "what kinds exist", not "how many".
  - PIL's default limit raises an error above ~179M pixels, so the impact scans also need the raised limit.
  - Running the impact grid overwrote image_paths and sample; the gunshot versions are gone until I rerun that cell.
- **Observations:**
  - Impact scans are clean (white paper, no labels or rulers), while many gunshot scans have text, scales and lines on the edges.
  - The two classes come from different papers and setups, so a model could learn these source differences instead of the spatter itself (leakage risk).
  - Impact stains almost disappear at thumbnail size (more support for observation 1).
  - Impact folders use two prefixes: HP_ and C. Meaning not checked yet.
  - HP_13 shows a faint vertical line in the middle; check at full resolution (crop, not thumbnail) before deciding what it is.
- **Next:**
  - Turn the grid cell into a function that takes a folder and a title, and use it for both classes.
  - Find out what HP_ and C mean (txt files or the 2018 paper).
  - Crop HP_13 at full resolution and check the vertical line.
  - Watch fast.ai Lesson 1 before deciding on cleaning and the model.
