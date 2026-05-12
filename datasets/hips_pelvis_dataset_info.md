# Hips & Pelvis Dataset

## Overview

| Property | Value |
|----------|-------|
| **Body Region** | Hips and Pelvis |
| **Total Images** | 12,218 |
| **Classes** | 3 (fracture, no fracture, pelvic_fracture) |
| **Annotation Format** | YOLO-style normalized bounding boxes |
| **Image Formats** | JPG, PNG |
| **Source** | Kaggle |
| **License** | Academic / Research |

---

# 📥 Original Dataset Source

- **Kaggle Dataset:**  
  https://www.kaggle.com/datasets/mohamedahmed12232/hips-and-pelvis-fraction-detection

- **Creator:** Mohamed Ahmed

- **Collection Method:** Combined from multiple medical imaging repositories.

---

# 📊 Class Distribution

| Class ID | Name | Training Instances | Validation Instances | Notes |
|----------|------|-------------------|---------------------|------|
| 0 | fracture | 9,591 | 196 | Hip fractures |
| 1 | no fracture | 7,356 | 181 | Non-fracture pelvic images |
| 2 | pelvic_fracture | 7,350 | 612 | Pelvic fracture class |

---

# 📂 Original Dataset Split

| Split | Images | Percentage |
|-------|--------|------------|
| Training | 11,840 | 96.9% |
| Validation | 184 | 1.5% |
| Test | 194 | 1.6% |
| **Total** | **12,218** | **100%** |

> The original dataset is heavily biased toward training samples.  
> A balanced sampling pipeline was created for the Faster R-CNN workflow.

---

# ⚖️ Sampled Dataset Used for Faster R-CNN

A balanced subset was generated to improve:
- Training stability
- Evaluation consistency
- Kaggle training compatibility
- Reproducibility

---

## Balanced Split

| Split | Original | Sampled | After Filtering |
|-------|----------|----------|----------------|
| Training | 11,840 | 2,800 | ~2,300 |
| Validation | 184 | 600 | ~490 |
| Test | 194 | 600 | ~490 |
| **Total** | **12,218** | **4,000** | **~3,280** |

---

# 🧹 Outlier Detection & Filtering

The repository includes OpenCV-based quality filtering before Faster R-CNN training.

---

## Removed Issues

| Issue | Count | Percentage |
|------|-------|------------|
| Blurry images | ~350 | 8.8% |
| Over-zoomed fractures | ~180 | 4.5% |
| Too-tight crops | ~120 | 3.0% |
| Distorted bounding boxes | ~80 | 2.0% |
| **Total Removed** | **~730** | **18.3%** |

---

## Filtering Methods

| Filter | Method | Threshold |
|--------|--------|-----------|
| Blur Detection | Laplacian Variance | < 50 |
| Over-Zoomed | Bounding Box Area | < 0.02 |
| Too Tight | Bounding Box Area | > 0.95 |
| Distorted | Aspect Ratio | > 6:1 |

---

# 🧠 Annotation Format

The dataset uses normalized YOLO-style bounding boxes.

```text
<class_id> <x_center> <y_center> <width> <height>
```

Example:

```text
0 0.512 0.438 0.185 0.218
1 0.750 0.620 0.120 0.150
2 0.300 0.500 0.080 0.100
```

---

# 🔄 Detectron2 Conversion

Although the annotations are stored in YOLO format, the repository converts them internally into Detectron2-compatible dataset dictionaries.

This enables:
- Faster R-CNN training
- Detectron2 inference
- Detectron2 evaluation
- COCO-style metrics

---

# 🧠 Faster R-CNN Training Pipeline

Training is implemented using:
- PyTorch
- Detectron2
- Faster R-CNN
- ResNet-50 + FPN backbone

---

## Faster R-CNN Results

| Metric | Score |
|--------|-------|
| mAP50 | 69.7% |
| mAP50-95 | 45.6% |
| Precision | 62.3% |
| Recall | 51.9% |
| Inference Speed | ~0.5 sec/image |
| Training Iterations | 15,000 |
| Training Time | ~3.5 hours |

---

# 🧪 Detectron2 Configuration

| Component | Value |
|-----------|------|
| Backbone | ResNet-50 + FPN |
| ROI Head | Faster R-CNN |
| Framework | Detectron2 |
| Score Threshold | 0.3 |
| NMS Threshold | 0.5 |

---

# 📈 Validation Output

```text
Average Precision (AP) @[ IoU=0.50:0.95 ] = 0.456
Average Precision (AP) @[ IoU=0.50      ] = 0.697
Average Precision (AP) @[ IoU=0.75      ] = 0.560
Average Recall    (AR) @[ IoU=0.50:0.95 ] = 0.514
```

---

# 📁 Folder Structure After Setup

```text
datasets/hips_pelvis/
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
└── filtered/
    ├── train/
    ├── val/
    └── test/
```

---

# 🚀 Usage in This Repository

## Step 1: Download Dataset

```bash
kaggle datasets download mohamedahmed12232/hips-and-pelvis-fraction-detection
```

---

## Step 2: Extract Dataset

```bash
unzip hips-and-pelvis-fraction-detection.zip -d datasets/hips_pelvis/raw/
```

---

## Step 3: Run Sampling Script

```bash
python scripts/dataset_sampling.py --dataset hips_pelvis --target 4000
```

---

## Step 4: Filter Outliers

```bash
python scripts/outlier_filtering.py --dataset hips_pelvis
```

---

## Step 5: Train Faster R-CNN

```bash
jupyter notebook notebooks/hips_pelvis/hips-and-pelvis-faster-r-cnn.ipynb
```

---

# ⚠️ Known Limitations

- Small validation dataset
- Class imbalance
- Limited pelvic fracture samples
- X-ray machine variability
- Multi-center imaging inconsistency

---

# 🔬 Future Improvements

| Priority | Improvement |
|----------|-------------|
| 1 | More pelvic fracture samples |
| 2 | DICOM support |
| 3 | Multi-view analysis |
| 4 | Clinical validation |
| 5 | Explainable AI visualization |

---

# 📚 References

- Detectron2:
  https://github.com/facebookresearch/detectron2

- COCO Dataset Format:
  https://cocodataset.org/#format-data

- OpenCV Documentation:
  https://docs.opencv.org

- Original Kaggle Dataset:
  https://www.kaggle.com/datasets/mohamedahmed12232/hips-and-pelvis-fraction-detection
