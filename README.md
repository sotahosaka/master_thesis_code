# master_thesis_code

# Thesis Experiment Code (Jupyter Notebooks)

This repository contains the experimental programs used in my master’s thesis.
All experiments are provided as Jupyter Notebooks (`.ipynb`).

## Notebooks

| Notebook | Purpose | Main Outputs |
|---|---|---|
| `MakingDriftImages.ipynb` | Generate drift data (brightness change / camera dust) from raw images | Drift images (generated files in your output directory) |
| `MakingMobileNetAsBaseModel.ipynb` | Train MobileNetV1 as a base model using raw images, and evaluate using drift images | `BaseModel.h5` |
| `ValidationOfDriftDetectionMethods.ipynb` | Compare drift detection methods (proposed: Mahalanobis-based vs conventional: prediction probability-based) | `*_possibility_results.csv`, `mahalanobis_*.csv` |
| `ValidationOfLabellingMethods.ipynb` | Compare labelling methods (Mahalanobis-based vs KRR + Mahalanobis-based) | CSV logs/results (depends on your save paths) |
| `ValidationOfModelUpdateMethods.ipynb` | Compare model update methods: CORAL, DANN, Self-training, Fine-tuning, Relearning, Proposed (partial update with Score-CAM) | Trained models / evaluation results (depends on your save paths) |
| `CalculatingGazingScore.ipynb` | Calculate “Gazing Score” for selecting high-score blocks in the proposed update method | `whichlayer.csv` |
| `ValidationOfAllMethod.ipynb` | Final validation combining effective labelling + update methods | Summary results/plots (depends on your save paths) |

> ⚠️ **Important:** The notebooks include placeholder paths such as `C:/path/...` or `C:\Users\...`.
> Please replace them with paths in your own environment before running.

---

## Environment

Tested as notebooks. Recommended setup:

- Python 3.9+ (3.10 is usually safe)
- Jupyter Notebook / JupyterLab
- (Optional) NVIDIA GPU + CUDA for faster training

### Install dependencies

Create a virtual environment:

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate
```

Install packages:

```bash
pip install -U pip
pip install numpy pandas scipy scikit-learn matplotlib seaborn plotly opencv-python pillow natsort tensorflow keras
```

If you have a `requirements.txt`, prefer:

```bash
pip install -r requirements.txt
```

---

## Repository Structure (recommended)

A typical structure is:

```text
.
├── notebooks/
│   ├── MakingDriftImages.ipynb
│   ├── MakingMobileNetAsBaseModel.ipynb
│   ├── ValidationOfDriftDetectionMethods.ipynb
│   ├── ValidationOfLabellingMethods.ipynb
│   ├── ValidationOfModelUpdateMethods.ipynb
│   ├── CalculatingGazingScore.ipynb
│   └── ValidationOfAllMethod.ipynb
├── data/
│   ├── raw/                 # original images (wire raw images)
│   ├── drift/               # generated drift images
│   └── (optional splits)/   # train/val/test if you use them
├── outputs/
│   ├── models/              # BaseModel.h5 and updated models
│   ├── csv/                 # result CSVs
│   └── figures/             # plots/images
└── README.md
```

You can use any structure, but you must update paths inside the notebooks accordingly.

---

## How to Run

Launch Jupyter:

```bash
jupyter lab
# or
jupyter notebook
```

Then open notebooks and run cells from top to bottom.

### 1) Generate drift images

Run:

- `MakingDriftImages.ipynb`

You will need to set:
- input directory of raw images
- output directory for drift images
- drift parameters (brightness, dust settings, etc.)

### 2) Train base model

Run:

- `MakingMobileNetAsBaseModel.ipynb`

You will need to set:
- image glob patterns (e.g., `.../mode0/*.jpg`, `.../mode1/*.jpg`, ...)
- training parameters
- output path for `BaseModel.h5`

Output:
- `BaseModel.h5`

### 3) Validate each method

Run as needed:

- `ValidationOfDriftDetectionMethods.ipynb`
- `ValidationOfLabellingMethods.ipynb`
- `ValidationOfModelUpdateMethods.ipynb`

Notes:
- These notebooks read `BaseModel.h5`
- Some notebooks generate CSV results such as:
  - `source_possibility_results.csv`
  - `target1_possibility_results.csv`
  - `target2_possibility_results.csv`
  - `mahalanobis_cluster_distances.csv`
  - `mahalanobis_full_cluster_distances.csv`

### 4) Gazing Score calculation (for proposed method)

Run:

- `CalculatingGazingScore.ipynb`

Output:
- `whichlayer.csv` (selected layers/blocks and scores)

### 5) Final combined validation

Run:

- `ValidationOfAllMethod.ipynb`

This notebook is intended to combine effective labelling + update methods verified above.

---

## Notes / Troubleshooting

- If OpenCV import fails:
  - `pip install opencv-python`
- If TensorFlow GPU issues occur:
  - try CPU TensorFlow first, or match CUDA/cuDNN versions to your TensorFlow build.
- If you see imports like `gradcamutils` or `affine_a_method_det`:
  - they are expected to be available in the repository (e.g., as local `.py` files).
  - ensure the files exist in the repo root or in a package that Python can import.

---
