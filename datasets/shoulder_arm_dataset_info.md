# Shoulder & Arm Dataset

## Overview

| Property | Value |
|----------|-------|
| **Body Region** | Shoulder and Arm |
| **Total Images** | 27,480 |
| **Classes** | 1 (fracture) |
| **Annotation Format** | YOLO-style normalized bounding boxes |
| **Image Formats** | JPG, PNG |
| **Source** | Kaggle |
| **License** | Academic / Research |

---

# 📥 Original Dataset Source

- **Kaggle Dataset:**  
  https://www.kaggle.com/datasets/andrewwageh111/arm-and-shoulder-fracture-dataset

- **Creator:** Andrew Wageh

- **Preprocessing:** Filtered and organized from public bone fracture X-ray repositories.

---

# 📊 Dataset Composition

| Body Part | Training | Validation | Test | Total |
|-----------|----------|------------|------|-------|
| Shoulder | 12,665 | 2,235 | 549 | 15,449 |
| Arm | 14,387 | 2,502 | 662 | 17,348 |
| **Total** | **27,052** | **4,737** | **1,211** | **27,480** |

---

# 📂 Original Dataset Split

| Split | Images | Percentage |
|-------|--------|------------|
| Training | 27,052 | 98.4% |
| Validation | 4,737 | 17.2% |
| Test | 1,211 | 4.4% |

> A balanced subset was created for Faster R-CNN training and reproducibility.

---

# ⭐ Primary Faster R-CNN Training Pipeline

## Main Notebook

```text
arm-and-shoulder-faster-r-cnn-project.ipynb
```

This notebook represents:
- Final optimized Faster R-CNN workflow
- Advanced preprocessing pipeline
- Doctor-priority implementation
- Detectron2 training pipeline
- Deployment preparation workflow

---

# 🧹 Outlier Detection & Filtering

One of the most important features of this repository is the OpenCV-based filtering system.

The filtering pipeline removes:
- Blurry images
- Over-zoomed fractures
- Distorted scans
- Invalid bounding boxes

---

## Filtering Results

| Filter | Count | Percentage |
|--------|-------|------------|
| Blurry | ~480 | 8.0% |
| Over-zoomed | ~260 | 4.3% |
| Too tight | ~180 | 3.0% |
| Distorted | ~120 | 2.0% |
| **Total Removed** | **~1,040** | **17.3%** |

---

## Detection Thresholds

| Filter | Method | Threshold |
|--------|--------|-----------|
| Blur Detection | Laplacian Variance | < 50 |
| Over-Zoomed | Bounding Box Area | < 0.02 |
| Too Tight | Bounding Box Area | > 0.95 |
| Distorted | Aspect Ratio | > 6:1 |

---

# ⚖️ Sampled Dataset Used for Faster R-CNN

| Split | Original | Sampled | After Filtering |
|-------|----------|----------|----------------|
| Training | 27,052 | 4,200 | ~3,450 |
| Validation | 4,737 | 900 | ~740 |
| Test | 1,211 | 900 | ~740 |
| **Total** | **27,480** | **6,000** | **~4,930** |

---

# 🧠 Annotation Format

Binary fracture detection dataset.

All labels use class:

```text
0 = fracture
```

---

## Annotation Example

```text
0 0.512 0.438 0.185 0.218
```

---

# 🔄 Detectron2 Dataset Conversion

The repository converts YOLO-style labels into Detectron2-compatible dataset dictionaries.

This enables:
- Faster R-CNN training
- Detectron2 evaluation
- COCO-style metrics
- Detectron2 inference

---

# 🧠 Faster R-CNN Architecture

| Component | Value |
|-----------|------|
| Framework | Detectron2 |
| Detector | Faster R-CNN |
| Backbone | ResNet-50 + FPN |
| Training Iterations | 15,000 |
| Score Threshold | 0.3 |
| NMS Threshold | 0.5 |

---

# 📊 Faster R-CNN Results

| Metric | Score |
|--------|-------|
| mAP50 | 48.6% |
| Precision | 43.68% |
| Recall | 41.76% |
| Inference Speed | ~0.5 sec/image |
| Training Time | ~24 hours |

---

# 📈 Training Features

The main notebook includes:

- Detectron2 integration
- Dataset registration
- Advanced filtering
- Training loss logging
- Validation metrics
- TorchScript export
- Deployment ZIP packaging
- CO₂ dashboard
- Energy tracking
- Deterministic inference
- Reproducibility testing

---

# 🔬 Reproducibility Configuration

```python
seed = 42
torch.backends.cudnn.deterministic = True
```

This ensures:
- Stable predictions
- Reproducible results
- Consistent evaluation

---

# 📁 Folder Structure After Setup

```text
datasets/shoulder_arm/
├── raw/
│   ├── train/
│   ├── val/
│   └── test/
│
├── sampled/
│   ├── train/
│   ├── val/
│   └── test/
│
├── filtered/
│   ├── train/
│   ├── val/
│   └── test/
│
└── reports/
    └── outlier_report.json
```

---

# 🚀 Usage in This Repository

## Step 1: Download Dataset

```bash
kaggle datasets download andrewwageh111/arm-and-shoulder-fracture-dataset
```

---

## Step 2: Extract Dataset

```bash
unzip arm-and-shoulder-fracture-dataset.zip -d datasets/shoulder_arm/raw/
```

---

## Step 3: Create Balanced Sample

```bash
python scripts/dataset_sampling.py --dataset shoulder_arm --target 6000
```

---

## Step 4: Run Outlier Filtering

```bash
python scripts/outlier_filtering.py --dataset shoulder_arm
```

---

## Step 5: Train Faster R-CNN

```bash
jupyter notebook notebooks/shoulder_arm/arm-and-shoulder-faster-r-cnn-project.ipynb
```

---

# ⚠️ Known Limitations

- Binary fracture classification only
- No fracture subtype prediction
- Single-view X-ray analysis
- Limited generalization across imaging centers
- No DICOM-native pipeline

---

# 🌱 Sustainable AI Features

The primary notebook includes:
- Energy tracking
- CO₂ emissions monitoring
- Compute efficiency analysis

This makes the repository more deployment- and sustainability-oriented than standard academic projects.

---

# 🔬 Future Improvements

| Priority | Improvement |
|----------|-------------|
| 1 | Multi-class fracture classification |
| 2 | Multi-view fusion |
| 3 | DICOM integration |
| 4 | Explainable AI |
| 5 | Clinical deployment |

---

# 📚 References

- Detectron2:
  https://github.com/facebookresearch/detectron2

- OpenCV:
  https://docs.opencv.org

- Original Kaggle Dataset:
  https://www.kaggle.com/datasets/andrewwageh111/arm-and-shoulder-fracture-dataset
