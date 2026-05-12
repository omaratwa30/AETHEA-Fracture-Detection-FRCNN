# Models Directory

This directory contains all trained Faster R-CNN models for the AETHEA Fracture Detection System.

The repository focuses exclusively on:
- Faster R-CNN
- Detectron2
- Medical fracture detection
- X-ray object detection

All YOLO-related architectures, exports, and dependencies were removed from this repository version.

---

# 📂 Directory Structure

```text
models/
│
├── hips_pelvis/
│   └── faster_rcnn/
│       ├── model_final.pth
│       ├── config.yaml
│       └── inference_interface.ipynb
│
└── shoulder_arm/
    └── faster_rcnn/
        ├── model_final.pth
        ├── config.yaml
        └── inference_interface.ipynb
```

---

# 📊 Model Summary

| Body Region | Architecture | mAP50 | Precision | Recall | File Size | Format |
|-------------|--------------|-------|-----------|--------|-----------|--------|
| Hips & Pelvis | Faster R-CNN | 69.7% | 62.3% | 51.9% | ~315 MB | `.pth` |
| Shoulder & Arm | Faster R-CNN | 48.6% | 43.68% | 41.76% | ~315 MB | `.pth` |

---

# 📥 Model Downloads

All trained Faster R-CNN weights are available through Google Drive:

📁 https://drive.google.com/drive/folders/1flUJU7YHtcPYDW7VHaFsfCByOGyQq9mj?usp=sharing

> Model weights are not uploaded directly to GitHub because of file size limitations.

After downloading:
- Place the files inside their corresponding directories
- Keep the exact folder structure shown above

---

# 🧠 Faster R-CNN Architecture

The repository uses Faster R-CNN implemented with Detectron2.

---

## Architecture Components

| Component | Description |
|-----------|-------------|
| Backbone | ResNet-50 + FPN |
| Detector | Faster R-CNN |
| Framework | Detectron2 |
| Region Proposal | RPN |
| ROI Head | Fully Connected ROI Head |

---

# ⚙️ Detectron2 Configuration

Common training configuration:

| Parameter | Value |
|-----------|------|
| Backbone | ResNet-50-FPN |
| Score Threshold | 0.3 |
| NMS Threshold | 0.5 |
| Anchor Sizes | `[32, 64, 128, 256, 512]` |
| ROI FC Layers | 1024-dim |
| Training Iterations | 15,000 |

---

# 🩻 Hips & Pelvis Faster R-CNN

## Performance

| Metric | Value |
|--------|-------|
| mAP50 | 69.7% |
| mAP50-95 | 45.6% |
| Precision | 62.3% |
| Recall | 51.9% |
| Inference Speed | ~0.5 sec/image |
| Training Time | ~3.5 hours |

---

## Validation Output

```text
Average Precision (AP) @[ IoU=0.50:0.95 ] = 0.456
Average Precision (AP) @[ IoU=0.50      ] = 0.697
Average Precision (AP) @[ IoU=0.75      ] = 0.560
Average Recall    (AR) @[ IoU=0.50:0.95 ] = 0.514
```

---

## Files

| File | Description |
|------|-------------|
| `model_final.pth` | Final trained weights |
| `config.yaml` | Detectron2 configuration |
| `inference_interface.ipynb` | Jupyter inference notebook |

---

# 🦴 Shoulder & Arm Faster R-CNN

## Performance

| Metric | Value |
|--------|-------|
| mAP50 | 48.6% |
| Precision | 43.68% |
| Recall | 41.76% |
| Inference Speed | ~0.5 sec/image |
| Training Time | ~24 hours |

---

# ⭐ Primary Training Notebook

The main Faster R-CNN implementation for this repository is:

```text
arm-and-shoulder-faster-r-cnn-project.ipynb
```

This notebook contains:
- Advanced Detectron2 pipeline
- OpenCV outlier filtering
- Blur detection
- Distortion filtering
- Training monitoring
- TorchScript export
- Deployment preparation
- Energy tracking
- CO₂ emissions dashboard

This notebook represents the final optimized Faster R-CNN workflow.

---

## Files

| File | Description |
|------|-------------|
| `model_final.pth` | Final trained weights |
| `config.yaml` | Detectron2 configuration |
| `inference_interface.ipynb` | Jupyter inference notebook |

---

# 🚀 How to Use These Models

---

## Option 1: Jupyter Notebook Interface

### Shoulder & Arm

```bash
jupyter notebook interfaces/Arm_and_Shoulder_Faster_R-CNN_INTERFACE.ipynb
```

### Hips & Pelvis

```bash
jupyter notebook interfaces/Hips_and_Pelvis_Faster_R-CNN_INTERFACE.ipynb
```

---

# 🧠 Option 2: Python Inference

```python
from detectron2.config import get_cfg
from detectron2.engine import DefaultPredictor
import cv2

# Load Detectron2 configuration
cfg = get_cfg()
cfg.merge_from_file("config.yaml")

# Load model weights
cfg.MODEL.WEIGHTS = "model_final.pth"

# Set score threshold
cfg.MODEL.ROI_HEADS.SCORE_THRESH_TEST = 0.3

# Create predictor
predictor = DefaultPredictor(cfg)

# Load image
image = cv2.imread("xray.jpg")

# Run inference
outputs = predictor(image)

print(outputs)
```

---

# 📤 TorchScript Export

The primary notebook supports TorchScript export for deployment.

Example:

```python
traced_model = torch.jit.trace(model, example_input)
traced_model.save("model.torchscript")
```

---

# 🔬 Reproducibility Configuration

The repository includes deterministic inference settings:

```python
seed = 42
torch.backends.cudnn.deterministic = True
```

This ensures:
- Stable predictions
- Reproducible evaluation
- Consistent inference behavior

---

# 🌱 Sustainable AI Features

The main Faster R-CNN pipeline includes:
- Energy consumption tracking
- CO₂ emissions estimation
- Training efficiency analysis

This makes the repository more deployment-oriented and sustainability-aware.

---

# 📈 Inference Performance

| Model | GPU Memory | CPU Memory | FPS (GPU) | FPS (CPU) |
|------|-------------|------------|-----------|-----------|
| Faster R-CNN | ~4.5 GB | ~6 GB | ~2 FPS | <1 FPS |

---

# ⚠️ Common Issues

---

## GPU Out of Memory

Reduce image size:

```python
cfg.INPUT.MIN_SIZE_TRAIN = (512,)
cfg.INPUT.MAX_SIZE_TRAIN = 800
```

---

## No Predictions Detected

Lower the confidence threshold:

```python
cfg.MODEL.ROI_HEADS.SCORE_THRESH_TEST = 0.2
```

---

## Detectron2 Installation Issues

Install compatible versions:

```bash
pip install torch torchvision
pip install detectron2
```

---

# 📁 Recommended Folder Placement

```text
models/
├── hips_pelvis/
│   └── faster_rcnn/
│       └── model_final.pth
│
└── shoulder_arm/
    └── faster_rcnn/
        └── model_final.pth
```

---

# 🔬 Future Improvements

| Priority | Improvement |
|----------|-------------|
| 1 | Faster inference optimization |
| 2 | ONNX export |
| 3 | DICOM support |
| 4 | Clinical deployment |
| 5 | Multi-view fracture analysis |

---

# 📚 References

- Detectron2:
  https://github.com/facebookresearch/detectron2

- PyTorch:
  https://pytorch.org

- OpenCV:
  https://docs.opencv.org
