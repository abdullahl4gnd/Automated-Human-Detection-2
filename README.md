# Automated Human Detection & Silhouette Segmentation
### LEEN350 — Image Processing | PETS 2009 Dataset

---

## Project Overview

This project detects and segments human silhouettes from surveillance video using classical image processing techniques only — no deep learning. The pipeline runs on the PETS 2009 outdoor pedestrian dataset and processes frames in real time on a standard CPU.

---

## Dataset

**PETS 2009 — Sequence S2/L1/Time_12-34/View_001**

- 795 frames at 768 × 576 pixels, ~7 fps
- Multiple pedestrians walking through an outdoor town square
- Download from Kaggle and place at:

```
C:\Users\wtpir\Documents\Datasets\image procc\Crowd_PETS09\S2\L1\Time_12-34\View_001\
```

To use a different path, change this line in the notebook:

```python
DATASET_BASE = r"C:\Users\wtpir\Documents\Datasets\image procc\Crowd_PETS09\S2\L1\Time_12-34"
```

---

## Requirements

```bash
pip install opencv-python numpy matplotlib
```

| Library | Version | Purpose |
|---|---|---|
| OpenCV | 4.x | Image processing, GMM |
| NumPy | any | Array operations |
| Matplotlib | any | Visualisation |

---

## Pipeline

```
Raw Frame
    │
    ▼
Gaussian Smoothing       5×5 kernel, σ=1.5 — removes sensor noise
    │
    ▼
GMM Background           MOG2, history=100, threshold=40.0
Subtraction              Marks moving pixels as foreground
    │
    ▼
Morphological Opening    3×3 ellipse — removes small noise blobs
    │
    ▼
Morphological Closing    15×15 ellipse — fills inter-leg gaps
    │
    ▼
Connected Component      Area filter: 400–50,000 px
Analysis                 Aspect ratio filter: 0.8–5.0 (height/width)
    │
    ▼
Detected Humans
```

---

## Parameters

| Parameter | Value | Effect |
|---|---|---|
| `GAUSSIAN_KERNEL` | 5 | Blur kernel size (must be odd) |
| `GAUSSIAN_SIGMA` | 1.5 | Blur strength |
| `GMM_HISTORY` | 100 | Frames to build background model |
| `GMM_THRESHOLD` | 40.0 | Sensitivity — lower = more detections, more noise |
| `OPEN_KERNEL_SIZE` | 3 | Opening kernel — removes tiny blobs |
| `CLOSE_KERNEL_SIZE` | 15 | Closing kernel — fills silhouette holes |
| `MIN_AREA` | 400 px | Minimum blob size to count as human |
| `MAX_AREA` | 50,000 px | Maximum blob size |
| `MIN_ASPECT` | 0.8 | Minimum height/width ratio |
| `MAX_ASPECT` | 5.0 | Maximum height/width ratio |

---

## Notebook Structure

| Cell | Step | What it does |
|---|---|---|
| 1 | Imports | Loads all libraries |
| 2 | Configuration | Set dataset path and parameters |
| 3 | Load Frames | Loads JPG/PNG/BMP frames from View_001 |
| 4 | Gaussian Smoothing | Applies blur and visualises noise removed |
| 5 | GMM Subtraction | Builds background model, shows mask evolution |
| 6 | Morphological Refinement | Opening + Closing, shows before/after |
| 7 | Human Detection | Connected components + bounding boxes |
| 8 | Full Pipeline | Runs all frames, stores results |
| 9 | Pipeline Visualisation | 6-panel figure at key frames |
| 10 | Detection Count | Plot of humans detected per frame |
| 11 | Quantitative Evaluation | Foreground ratio + GMM threshold sensitivity |

---

## How to Run

1. Install requirements
2. Set your dataset path in Cell 2
3. Run all cells top to bottom
4. Results appear inline as matplotlib figures

> **Note:** The GMM needs ~100 frames to warm up. Detection quality before frame 100 will be noisy. All evaluation metrics are computed after frame 100.

---

## Results

| Metric | Value |
|---|---|
| Average humans detected per frame (after warm-up) | ~2.5 |
| Max humans detected in one frame | 6 |
| Processing speed | ~45 fps |
| Best GMM threshold | 40.0 |

---

## Authors

- Ehab Saleh Gaafar Noor — 220303915
- Abdullah Al Obaidi — 220303936

**Course:** LEEN350 Image Processing and Lab
**University:** Istanbul Arel University — Faculty of Engineering
