# COL780: Computer Vision

**Author:** Somisetty Harsha Vardhan
**Entry Number:** 2020CS10390
**Program:** IIT Delhi, Computer Science & Engineering (CSE)

This folder collects my coursework for **COL780 - Computer Vision** at IIT Delhi. COL780 is the
department's classical-and-modern computer vision course: it walks through image formation and
processing, filtering, edge and corner detection, contours, connected-component analysis,
segmentation, feature matching, and the geometry that underpins later deep-learning vision. The
recurring theme in the early part of the course is building real measurements out of raw pixels
using algorithms I implement myself, rather than calling a high-level library function.

---

## What this folder holds (be honest)

I have a single assignment archived here, and only its **problem statement and sample dataset** —
**there is no submitted source code** (`main.py` / Python files) and no report in this folder. So
the write-ups below describe what the assignment *asked for* and the pipeline it *expects*, not a
solution I am shipping here. If I ever recover the code I wrote at submission time, it would slot
into `submissions/ass1-microsuture-evaluation/`.

```
col780-computer-vision/
├── README.md                              <- this journal-style overview
└── submissions/
    └── ass1-microsuture-evaluation/
        ├── README.md                      <- per-assignment problem + interface spec
        ├── COL780_A1.pdf                  <- the original assignment problem statement
        └── data/                          <- 10 sample micro-suturing images (img1..img10.png)
```

---

## Assignment 1: Automated Evaluation of Micro-sutures

**Deadline:** 5th February, 2024

### The problem, in plain terms

Micro-suturing is stitching done under a microscope — an indispensable neurosurgical skill that is
slow and expensive to train and grade by hand. The assignment asks: given the *final image* of a
set of sutures, can I quantify how good the suturing was, purely with **classical computer vision**?
The quality is captured by three measurable parameters:

1. **Number of sutures** in the image.
2. **Inter-suture distance** — how evenly spaced consecutive sutures are.
3. **Suture angulation** — the angle each suture makes with the x-axis (well-aligned, near-parallel
   sutures are the ideal).

The hard constraint that makes this a *classical* CV exercise: I may use OpenCV only to read/write
and visualize images. Every actual image-processing step — thresholding, edge detection, component
labelling, centroid maths — has to be done without leaning on high-level inbuilt functions.

### The pipeline it expects

The assignment is graded out of 100 (4 implementation tasks + a written report) and sketches a
classical pipeline I would build top to bottom:

- **Preprocess** each image: resize, adjust contrast, reduce noise, and **threshold** to separate
  the dark sutures from the lighter skin/background.
- **Detect edges** with **Canny** (or a comparable edge detector) to outline each suture.
- **Filter and refine** the edge map by size/shape to drop spurious detections.
- **Count** sutures by grouping edge pixels — via **connected components / region growing**, or
  alternatively via **Harris corner detection** (number of corners divided by a fixed constant).
  Contour hunting is mentioned as another valid route.
- **Locate centroids** of each connected component, then derive **inter-suture spacing** (distances
  between consecutive centroids, mean and variance, relative to image height).
- **Compute angulation** as the angle between the line (centroid -> a reference point such as the
  leftmost point) and the x-axis, again reporting mean and variance.
- **Compare two outcomes** on each feature independently: the better image is the one with *lower
  variance* for that feature.

The detailed task breakdown, the exact `main.py` CSV interface (two run modes), the dataset notes,
and the marks per task live in the assignment's own README at
[`submissions/ass1-microsuture-evaluation/README.md`](submissions/ass1-microsuture-evaluation/README.md).

---

## What I learned / skills

- Building an end-to-end **classical CV measurement pipeline** from raw pixels — and the discipline
  of doing thresholding, edge detection and component labelling *by hand* instead of reaching for a
  one-line library call.
- **Canny edge detection** and **Harris corner detection** as practical, tunable tools, and how
  sensitive they are to preprocessing and parameter choices on noisy real-world images.
- **Connected-component analysis and centroids** as a bridge from "pixels" to "objects I can count
  and measure" (spacing, angle).
- Turning fuzzy quality notions ("are these sutures even and parallel?") into **statistics** —
  mean and variance — and using variance as a comparison criterion.
- Practical engineering details: a clean **CSV-driven CLI** (`main.py <part_id> ...`) for batch
  evaluation, and visualizing intermediate results (centroids, distances) to debug a vision
  pipeline.

*Note: this overview is based solely on `COL780_A1.pdf` and the `data/` sample images archived here;
no solution code is present.*
