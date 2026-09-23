# Brain Tumor MRI Segmentation with U-Net

Deep learning project for brain tumor segmentation from multimodal MRI using a 2D U-Net and the BraTS2020 dataset.

The project investigates two related tasks:

- **Binary segmentation:** whole tumor vs. background
- **Multi-class segmentation:** tumor sub-region segmentation

The main focus is on building a more reproducible and methodologically correct segmentation pipeline rather than proposing a new network architecture.

---

## Project Overview

Brain tumor segmentation is an important problem in medical image analysis because accurate identification of tumor regions can support diagnosis, treatment planning, and disease monitoring.

This project uses the **BraTS2020** dataset and a 2D U-Net implemented in PyTorch.

Two MRI modalities are used:

- FLAIR
- T1ce

The original 3D MRI volumes are converted into 2D axial slices and resized to 128 × 128 pixels.

---

## Tasks

### 1. Binary Whole-Tumor Segmentation

The first model predicts whether each pixel belongs to:

- Background
- Whole tumor

The model uses a single output channel and is trained using a combination of:

- Dice loss
- Binary Cross-Entropy loss

### 2. Multi-Class Tumor Segmentation

The second model predicts four classes:

- Background
- Necrotic / non-enhancing tumor core
- Peritumoral edema
- Enhancing tumor

The model uses a four-class output and is trained using:

- Dice loss
- Cross-Entropy loss

---

## Methodology

The main pipeline includes:

1. Loading BraTS2020 NIfTI volumes
2. Selecting FLAIR and T1ce modalities
3. Extracting axial MRI slices
4. Resizing images to 128 × 128
5. Per-channel min-max normalization
6. Nearest-neighbor interpolation for segmentation masks
7. Patient-level train / validation / test splitting
8. Training a 2D U-Net
9. Model selection using validation Dice
10. Evaluation using Dice-based segmentation metrics

Several methodological improvements were introduced compared with the original public baseline:

- Per-channel MRI normalization
- Integer-preserving mask resizing
- Batch normalization
- Dice-based compound loss
- Fixed-seed patient-level splitting
- Reproducible split fingerprints
- Validation-Dice-based checkpoint selection
- Standard BraTS WT / TC / ET evaluation

---

## Results

### Binary Whole-Tumor Segmentation

| Metric | Score |
|---|---:|
| Dice | 0.771 |
| Precision | 0.796 |
| Sensitivity | 0.651 |
| Specificity | 0.999 |

Increasing the training set improved whole-tumor Dice from **0.694 to 0.771**.

### Multi-Class Segmentation

| Region | Dice |
|---|---:|
| Necrotic / non-enhancing core | 0.196 |
| Edema | 0.598 |
| Enhancing tumor | 0.387 |
| Whole Tumor (WT) | 0.690 |
| Tumor Core (TC) | 0.743 |
| Enhancing Tumor (ET) | 0.735 |

The results show that smaller and more imbalanced tumor sub-regions are substantially harder to segment than the whole tumor.

---

## Repository Structure

```text
brain-tumor-mri-segmentation/
│
├── notebook/
│   ├── 01_binary_whole_tumor_unet.ipynb
│   ├── 02_multiclass_tumor_segmentation.ipynb
│   │
│   └── Ablation/
│       ├── ...
│       └── ...
│
├── requirements.txt
├── .gitignore
└── README.md
```
---

## Dataset

This project uses the **BraTS2020 Brain Tumor Segmentation dataset**.

Dataset:  
https://www.kaggle.com/datasets/awsaf49/brats20-dataset-training-validation

The dataset is not included in this repository.

---

## Limitations

This project is a reproducible experimental pipeline rather than a state-of-the-art BraTS system.

Main limitations:

- Reduced subset of the full BraTS2020 dataset
- Only FLAIR and T1ce modalities are used
- 2D slices instead of full 3D volumes
- Images downsampled to 128 × 128
- Strong class imbalance in smaller tumor regions

