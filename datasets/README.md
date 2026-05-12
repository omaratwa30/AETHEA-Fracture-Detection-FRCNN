# Datasets Directory

## Overview

This directory contains documentation for the X-ray datasets used to train and evaluate the Faster R-CNN fracture detection models.

The actual image files are **not included** in this repository due to size constraints (over 40GB total).

This README explains:
- Where to obtain the datasets
- How the datasets were prepared
- The preprocessing workflow
- Sampling strategy
- Outlier filtering pipeline
- Faster R-CNN dataset preparation

---

# 📊 Available Datasets

| Dataset | Body Region | Total Images | Classes | Annotation Format | Size |
|----------|-------------|--------------|----------|------------------|------|
| Hips & Pelvis | Hips, Pelvis | 12,218 | 3 | YOLO-style bounding boxes | ~2 GB |
| Shoulder & Arm | Shoulder, Arm | 27,480 | 1 | YOLO-style bounding boxes | ~4 GB |

---

# 📥 Accessing the Original Datasets

## Hips & Pelvis Dataset

- **Kaggle Link:** https://www.kaggle.com/datasets/mohamedahmed12232/hips-and-pelvis-fraction-detection
- **Contents:** Hip and pelvis X-ray images with fracture annotations
- **Classes:**
  - 0 = fracture
  - 1 = no fracture
  - 2 = pelvic_fracture

---

## Shoulder & Arm Dataset

- **Kaggle Link:** https://www.kaggle.com/datasets/andrewwageh111/arm-and-shoulder-fracture-dataset
- **Contents:** Filtered shoulder and arm X-ray images
- **Classes:**
  - 0 = fracture

---

# 🔄 Dataset Preparation Workflow

```text
Original Dataset (Kaggle)
↓
Balanced Dataset Sampling
↓
Outlier Detection & Filtering
↓
Clean Dataset
↓
Detectron2 Dataset Registration
↓
Faster R-CNN Training
```

---

# 📦 Step 1: Download from Kaggle

```bash
# Install Kaggle CLI
pip install kaggle

# Download Hips & Pelvis dataset
kaggle datasets download mohamedahmed12232/hips-and-pelvis-fraction-detection

# Download Shoulder & Arm dataset
kaggle datasets download andrewwageh111/arm-and-shoulder-fracture-dataset

# Extract datasets
unzip hips-and-pelvis-fraction-detection.zip -d datasets/hips_pelvis/raw/
unzip arm-and-shoulder-fracture-dataset.zip -d datasets/shoulder_arm/raw/
```

---

# ⚖️ Step 2: Dataset Sampling

Due to Kaggle computational limitations and training efficiency requirements, balanced subsets are generated using:

```bash
python ../scripts/dataset_sampling.py
```

---

## Sampling Configuration

| Parameter | Value |
|-----------|------|
| Target Total | 6,000 images |
| Train Ratio | 70% |
| Validation Ratio | 15% |
| Test Ratio | 15% |
| Random Seed | 42 |

---

## Output Structure

```text
SAMPLED_DATASET_BALANCED/
├── train/
│   ├── images/
│   └── labels/
│
├── val/
│   ├── images/
│   └── labels/
│
└── test/
    ├── images/
    └── labels/
```

---

# 🧹 Step 3: Outlier Detection & Filtering

The repository includes an OpenCV-based image quality filtering system.

The filtering pipeline automatically removes:
- Blurry X-rays
- Over-zoomed fractures
- Distorted bounding boxes
- Low-quality scans

Run the filtering script:

```bash
python ../scripts/outlier_filtering.py
```

---

## Filtering Thresholds

| Filter | OpenCV Method | Threshold | Purpose |
|--------|---------------|-----------|---------|
| Blur Detection | Laplacian Variance | < 50 | Remove blurry scans |
| Over-Zoomed | Bounding Box Area | < 0.02 | Fracture too small |
| Too Tight | Bounding Box Area | > 0.95 | Missing anatomical context |
| Distorted | Aspect Ratio | > 6:1 | Invalid bounding boxes |

---

## Sample Filtering Output

```text
Scanning train: 100%|██████████| 4200/4200
TRAIN: 752/4200 outlier images (17.9%)

WARNING blurry_image_001.jpg -> BLURRY
WARNING tiny_fracture_023.jpg -> OVER-ZOOMED

Validation: 148/900 outlier images
Test: 142/900 outlier images
```

---

## Performance Impact

Removing low-quality samples improves Faster R-CNN performance by approximately:

```text
+5% to +10% mAP50
```

---

# 🧠 Annotation Format

The original datasets use YOLO-style normalized bounding boxes.

```text
<class_id> <x_center> <y_center> <width> <height>
```

Example:

```text
0 0.512 0.438 0.185 0.218
```

---

## Coordinate Explanation

| Field | Description |
|------|-------------|
| class_id | Object class index |
| x_center | Bounding box center X |
| y_center | Bounding box center Y |
| width | Bounding box width |
| height | Bounding box height |

All coordinates are normalized between:

```text
0.0 → 1.0
```

---

# 🔄 Detectron2 Conversion

Although the datasets use YOLO-style annotations, the repository converts them internally into Detectron2-compatible dataset dictionaries.

This enables:
- Faster R-CNN training
- Detectron2 evaluation
- Detectron2 inference
- COCO-style metrics

---

# 📂 Directory Structure

```text
datasets/
├── README.md
├── hips_pelvis_dataset_info.md
├── shoulder_arm_dataset_info.md
│
├── hips_pelvis/
│   ├── raw/
│   └── processed/
│
└── shoulder_arm/
    ├── raw/
    └── processed/
```

---

# ⚠️ Important Notes

The following folders are excluded from Git:

```text
raw/
processed/
filtered/
sampled/
```

because they contain large medical image files.

Only documentation and scripts are tracked.

---

# 🔬 Detectron2 Dataset Registration

Datasets are registered using:

```python
DatasetCatalog.register(...)
MetadataCatalog.get(...)
```

The training notebooks automatically:
- Load annotations
- Convert labels
- Register datasets
- Build Faster R-CNN dataloaders

---

# 📈 Dataset Usage in This Repository

## Shoulder & Arm

Main notebook:

```text
notebooks/shoulder_arm/arm-and-shoulder-faster-r-cnn-project.ipynb
```

This notebook contains:
- Full training pipeline
- Outlier filtering
- Detectron2 training
- TorchScript export
- CO₂ dashboard

---

## Hips & Pelvis

Training notebook:

```text
notebooks/hips_pelvis/hips-and-pelvis-faster-r-cnn.ipynb
```

Supports:
- Multi-class fracture detection
- Faster R-CNN evaluation
- Detectron2 inference

---

# 📚 References

- Detectron2 Documentation:
  https://github.com/facebookresearch/detectron2

- COCO Dataset Format:
  https://cocodataset.org/#format-data

- OpenCV Documentation:
  https://docs.opencv.org

- Kaggle Dataset Sources:
  See individual dataset documentation files
