# Assignment 1 — Automated Evaluation of Micro-sutures

**Course:** COL780 Computer Vision, IIT Delhi
**Author:** Somisetty Harsha Vardhan (2020CS10390)
**Deadline:** 5th February, 2024
**Problem statement:** [`COL780_A1.pdf`](COL780_A1.pdf)

> **No code is present in this folder.** Only the problem statement (`COL780_A1.pdf`) and the
> sample dataset (`data/`) are archived here. The `main.py` interface and run instructions below
> describe what the assignment *required*; they are not backed by a submitted implementation here.

---

## The problem

Micro-suturing is suturing performed under a microscope — an indispensable neurosurgical skill.
Automating the evaluation of a micro-suturing outcome lets trainee neurosurgeons be assessed
quickly, cheaply and without human bias. Given the *final image* of a set of sutures, the task is
to quantify quality using three parameters, computed with **classical computer vision only**:

1. Number of sutures
2. Inter-suture distance
3. Angulation of the sutures (relative to the x-axis)

**Constraint:** OpenCV may be used *only* for reading/writing images and for visualization. No
other inbuilt image-processing functions are allowed unless explicitly exempted — thresholding,
edge detection, component labelling, centroid/angle maths must be implemented manually.

## The four tasks (and marks)

| Task | Description | Marks |
|------|-------------|------:|
| **Task 1 — Number of micro-sutures** | Count the sutures in an image. Suggested pipeline: preprocess (resize, contrast, denoise, **threshold** background vs. sutures) → **Canny edge detection** → filter/refine by size & shape → count via **connected components / region growing**, or via **Harris corner detection** (corners ÷ a fixed constant). Contour hunting is an alternative. | 40 |
| **Task 2 — Inter-suture spacing** | Compute the **centroid** of each connected component, then distances between consecutive centroids (relative to image height). Report **mean and variance** of inter-suture distance; visualize centroids/distances on the original image. | 25 |
| **Task 3 — Angulation** | For each suture, compute the angle between the x-axis and the line from its centroid to a reference point (e.g. the leftmost point in the image). Report **mean and variance** of these angles. | 15 |
| **Task 4 — Comparison of two outcomes** | Given pairs of images, decide which image is "better" w.r.t. inter-suture distance and w.r.t. angulation, *independently*. The better image is the one with **lower variance** for that feature. | 10 |
| **Report** | Detailed write-up of the approach taken and experimentation details. | 10 |
| | **Total** | **100** |

## Required `main.py` interface

The submission had to include a `main.py` runnable in two modes:

```bash
# Part 1 — per-image parameters → CSV
python3 main.py 1 <img_dir> <output_csv>
```
Iterates over every image in `<img_dir>` and writes one row per image. Output CSV columns:

```
image_name, number of sutures,
mean inter suture spacing, variance of inter suture spacing,
mean suture angle wrt x-axis, variance of suture angle wrt x-axis
```

```bash
# Part 2 — pairwise comparison → CSV
python3 main.py 2 <input_csv> <output_csv>
```
Reads pairs of images and writes which one is better per feature.

- **Input CSV columns:** `img1_path, img2_path`
- **Output CSV columns:** `img1_path, img2_path, output_distance, output_angle`
- `output_distance` and `output_angle` are each **1** or **2**, indicating which image is better for
  that feature (lower variance wins).

A sample `output.csv` with expert-labelled ground truth for some pairs from `data/` was to be
provided by the course staff for Part 2.

## Dataset (`data/`)

A subset of a larger micro-suturing dataset. **Ten sample images**, `img1.png … img10.png`, are
provided here for development/testing; the course staff graded on a separate held-out test set of
similar images. The samples are **8-bit RGBA PNGs** of varying size (roughly 180–430 px wide by
250–850 px tall), each a microscope view of a row of sutures — e.g. `img1.png` is 404×742 and
`img10.png` is 427×848. The problem statement uses one "good" (evenly spaced, aligned) and one
"bad" outcome as illustrative examples.

## How it would be run

```bash
# Part 1: compute parameters for all sample images into a CSV
python3 main.py 1 data/ results.csv

# Part 2: given a CSV of image pairs, decide the better one per feature
python3 main.py 2 pairs.csv comparison.csv
```

## Submission notes (from the problem statement)

- Submit code + data + a detailed report, zipped as `ENTRY_NUMBER.zip` (e.g. `2020CS10390.zip`)
  via Moodle.
- Individual assignment; collaborators discussed with must be named in the report. Plagiarism →
  zero, with stricter action possible.

---

*This README is derived solely from `COL780_A1.pdf` and the contents of `data/`. No solution source
code or report is archived in this folder.*
