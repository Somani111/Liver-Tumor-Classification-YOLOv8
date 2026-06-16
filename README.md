# 🫀 Liver Tumor Classification using YOLOv8


**A deep learning pipeline for automated liver tumor classification into Cholangiocarcinoma, HCC (Hepatocellular Carcinoma), and Normal Liver tissue using YOLOv8.**

[Overview](#-overview) • [Classes](#-tumor-classes) • [Pipeline](#-pipeline) • [Getting Started](#-getting-started) • [Results](#-results) • [Improvements](#-future-improvements) • [Report](#-report)

</div>

---

## 📌 Overview

Liver cancer is one of the leading causes of cancer-related mortality worldwide. Early and accurate classification of liver tumors from medical imaging is critical for timely treatment. This project leverages **YOLOv8's classification model** — state-of-the-art in real-time object detection and classification — to distinguish between three categories of liver tissue from CT scan images.

| Feature | Detail |
|---|---|
| Model | YOLOv8n-cls (nano classification) |
| Training Epochs | 50 |
| Image Size | 128×128 |
| Framework | Ultralytics + PyTorch |
| Platform | Google Colab (GPU) |

---

## 🎯 Tumor Classes

| Class | Description |
|---|---|
| 🔴 **Cholangiocarcinoma** | Bile duct cancer — originates in the bile ducts inside or outside the liver |
| 🟡 **HCC (Hepatocellular Carcinoma)** | Most common primary liver cancer, arising from hepatocytes |
| 🟢 **Normal Liver** | Healthy liver tissue with no tumor presence |

---

## 🔄 Pipeline

```
Raw CT Images
     │
     ▼
┌─────────────────────────┐
│   Image Preprocessing   │  → Normalization, Gaussian Blur,
│                         │    CLAHE, Resizing
└─────────────────────────┘
     │
     ▼
┌─────────────────────────┐
│   Data Augmentation     │  → 3× Dataset Expansion
│                         │    (flips, rotations, color jitter)
└─────────────────────────┘
     │
     ▼
┌─────────────────────────┐
│  Train / Val / Test     │  → Stratified Split
│       Split             │
└─────────────────────────┘
     │
     ▼
┌─────────────────────────┐
│  YOLOv8 Classification  │  → Transfer Learning on
│       Training          │    yolov8n-cls.pt
└─────────────────────────┘
     │
     ▼
┌─────────────────────────┐
│  Evaluation & Metrics   │  → Accuracy, Precision, Recall,
│                         │    F1-Score, Confusion Matrix
└─────────────────────────┘
```

---

## 📁 Project Structure

```
liver-tumor-yolo/
│
├── notebooks/
│   ├── Image_Preprocessing_for_Liver_Tumor_Classification.ipynb   # Step 1: Preprocessing & Augmentation
│   ├── Liver_Tumor_Classification_yolov8.ipynb                    # Step 2: YOLOv8 Training & Evaluation
│   └── Liver_Tumor_Classification_Experiments.ipynb               # Step 3: Experiments & Extended Analysis
│
├── docs/
│   └── Project_Report.pdf                                         # Full project report
│
├── assets/                                                         # Images for README (results, diagrams)
│
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install ultralytics opencv-python torch torchvision scikit-learn matplotlib seaborn Pillow
```

### Running on Google Colab (Recommended)

1. **Open the notebooks** in Google Colab (click the badge links above each notebook)
2. **Mount Google Drive** and place your dataset at:
   ```
   MyDrive/Liver Tumor Classification/
   ├── Cholangiocarcinoma/
   ├── HCC/
   └── Normal Liver/
   ```
3. **Run notebooks in order:**
   - `Image_Preprocessing...` → generates preprocessed + augmented images
   - `Liver_Tumor_Classification_yolov8...` → trains and evaluates the model

### Dataset Structure (after preprocessing)

```
Liver Tumor Classification/
├── train/
│   ├── Cholangiocarcinoma/
│   ├── HCC/
│   └── Normal Liver/
├── val/
│   ├── ...
└── test/
    ├── ...
```

> ⚠️ **Dataset Note:** The CT scan dataset is not included in this repository due to size/privacy constraints. Please use your own dataset or publicly available liver CT datasets (e.g., [LiTS](https://competitions.codalab.org/competitions/17094), [CHAOS](https://chaos.grand-challenge.org/)).

---

## 🧪 Preprocessing Steps

The preprocessing pipeline (see `Image_Preprocessing_for_Liver_Tumor_Classification.ipynb`) applies:

1. **Image Normalization** — Scales pixel intensities to [0, 255]
2. **Gaussian Blur** — Removes high-frequency noise
3. **Augmentation (3× expansion)** — Random flips, rotations, and color jitter via `torchvision.transforms`
4. **Train/Val/Test Split** — Stratified split using `sklearn.model_selection`

---

## 🏋️ Model Training

```bash
yolo task=classify mode=train \
     model=yolov8n-cls.pt \
     data="path/to/Liver Tumor Classification" \
     epochs=50 \
     imgsz=128 \
     project=logs
```

**Monitoring training with TensorBoard:**

```python
%load_ext tensorboard
%tensorboard --logdir logs/train
```

---

## 📊 Results

After training for 50 epochs with YOLOv8n-cls:

| Metric | Score |
|---|---|
| Accuracy | *See notebook output* |
| Precision | *See notebook output* |
| Recall | *See notebook output* |
| F1-Score | *See notebook output* |

> 📌 **Note:** Add your actual results here after training. Confusion matrix and classification report are auto-generated in `Liver_Tumor_Classification_yolov8.ipynb`.

---

## 🔮 Future Improvements

- [ ] Upgrade to **YOLOv8m/YOLOv8l** for higher accuracy
- [ ] Integrate **Grad-CAM** for visual explainability (highlight tumor regions)
- [ ] Add **DICOM image support** for real clinical CT scans
- [ ] Build a **Streamlit/Gradio web app** for real-time inference
- [ ] Experiment with **ensemble models** (YOLOv8 + EfficientNet)
- [ ] Add **cross-validation** for robust performance estimation
- [ ] Deploy as a **REST API** using FastAPI + Docker

---

## 📖 Report

The full project report is available in [\docs\report liver tumor classification.pdf]
---

## 🧰 Tech Stack

| Tool | Purpose |
|---|---|
| [Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics) | Model backbone |
| [OpenCV](https://opencv.org/) | Image preprocessing |
| [PyTorch + torchvision](https://pytorch.org/) | Augmentation pipeline |
| [scikit-learn](https://scikit-learn.org/) | Metrics & data splitting |
| [Matplotlib + Seaborn](https://seaborn.pydata.org/) | Visualization |
| [Google Colab](https://colab.research.google.com/) | GPU training environment |

---

## 🤝 Contributing

Contributions are welcome! If you'd like to improve the model, add new features, or fix issues:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add some feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---


---

## 🙏 Acknowledgements

- [Ultralytics](https://ultralytics.com/) for the YOLOv8 framework
- Medical imaging community for publicly available liver CT datasets
- Google Colab for free GPU access

---

<div align="center">
Made with ❤️ for advancing medical AI
</div>
