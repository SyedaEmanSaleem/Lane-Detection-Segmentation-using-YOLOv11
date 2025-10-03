# Lane Detection Segmentation using YOLOv11

This repository provides a deep learning solution for **lane detection segmentation** using the **YOLOv11 segmentation model**. The project is implemented in Python using the **Ultralytics YOLO** framework and is optimized for training on custom lane datasets.

---

## 🔹 Project Overview

Lane detection is a crucial task in autonomous driving and advanced driver-assistance systems (ADAS). This project uses **YOLOv11-seg** to accurately segment lane markings, including **solid and dashed lines** on both left and right boundaries.

**Key Features:**

* Segment lanes in real-time
* Handles multiple lane types (solid/dashed)
* High precision and recall for both bounding boxes and masks
* Early stopping to prevent overfitting

---

## 🔹 Model Training

The model is trained using **YOLOv11-seg** with the following configuration:

```python
from ultralytics import YOLO

model = YOLO('yolo11n-seg.pt')
!yolo segment train \
  data="data.yaml" \
  model=yolo11n-seg.pt \
  epochs=200 \
  patience=30 \
  imgsz=640 \
  name="lane-seg"
```

**Training Details:**

* **Epochs:** 200 (training stopped early at 144 due to EarlyStopping)
* **Image size:** 640
* **Patience:** 30 epochs
* **Best Model:** `best.pt`
* **Hardware:** Tesla T4 GPU, CUDA acceleration

---

## 🔹 Performance Metrics

| Metric         | Value |
| -------------- | ----- |
| Box Precision  | 0.889 |
| Box Recall     | 0.855 |
| Box mAP50      | 0.908 |
| Box mAP50-95   | 0.747 |
| Mask Precision | 0.882 |
| Mask Recall    | 0.832 |
| Mask mAP50     | 0.889 |
| Mask mAP50-95  | 0.559 |

**Per Class Performance:**

| Lane Type               | Images | Instances | Box(P) | Box(R) | mAP50 | Mask(P) | Mask(R) | Mask mAP50 |
| ----------------------- | ------ | --------- | ------ | ------ | ----- | ------- | ------- | ---------- |
| Left Boundary - Dashed  | 158    | 356       | 0.935  | 0.762  | 0.888 | 0.931   | 0.742   | 0.877      |
| Left Boundary - Solid   | 138    | 139       | 0.932  | 0.942  | 0.952 | 0.938   | 0.942   | 0.956      |
| Right Boundary - Dashed | 86     | 172       | 0.862  | 0.779  | 0.847 | 0.840   | 0.730   | 0.796      |
| Right Boundary - Solid  | 172    | 174       | 0.826  | 0.937  | 0.944 | 0.914   | 0.926   | 0.613      |

---

## 🔹 Validation and Inference

After training, the best model `best.pt` was validated on a test set, showing **high accuracy and mask quality**.

**Inference Speed:**

* Preprocess: 0.1 ms per image
* Inference: 2.7 ms per image
* Postprocess: 4.3 ms per image

The trained model can be used to **segment lane markings on new images** with high reliability.

```python
results = model("/path/to/test/images", conf=0.6)
results.show()
```

---

## 🔹 Key Highlights

* Uses **YOLOv11-seg**, a lightweight yet powerful segmentation model
* EarlyStopping prevents overfitting and saves the best weights automatically
* Supports multiple lane types with high precision and recall
* Can be integrated into **autonomous driving pipelines** or lane monitoring systems

---

## 🔹 Future Improvements

* Train on a larger, more diverse dataset for improved generalization
* Deploy the model on edge devices for **real-time lane detection**
* Combine with **object detection** for a complete ADAS solution

---
