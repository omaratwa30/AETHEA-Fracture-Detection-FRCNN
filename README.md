# 🦴 AETHEA Fracture Detection System — Faster R-CNN Edition

## Deep Learning for Automated X-Ray Fracture Analysis Using Detectron2

[![Python](https://img.shields.io/badge/Python-3.12-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.10-red.svg)](https://pytorch.org/)
[![Detectron2](https://img.shields.io/badge/Detectron2-Faster%20R--CNN-orange.svg)](https://github.com/facebookresearch/detectron2)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📌 Project Information

| Field | Details |
|-------|---------|
| **Project Title** | AETHEA Fracture Detection System — Faster R-CNN |
| **Course** | SET 491 - Deep Learning |
| **Institution** | Egyptian Chinese University |
| **Semester** | Spring 2026 |
| **Instructor** | Dr. Hossam Hawash |
| **Students** | Andrew Wageh (192100099), Omar Atwa (192100141) |

---

## 📋 Project Overview

This repository presents a complete medical fracture detection pipeline using **Faster R-CNN** with **Detectron2** for automated X-ray analysis.

The project focuses exclusively on **two-stage object detection** using Faster R-CNN after removing all YOLO-related components from the original repository.

---

## 🩻 Body Regions Covered

- Hips & Pelvis
- Shoulder & Arm

---

## 🧠 Architecture Used

### Faster R-CNN (Detectron2)

A two-stage object detector consisting of:

1. **Region Proposal Network (RPN)**  
   Generates candidate fracture regions.

2. **ROI Head**  
   Performs fracture classification and bounding box refinement.

---

## 🎯 Clinical Objective

The system assists radiologists and medical professionals by:

- Detecting fractures in X-ray images
- Localizing fracture regions using bounding boxes
- Displaying confidence scores
- Improving diagnostic workflow efficiency
- Reducing missed fractures

> This system is designed as an AI-assisted medical screening tool and not as a replacement for professional diagnosis.

---

# ⭐ Primary Notebook Priority

## `arm-and-shoulder-faster-r-cnn-project.ipynb`

This is the **main and most important notebook** in the repository.

It contains the most advanced implementation of the entire project and should be considered the official Faster R-CNN training pipeline.

### Key Features Included

- Full Faster R-CNN training workflow
- Detectron2 integration
- Dataset registration
- Advanced OpenCV outlier filtering
- Blur detection
- Distortion filtering
- Zoom quality validation
- Training loss tracking
- Validation metrics
- TorchScript export
- Deployment ZIP packaging
- Energy consumption tracking
- CO₂ emissions dashboard
- Reproducibility testing
- Deterministic inference configuration

This notebook represents the final optimized architecture approved for the project.

---

# 📊 Model Performance Results

## Hips & Pelvis Dataset (12,218 X-ray images)

| Model | mAP50 | Precision | Recall | Inference Speed |
|-------|-------|-----------|--------|-----------------|
| Faster R-CNN | 69.7% | 62.3% | 51.9% | ~0.5 sec/image |

---

## Shoulder & Arm Dataset (27,480 X-ray images)

| Model | mAP50 | Precision | Recall | Inference Speed |
|-------|-------|-----------|--------|-----------------|
| Faster R-CNN | 48.6% | 43.68% | 41.76% | ~0.5 sec/image |

---

# 📁 Repository Structure

```text
AETHEA-Fracture-Detection-FRCNN/
│
├── datasets/
│   ├── hips_pelvis/
│   └── shoulder_arm/
│
├── docs/
│   └── report.pdf
│
├── models/
│   ├── hips_pelvis/
│   │   └── faster_rcnn/
│   │
│   └── shoulder_arm/
│       └── faster_rcnn/
│
├── notebooks/
│   ├── hips_pelvis/
│   │   └── hips-and-pelvis-faster-r-cnn.ipynb
│   │
│   └── shoulder_arm/
│       ├── arm-and-shoulder-faster-r-cnn-project.ipynb
│       └── arm-and-shoulder-faster-r-cnn.ipynb
│
├── interfaces/
│   ├── Arm_and_Shoulder_Faster_R-CNN_INTERFACE.ipynb
│   └── Hips_and_Pelvis_Faster_R-CNN_INTERFACE.ipynb
│
├── evaluation/
│   └── Hips_and_Pelvis_Faster_R-CNN_Evaluate.ipynb
│
├── scripts/
│   ├── dataset_sampling.py
│   ├── outlier_filtering.py
│   └── export_model.py
│
├── results/
│   ├── hips_pelvis/
│   └── shoulder_arm/
│
├── requirements.txt
├── LICENSE
└── README.md
```

---

# 🔧 Dataset Preparation

The project uses the same datasets from the original repository.

---

## Datasets Used

| Dataset | Body Region | Images | Classes |
|---------|-------------|--------|---------|
| Hips & Pelvis | Hips, Pelvis | 12,218 | 3 |
| Shoulder & Arm | Shoulder, Arm | 27,480 | 1 |

---

## Dataset Workflow

```text
Original Dataset
↓
Balanced Sampling
↓
Outlier Detection & Filtering
↓
Clean Dataset
↓
Faster R-CNN Training
```

---

## Sampling Strategy

Balanced subsets were created to improve training efficiency and reproducibility.

| Parameter | Value |
|-----------|-------|
| Train Split | 70% |
| Validation Split | 15% |
| Test Split | 15% |
| Random Seed | 42 |

---

## Outlier Filtering

The repository includes an OpenCV-based filtering pipeline that removes:

- Blurry X-rays
- Distorted images
- Over-zoomed fractures
- Incorrect bounding boxes

### Quality Filters Used

| Filter | Method | Threshold |
|--------|--------|-----------|
| Blur Detection | Laplacian Variance | < 50 |
| Over-Zoomed | Bounding Box Area | < 0.02 |
| Too Tight | Bounding Box Area | > 0.95 |
| Distorted | Aspect Ratio | > 6:1 |

### Performance Impact

Removing low-quality images improves Faster R-CNN performance by approximately **5–10% mAP50**.

---

# 🚀 Training Pipeline

The training workflow is implemented using Detectron2.

---

## Faster R-CNN Configuration

| Component | Value |
|-----------|------|
| Backbone | ResNet-50 + FPN |
| Framework | Detectron2 |
| Training Iterations | 15,000 |
| Score Threshold | 0.3 |
| NMS Threshold | 0.5 |

---

## Training Features

- GPU acceleration
- Dataset registration
- Automatic checkpointing
- Validation monitoring
- Loss tracking
- Metric visualization
- TorchScript export
- Deterministic training

---

## Training Notebook Priority

### Main Notebook
```text
notebooks/shoulder_arm/arm-and-shoulder-faster-r-cnn-project.ipynb
```

### Secondary Notebook
```text
notebooks/shoulder_arm/arm-and-shoulder-faster-r-cnn.ipynb
```

### Hips & Pelvis Notebook
```text
notebooks/hips_pelvis/hips-and-pelvis-faster-r-cnn.ipynb
```

---

# 💻 Inference Instructions

---

## Option 1: Jupyter Interface

```bash
jupyter notebook interfaces/Arm_and_Shoulder_Faster_R-CNN_INTERFACE.ipynb
```

or

```bash
jupyter notebook interfaces/Hips_and_Pelvis_Faster_R-CNN_INTERFACE.ipynb
```

---

## Features

- Upload X-ray image
- Adjustable confidence threshold
- Fracture bounding boxes
- Confidence scores
- Real-time visualization
- Medical assistance output

---

# 🔬 Reproducibility & Stability

The repository includes deterministic inference configuration:

```python
seed = 42
torch.backends.cudnn.deterministic = True
```

This ensures:

- Stable predictions
- Reproducible experiments
- Consistent evaluation

---

# 🌱 Sustainable AI Features

One of the unique components of this repository is the inclusion of:

- Energy usage tracking
- CO₂ emissions estimation
- Training efficiency analysis

The primary notebook includes a sustainability dashboard for monitoring environmental impact during model training.

---

# 📤 Model Export

Supported export formats:

- `.pth`
- TorchScript

The repository also includes export utilities for deployment preparation.

---

# 📊 Evaluation Metrics

The project evaluates Faster R-CNN using:

- mAP50
- mAP50-95
- Precision
- Recall
- Per-class AP

---

# 📦 Dependencies

Install all dependencies using:

```bash
pip install -r requirements.txt
```

---

## Core Dependencies

```text
torch
torchvision
detectron2
opencv-python
gradio
ipywidgets
pycocotools
matplotlib
numpy
Pillow
```

---

# 🎯 Key Results Summary

| Body Region | Model | mAP50 | Precision | Recall | Inference Time |
|-------------|------|-------|-----------|--------|----------------|
| Shoulder & Arm | Faster R-CNN | 48.6% | 43.68% | 41.76% | 0.5 sec |
| Hips & Pelvis | Faster R-CNN | 69.7% | 62.3% | 51.9% | 0.5 sec |

---

# ⚠️ Medical Disclaimer

**FOR RESEARCH AND EDUCATIONAL PURPOSES ONLY**

This project is an AI-assisted fracture detection system intended for academic and research applications.

All predictions and detections must be reviewed and verified by qualified medical professionals.

This system must not be used as the sole basis for diagnosis or treatment decisions.

---

# 📄 License

MIT License — see the LICENSE file for details.

---

# 📧 Contact

| Name | ID | Email |
|------|----|-------|
| Andrew Wageh | 192100099 | 192100099@ecu.edu.eg |
| Omar Atwa | 192100141 | 192100141@ecu.edu.eg |

Egyptian Chinese University — Faculty of Software Engineering  
Spring 2026
