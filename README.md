# DisQUE-FS: Frequency-Split Appearance Statistics for Improved Full-Reference Image Quality Assessment

> A lightweight, training-free enhancement to the **Disentangled Quality Evaluator (DisQUE)** for Full-Reference Image Quality Assessment (FR-IQA).

---

## Overview

DisQUE-FS extends the original **DisQUE** framework by introducing **Frequency-Split Appearance Statistics (FSAS)**, a novel appearance representation that separates feature-map statistics into **low-frequency** and **high-frequency** components.

Instead of representing appearance using a single global standard deviation, DisQUE-FS computes:

- **Low-Frequency Standard Deviation (LF-STD)**
- **High-Frequency Standard Deviation (HF-STD)**

using a simple average-pooling decomposition.

This modification:

- requires **zero encoder retraining**
- requires **zero architectural modifications**
- adds only **minimal implementation changes**
- improves quality prediction performance across multiple benchmark datasets.

---

# Highlights

✔ Training-free enhancement

✔ Compatible with existing pretrained DisQUE checkpoints

✔ Improved feature representation using frequency-aware appearance statistics

✔ Supports dataset-wide feature extraction

✔ Compatible with:

- LIVE IQA
- CSIQ
- TID2013
- KADID-10K

---

# Proposed Method

Original DisQUE computes appearance features as

```
Appearance = Mean + Global Standard Deviation
```

DisQUE-FS replaces the global standard deviation with

```
Appearance = Mean
           + Low-Frequency Standard Deviation
           + High-Frequency Standard Deviation
```

where feature maps are decomposed into

- Low-frequency component
- High-frequency residual

using average pooling.

This richer statistical representation improves distortion discrimination without changing the backbone network.

---

# Repository Structure

```
DISQUE-FS/
│
├── disque/
│   ├── criteria/
│   ├── datasets/
│   ├── learning/
│   ├── models/
│   ├── utils/
│   └── __init__.py
│
├── download_data.py
├── extract_features.py
├── extract_features_from_dataset.py
├── process_image_using_example.py
├── process_all_sample_images.sh
├── train_disque.py
│
├── requirements.txt
├── README.md
└── LICENSE
```

---

# Installation

Clone the repository

```bash
git clone https://github.com/rahul09140102/DISQUE-FS.git

cd DISQUE-FS
```

Create a virtual environment

```bash
python -m venv .venv
```

Activate it

### Windows

```bash
.venv\Scripts\activate
```

### Linux / macOS

```bash
source .venv/bin/activate
```

Install dependencies

```bash
pip install -r requirements.txt
```

---

# Download Pretrained Models

Download pretrained checkpoints and sample data

```bash
python download_data.py
```

---

# Running the Project

## 1. Feature Extraction for a Single Reference–Distorted Pair

```bash
python extract_features.py \
    --ref_video path/to/reference \
    --dis_video path/to/distorted \
    --ckpt_path path/to/checkpoint.ckpt
```

---

## 2. Feature Extraction for an Entire Dataset

```bash
python extract_features_from_dataset.py \
    --dataset path/to/dataset \
    --ckpt_path path/to/checkpoint.ckpt \
    --processes 8
```

The extracted features are saved to disk and can subsequently be used for quality prediction.

---

## 3. Train the Quality Prediction Model

```bash
python train_disque.py
```

---

## 4. Example-Guided Image Processing

```bash
python process_image_using_example.py \
    --ckpt_path path/to/checkpoint.ckpt \
    --source_range <source_range> \
    --target_range <target_range> \
    --example_source_path <source_image> \
    --example_target_path <target_image> \
    --input_source_path <input_image> \
    --output_target_path <output_image>
```

---

## 5. Run All Sample Examples

```bash
./process_all_sample_images.sh
```

---

# Experimental Datasets

The implementation has been evaluated using the following benchmark datasets.

| Dataset | Evaluation |
|----------|------------|
| LIVE IQA | ✔ |
| CSIQ | ✔ |
| TID2013 | ✔ |
| KADID-10K | ✔ |

---

# Experimental Results

DisQUE-FS consistently improves the original DisQUE performance.

| Dataset | Original DisQUE | DisQUE-FS |
|---------|----------------:|----------:|
| LIVE IQA | 0.867 | **0.872** |
| CSIQ | 0.938 | **0.941** |
| TID2013 | 0.909 | **0.922** |
| KADID-10K | 0.934 | **0.936** |

---

# Research Contribution

Compared to the original implementation, this repository introduces:

- Frequency-Split Appearance Statistics (FSAS)
- Low-Frequency Standard Deviation
- High-Frequency Standard Deviation
- Improved appearance feature representation
- No retraining required
- Minimal implementation overhead
- Improved quality prediction performance

---

# Citation

If you use this repository, please cite both the original DisQUE paper and the DisQUE-FS paper.

### Original DisQUE

```bibtex
@article{venkataramanan2024disque,
  title={Joint Quality Assessment and Example-Guided Image Processing by Disentangling Picture Appearance from Content},
  author={Venkataramanan, A. K. and others},
  journal={arXiv preprint},
  year={2024}
}
```

### DisQUE-FS

```bibtex
@article{disquefs2026,
  title={DisQUE-FS: Frequency-Split Appearance Statistics for Improved Full-Reference Image Quality Assessment},
  author={Anonymous},
  year={2026}
}
```

(Replace this citation with the published version after acceptance.)

---

# Acknowledgements

This work is built upon the original **DisQUE** implementation developed by:

- A. K. Venkataramanan
- C. Stejerean
- I. Katsavounidis
- H. Tmar
- A. C. Bovik

The original repository has been extended with the proposed **Frequency-Split Appearance Statistics (FSAS)** for improved full-reference image quality assessment.

---

# License

This repository follows the license provided with the original DisQUE implementation.

Please refer to the `LICENSE` file for details.

---

# Contact

If you encounter issues or have suggestions, please open an Issue on this repository.

For research-related questions, feel free to contact the repository maintainer.
