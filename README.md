# AI POWERED PLASTIC WASTE SEGREGATION From Hotspot Detection to Material Classification

This repository contains a real-time waste detection and classification prototype built with YOLOv8. It supports live inference using your phone camera via Camo Studio and includes tools for dataset preparation, training, evaluation, and deployment.

## 📂 Repository Structure

```
Waste_Detection/
├── YOLO-Waste-Detection-1/             # Core detection project
│   ├── train_yolo.py                   # YOLOv8 training script
│   ├── accuracy_check.py               # Per-class AP@0.5 metrics script
│   ├── real_time_camo_fixed.py         # Real-time demo using Camo Studio
│   ├── data.yaml                       # Dataset configuration
│   ├── train/                          # Training images & labels
│   │   ├── images/
│   │   └── labels/
│   ├── valid/                          # Validation images & labels
│   │   ├── images/
│   │   └── labels/
│   ├── test/                           # Test images & labels
│   │   ├── images/
│   │   └── labels/
│   └── baseline_train4/                # Training outputs
│       ├── weights/best.pt             # Final YOLOv8 weights
│       └── results.csv                 # Validation results table
└── README.md                           # This file
```

## 📝 Overview

1. **Data Preparation**: Organize YOLO-format dataset with `images/` and `labels/` for train, valid, test and configure `data.yaml`.
2. **Environment Setup**: Install CUDA-enabled PyTorch, Ultralytics YOLOv8, and OpenCV.
3. **Training**: Train a YOLOv8n model for 15 epochs on GTX 1650 GPU using `train_yolo.py`.
4. **Evaluation**: Compute per-class AP\@0.5 with `accuracy_check.py` to identify strong and weak categories.
5. **Real-Time Inference**: Stream live detection from phone camera via Camo Studio using `real_time_camo_fixed.py`.

## ⚙️ Setup & Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/your-username/waste-detection.git
   cd waste-detection/YOLO-Waste-Detection-1
   ```
2. **Create environment** (conda or venv)

   ```bash
   conda create -n waste_env python=3.10 -y
   conda activate waste_env
   ```
3. **Install dependencies**

   ```bash
   pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118
   pip install ultralytics opencv-python sort-tracker
   ```
4. **Verify GPU**

   ```python
   python - << EOF
   import torch
   print('CUDA available:', torch.cuda.is_available())
   print('GPU:', torch.cuda.get_device_name(0) if torch.cuda.is_available() else None)
   EOF
   ```

## 📊 Data Configuration

Edit `data.yaml` to point to your dataset:

```yaml
train: train/images
val:   valid/images
test:  test/images
nc: 43
names: ['Aerosols', 'Aluminum can', ... , 'Ramen Cup']
```

## 🚀 Training YOLOv8

Run the training script:

```bash
python train_yolo.py
```

* **Epochs**: 15
* **Batch size**: 8
* **imgsz**: 640
* **device**: GPU (0)
* Outputs saved under `baseline_train4/weights/best.pt` and logs in `baseline_train4/`.

## 📈 Evaluation & Metrics

Compute per-class AP\@0.5:

```bash
python accuracy_check.py
```

* Generates a sorted list of classes by AP\@0.5 to identify weak categories.

## 🎥 Real-Time Demo

Ensure Camo Studio is running (phone connected). Update camera backend and rotate settings in `real_time_camo_fixed.py` if needed.

```bash
python real_time_camo_fixed.py
```

* **conf**: 0.3 (adjustable)
* **backend**: MSMF (1400)

## 📦 Export to ONNX (Optional)

```bash
yolo export model=baseline_train4/weights/best.pt format=onnx
```

## 📁 Requirements

```bash
pip freeze > requirements.txt
```

## 🛠 Next Steps

* Augment and retrain weak classes (AP < 0.3)
* Integrate MobileNetV3 classification for multi-modal sensing
* Deploy on edge devices using TensorRT or ONNX Runtime

## 📜 License

This project is released under the [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) license.

---

*Created with ❤️ during a 7-day sprint.*
