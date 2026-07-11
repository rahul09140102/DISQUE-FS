# DisQUE-FS: Frequency-Split Appearance Statistics for Improved Full-Reference Image Quality Assessment

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)]()
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-red.svg)]()
[![License](https://img.shields.io/badge/License-MIT-green.svg)]()

Official implementation of **DisQUE-FS**, a lightweight improvement over **DisQUE** for Full-Reference Image Quality Assessment (FR-IQA).

This work introduces **Frequency-Split Appearance Statistics (FSAS)**, a training-free modification that replaces the global standard deviation used in DisQUE with separate **low-frequency** and **high-frequency** statistics. The modification improves distortion discrimination while requiring **no retraining** of the DisQUE encoder.

---

# Overview

DisQUE represents image appearance using spatial statistics extracted from intermediate feature maps.

The original implementation summarizes each feature map using

- Mean
- Global Standard Deviation

Our proposed method replaces the single standard deviation with

- Low-Frequency Standard Deviation (LF-STD)
- High-Frequency Standard Deviation (HF-STD)

obtained by decomposing feature maps into low-frequency and high-frequency components using average pooling.

This simple modification preserves the original DisQUE architecture while improving prediction accuracy across multiple IQA datasets.

---

# Proposed Method

For each feature map,

1. Apply Average Pooling

```
Feature Map
      │
Average Pool
      │
Low Frequency Component
```

2. Compute residual

```
High Frequency = Original − Low Frequency
```

3. Compute statistics

```
Mean
LF Standard Deviation
HF Standard Deviation
```

instead of

```
Mean
Global Standard Deviation
```

This increases the feature dimensionality from

```
8192 → 12288
```

without modifying the backbone network.

---

# Repository Structure

```
DISQUE-FS/
│
├── disque/                         # Original DisQUE framework
│   ├── models/
│   ├── learning/
│   ├── utils/
│   ├── criteria/
│   ├── datasets/
│   ├── disque_module.py
│   └── disque_fex.py
│
├── DisQUE_FS_Implementation.ipynb  # Main notebook (our implementation)
│
├── extract_features.py
├── extract_features_from_dataset.py
├── train_disque.py
├── process_image_using_example.py
├── process_all_sample_images.sh
├── download_data.py
├── requirements.txt
└── README.md
```

---

# Requirements

Install all dependencies

```bash
pip install -r requirements.txt
```

The project requires

```
Python 3.10+
PyTorch
TorchVision
NumPy
SciPy
Pandas
OpenCV
scikit-image
PyWavelets
TensorBoard
Lightly
Lightning
wandb
gdown
```

Additional packages are automatically installed from GitHub

```
videolib
qualitylib
tonemaplib
```

---

# Datasets

The experiments use

- TID2013
- KADID-10K

Download the datasets manually and update the paths inside the notebook.

Example

```
/content/drive/MyDrive/project_datasets/
```

or

```
/kaggle/input/
```

depending on your platform.

---

# Pretrained Model

The notebook uses the pretrained **DisQUE SDR checkpoint**.

Download the checkpoint using

```
download_data.py
```

or place the checkpoint manually inside

```
DisQUE_Checkpoints/
```

---

# Running the Project

## Step 1

Clone the repository

```bash
git clone https://github.com/rahul09140102/DISQUE-FS.git

cd DISQUE-FS
```

---

## Step 2

Install dependencies

```bash
pip install -r requirements.txt
```

---

## Step 3

Open

```
DisQUE_FS_Implementation.ipynb
```

This notebook contains the complete implementation.

---

## Step 4

Run the notebook sequentially.

The notebook performs

- Environment setup
- Imports
- DisQUE setup
- Loading pretrained checkpoint
- Dataset loading
- Feature extraction
- Frequency-Split Appearance Statistics
- Ridge Regression
- Nested Hyperparameter Search
- Performance evaluation
- Statistical analysis
- Visualization
- Final comparison tables

Run every cell from top to bottom.

---

# Experimental Pipeline

```
Reference Image
        │
        ▼
Pretrained DisQUE Encoder
        │
        ▼
Feature Maps
        │
        ▼
Frequency Split
        │
 ┌───────────────┐
 │ Low Frequency │
 └───────────────┘
        │
 ┌───────────────┐
 │ High Frequency│
 └───────────────┘
        │
        ▼
LF STD + HF STD
        │
        ▼
Feature Vector
        │
        ▼
Ridge Regression
        │
        ▼
Quality Score
```

---

# Evaluation

The notebook reproduces experiments on

## TID2013

- Leave-One-Reference-Out Cross Validation
- SROCC
- PLCC

## KADID-10K

- 5-Fold Cross Validation
- SROCC
- PLCC

It also generates

- Scatter plots
- Per-distortion analysis
- Performance tables
- Statistical comparisons

---

# Expected Outputs

Running the notebook generates

- Feature vectors
- Predicted MOS
- Scatter plots
- Correlation tables
- Distortion-wise performance
- Publication-ready figures

---

# Results

Compared to the original DisQUE,

DisQUE-FS

- Improves TID2013 performance
- Improves KADID-10K performance
- Improves distortion-specific prediction accuracy
- Requires no retraining
- Introduces only a lightweight feature extraction modification

---

# Main Contribution

✔ Training-free improvement over DisQUE

✔ Frequency-aware appearance representation

✔ Zero backbone modifications

✔ Zero encoder retraining

✔ Better distortion discrimination

✔ Improved SROCC and PLCC

---

# Citation

If you use this repository, please cite

```
@article{disquefs2026,
  title={DisQUE-FS: Frequency-Split Appearance Statistics for Improved Full-Reference Image Quality Assessment},
  author={Dikshant Singh et al.},
  year={2026}
}
```

and the original DisQUE paper

```
A. K. Venkataramanan,
C. Stejerean,
I. Katsavounidis,
H. Tmar,
A. C. Bovik,

Joint Quality Assessment and Example-Guided Image Processing by Disentangling Picture Appearance from Content.

arXiv, 2024.
```

---

# Acknowledgements

This work builds upon the original **DisQUE** framework proposed by

**A. K. Venkataramanan et al.**

We thank the original authors for making their implementation publicly available.

---

# License

MIT License
