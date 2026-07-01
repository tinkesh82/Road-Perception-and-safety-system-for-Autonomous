🚗 Road Perception and Safety System for Autonomous Vehicles


📌 Project Overview

The Road Perception and Safety System is an end-to-end, multi-task visual
perception pipeline designed to assist autonomous vehicles and Advanced Driver
Assistance Systems (ADAS). Tailored specifically for the diverse and challenging
conditions of Indian roads, this system processes monocular camera video in
real-time to simultaneously handle lane tracking, traffic sign recognition,
pothole detection, and speed breaker detection.

✨ Key Features

  - 🛣️ Lane Detection & Steering Angle Estimation: Utilizes a VGG16-UNet
    segmentation model combined with classical geometric post-processing
    (Perspective Transform, Sliding-Window Polyfit) to track lane boundaries.
  - 🌙 Night-Time Enhancement: Implements HLS color-space processing and CLAHE
    (Contrast Limited Adaptive Histogram Equalization) to improve lane
    visibility under low-light and night-time conditions.
  - 🚸 Indian Traffic Sign Recognition: A two-stage pipeline using YOLOv5s for
    real-time sign localization and a fine-tuned EfficientNet-B0 for
    classifying 59 distinct Indian traffic sign categories.
  - ⚠️ Road Hazard Detection: Real-time detection of potholes (YOLO11s) and
    speed breakers (YOLO11n) providing advance safety alerts via a Heads-Up
    Display (HUD).
  - ⚡ Integrated Real-Time Pipeline: A multi-threaded architecture that runs all
    perception modules concurrently, fusing the outputs into a single,
    comprehensive HUD overlay on the video feed.

🏗️ System Architecture

The pipeline is divided into independent modules that run in parallel threads:

1.  Frame Acquisition & Preprocessing: Resizing, normalization, and CLAHE.
2.  LaneDetector: VGG16-UNet inference → Perspective Transform → Polynomial
    Fitting → Steering Angle.
3.  TrafficSignDetector: YOLOv5s (Phase 1) to find bounding boxes.
4.  TrafficSignClassifier: EfficientNet-B0 (Phase 2) to label cropped signs.
5.  PotholeDetector: YOLO11s for surface hazard detection.
6.  SpeedBreakerDetector: YOLO11n for speed bump detection.
7.  Decision & Logger Module: Generates rule-based driving commands (SLOW_DOWN,
    AVOID, FOLLOW) and renders the unified HUD.

🛠️ Technology Stack

  - Languages: Python 3.11.x
  - Deep Learning: TensorFlow/Keras 2.x (VGG16-UNet), PyTorch 2.x + Torchvision
    (EfficientNet-B0)
  - Object Detection: Ultralytics YOLO (YOLOv5s, YOLO11s, YOLO11n)
  - Computer Vision: OpenCV 4.x, NumPy, Pillow
  - Data Management: Pandas, Matplotlib, Scikit-Learn

📊 Datasets & Model Performance

The models were trained on over 34,000 annotated images across five distinct
datasets:

| Module                  | Model Architecture | Dataset                              | Key Metric (Performance)            |
| :---------------------- | :----------------- | :----------------------------------- | :---------------------------------- |
| **Lane Detection**      | VGG16-UNet         | TuSimple Dataset (6.4k images)       | **96.8%** Validation Pixel Accuracy |
| **Sign Detection**      | YOLOv5s            | Kaggle Traffic Sign Det. (9k images) | **0.979** mAP@50                    |
| **Sign Classification** | EfficientNet-B0    | Indian Traffic Sign (59 classes)     | **\~96.0%** Validation Accuracy     |
| **Pothole Detection**   | YOLO11s            | YOLOv11 Optimized (3.9k images)      | **0.862** mAP@50                    |
| **Speed Breakers**      | YOLO11n            | Roboflow Dataset (3.2k images)       | **0.962** mAP@50                    |

🚀 Getting Started

Prerequisites

  - Python 3.11+
  - CUDA Toolkit & cuDNN (for GPU acceleration)

Installation

1.  Clone the repository:

    git clone https://github.com/yourusername/road-perception-system.git
    cd road-perception-system

2.  Install the required dependencies:

    pip install -r requirements.txt

    (Ensure you install the GPU versions of TensorFlow and PyTorch for real-time
    inference).

3.  Download the pre-trained weights (best_model.keras, yolov5s.pt,
    best_model.pth, pothole.pt, breaker.pt) and place them in the weights/
    directory.

Usage

To run the fully integrated multi-threaded pipeline on a test video:

python fully_integrated_runner.py --input data/test_video.mp4 --output results/output.mp4

🔮 Future Enhancements

  - Sensor Fusion: Integration of stereo cameras or LiDAR for accurate absolute
    depth estimation and Time-to-Collision (TTC) metrics.
  - Edge Deployment: Model pruning and quantization (TensorRT) for deployment on
    embedded hardware like the NVIDIA Jetson.
  - Simulation Integration: Connecting the perception outputs to ROS2 and CARLA
    for closed-loop autonomous navigation testing.

👤 Author

Tinkesh Kumar (CSB22082)
