# 🚗 ParkVision AI

### AI-Powered Smart Parking Space Detection System

ParkVision AI is a computer-vision-based smart parking system that uses **YOLOv8** to detect and classify parking spaces as **empty or occupied** from parking-lot images.

The system processes parking images, identifies individual parking spaces, calculates parking occupancy, and generates useful insights such as parking availability and congestion level.

---

## 📌 Project Overview

Finding available parking spaces in large parking areas can be time-consuming and inefficient. Drivers often need to manually search for an empty space, while parking operators may not have an automated way to monitor occupancy.

**ParkVision AI** addresses this problem by using artificial intelligence and computer vision to analyse parking-lot images and identify:

* 🟢 Empty parking spaces
* 🔴 Occupied parking spaces
* 📊 Total parking spaces
* 📈 Parking occupancy percentage
* 🚦 Parking congestion level
* 💡 Automated parking recommendations

The project combines **YOLOv8, OpenCV, Python and Streamlit** to create an interactive smart-parking application.

---

# 🎯 Problem Statement

Traditional parking management often relies on manual monitoring or basic parking sensors. These approaches can become difficult to scale for large parking areas.

ParkVision AI aims to provide an image-based solution that can automatically analyse a parking area and determine the availability of parking spaces.

### Main Problem

> How can computer vision and machine learning be used to automatically detect available and occupied parking spaces and provide useful parking-occupancy insights?

---

# 🎯 Project Objectives

The main objectives of ParkVision AI are to:

1. Detect parking spaces from parking-lot images.
2. Classify each detected space as empty or occupied.
3. Calculate total, occupied and available spaces.
4. Calculate the overall parking occupancy percentage.
5. Identify the congestion level of a parking area.
6. Provide visual detection results using bounding boxes.
7. Present the results through an interactive Streamlit interface.
8. Evaluate the performance of the trained YOLOv8 model.
9. Provide useful recommendations based on parking availability.

---

# 🧠 Machine Learning Approach

ParkVision AI uses **YOLOv8 object detection** rather than traditional image classification.

The model detects individual parking spaces directly within a full parking-lot image.

### Classes

The final dataset configuration uses two classes:

| Class ID | Class            | Meaning                    |
| -------- | ---------------- | -------------------------- |
| 0        | `space-empty`    | 🟢 Available parking space |
| 1        | `space-occupied` | 🔴 Occupied parking space  |

The original dataset annotations contained a parent `spaces` category. During preprocessing, this parent category was excluded and the two relevant parking-space classes were mapped to the YOLO format.

---

# 📊 Dataset

The project uses the **PKLot parking dataset**, obtained in YOLO-compatible form and prepared for object detection.

The dataset contains images of parking areas with annotated parking spaces.

### Dataset Structure

```text
dataset/
│
├── train/
│   ├── images/
│   └── labels/
│
├── valid/
│   ├── images/
│   └── labels/
│
├── test/
│   ├── images/
│   └── labels/
│
└── data.yaml
```

The dataset was divided into training, validation and testing subsets.

### Dataset Preprocessing

The preprocessing pipeline included:

* Conversion from COCO annotation format to YOLO format
* Bounding-box normalization
* Image and label organization
* Class mapping
* Dataset validation
* Image resizing during YOLO training
* Data augmentation during training

The YOLO training pipeline also applies augmentation techniques such as:

* Horizontal flipping
* HSV/brightness-related augmentation
* Rotation
* Other YOLO-supported augmentation techniques

---

# 🔄 Data Conversion

The original annotations were provided using the COCO annotation structure.

The conversion process mapped the relevant categories as follows:

```text
COCO:
1 → space-empty
2 → space-occupied

YOLO:
0 → space-empty
1 → space-occupied
```

The parent `spaces` category was not used as a detection class.

This ensured that the final YOLO model was trained using exactly two meaningful parking-space classes.

---

# 🤖 Model

### Model Used

**YOLOv8n**

YOLOv8 Nano was selected because it provides a relatively lightweight object-detection model suitable for an educational computer-vision project and interactive application.

The model was initialized using pretrained YOLOv8 weights and fine-tuned for the parking-space detection task.

### Main Training Configuration

```text
Model: YOLOv8n
Task: Object Detection
Image Size: 640 × 640
Batch Size: 16
Training Epochs: 20–30
Optimizer: Automatically selected by Ultralytics
Classes: 2
```

Training was performed using the Ultralytics YOLO framework.

---

# 📈 Model Evaluation

The model was evaluated using common object-detection metrics including:

* Precision
* Recall
* mAP@50
* mAP@50–95
* Confusion Matrix

During the initial dataset/model sanity-check run, the model successfully recognized the two intended classes:

```text
space-empty
space-occupied
```

The one-epoch validation run produced:

| Metric    | Result |
| --------- | -----: |
| Precision |  0.950 |
| Recall    |  0.943 |
| mAP@50    |  0.966 |
| mAP@50–95 |  0.705 |

### Per-Class Sanity-Check Results

| Class          | Precision | Recall | mAP@50 | mAP@50–95 |
| -------------- | --------: | -----: | -----: | --------: |
| space-empty    |     0.959 |  0.906 |  0.958 |     0.714 |
| space-occupied |     0.942 |  0.980 |  0.974 |     0.696 |

**Note:** These values were obtained during the initial one-epoch sanity-check training run and should not be interpreted as the final fully trained model's performance.

---

# 🖥️ Streamlit Application

The project includes an interactive **Streamlit web application** designed to present the parking detection system in an accessible interface.

## Main Application Sections

### 🏠 Dashboard

The dashboard provides an overview of the system and displays:

* Total parking spaces
* Empty spaces
* Occupied spaces
* Occupancy percentage
* Parking health/availability
* AI-generated insights
* Parking visualisation

---

### 📷 Detection

The Detection section allows a user to upload a parking-lot image.

The application then:

1. Processes the uploaded image.
2. Runs YOLOv8 object detection.
3. Identifies parking spaces.
4. Classifies them as empty or occupied.
5. Draws bounding boxes.
6. Calculates parking statistics.
7. Displays recommendations.

The visual convention is:

```text
🟢 Green → Empty
🔴 Red   → Occupied
```

---

### 🎥 Video Detection

The Video Detection section allows parking footage to be analysed using the computer-vision pipeline.

The purpose is to demonstrate how the detection approach can be extended from individual images to video-based parking analysis.

---

### 📊 Analytics

The Analytics section presents parking statistics and historical detection information.

It can display:

* Occupied spaces
* Empty spaces
* Total spaces
* Occupancy percentage
* Detection history
* Parking trends

---

### 📈 Model Performance

The Model Performance section provides visual information about the machine-learning model, including evaluation results and a confusion matrix.

This allows the model's classification performance to be examined rather than treating the prediction system as a black box.

---

### ℹ️ About

The About section documents the project, technologies and major features.

Technologies used include:

* Python
* YOLOv8
* OpenCV
* Streamlit
* Plotly
* PKLot Dataset

---

# 🧮 Parking Logic

After detection, the application calculates:

### Total Spaces

```text
Total = Empty + Occupied
```

### Occupancy Percentage

```text
Occupancy % = (Occupied / Total) × 100
```

### Availability Percentage

```text
Availability % = (Empty / Total) × 100
```

---

# 🚦 Congestion Classification

ParkVision AI categorizes parking conditions according to occupancy:

| Occupancy | Congestion  |
| --------: | ----------- |
|     < 40% | 🟢 Low      |
|    40–75% | 🟠 Moderate |
|     > 75% | 🔴 High     |

The application uses these values to generate simple parking recommendations.

For example:

```text
Low occupancy
→ Parking availability is high.

Moderate occupancy
→ Parking availability is becoming limited.

High occupancy
→ Parking area is highly congested.
```

---

# 🛠️ Technologies Used

| Technology     | Purpose                              |
| -------------- | ------------------------------------ |
| Python         | Core programming language            |
| YOLOv8         | Object detection                     |
| Ultralytics    | YOLO training and inference          |
| OpenCV         | Image/video processing               |
| Streamlit      | Web application                      |
| Plotly         | Interactive visualisations           |
| Pandas         | Data processing and analytics        |
| NumPy          | Numerical operations                 |
| Pillow         | Image handling                       |
| Scikit-learn   | Supporting machine-learning analysis |
| Albumentations | Image augmentation                   |

---

# 📁 Project Structure

```text
ParkVision-AI/
│
├── app.py
├── requirements.txt
├── README.md
├── style.css
│
├── train_model.py
├── convert_dataset.py
├── processing.py
├── dataset_manager.py
│
├── dataset/
│   ├── data.yaml
│   ├── train/
│   ├── valid/
│   └── test/
│
├── utils/
│   ├── detector.py
│   ├── analytics.py
│   ├── video_detector.py
│   ├── heatmap.py
│   └── check_classes.py
│
├── assets/
│   └── hero.jpg
│
└── runs/
    └── detect/
```

---

# 🧪 Testing

The application was tested using parking-lot images containing different numbers of occupied and empty spaces.

Testing focused on:

* Parking-space detection
* Empty/occupied classification
* Bounding-box visualisation
* Occupancy calculations
* Different parking layouts
* Different viewing angles
* Different lighting conditions
* Application functionality
* Model-performance visualisation

### Potential Failure Cases

Computer-vision-based parking detection may experience errors in situations such as:

* Heavy shadows
* Poor lighting
* Significant image blur
* Severe occlusion
* Unusual camera angles
* Vehicles partially blocking parking spaces
* Parking layouts significantly different from the training data

These limitations are important considerations when applying the system to real-world environments.

---

# 📱 Application Evidence and Screenshot Documentation

The complete Streamlit application contains multiple interconnected components, including image detection, video processing, analytics, model evaluation, visualisations and supporting interface elements.

Because the complete project interface and its generated visual outputs are relatively large to represent in a single repository preview, **screenshots of the working application have been included as the primary visual evidence of the application's implementation and features for the project submission**.

The screenshots demonstrate the application's:

* Dashboard
* Parking detection interface
* Detection results
* Video detection interface
* Analytics page
* Model performance page
* Confusion matrix
* About page
* Parking visualisations
* Occupancy statistics
* AI-generated insights

The screenshots are provided as **proof of the application's implemented interface and functionality** for the academic submission.

> **Note:** The screenshots are documentation/evidence of the application interface and are not intended to replace the underlying source code. The project source files and machine-learning workflow are included in this repository.

---

# 💡 Key Features

### Computer Vision

* YOLOv8 object detection
* Parking-space localization
* Empty/occupied classification
* Bounding-box visualisation

### Smart Analytics

* Occupancy calculation
* Availability calculation
* Congestion classification
* Parking recommendations
* Detection history

### Interactive Application

* Streamlit interface
* Image upload
* Video detection
* Interactive charts
* Model-performance visualisation
* Downloadable annotated results

---

# 🌱 Future Improvements

The project could be further developed by adding:

1. Real-time CCTV camera integration.
2. Automatic parking-space tracking across video frames.
3. License-plate recognition where legally and ethically appropriate.
4. Multi-camera parking management.
5. Cloud-based parking monitoring.
6. Mobile application integration.
7. Real-time notifications when parking availability changes.
8. Improved performance under night-time, rainy and heavily occluded conditions.
9. More diverse training data from real-world parking environments.
10. Optimisation for edge devices and low-power hardware.

---

# 📚 Academic and Technical References

### YOLO / Ultralytics

Ultralytics YOLO documentation and model framework were used for object detection, model training and evaluation.

**Ultralytics:**
https://docs.ultralytics.com/

### PKLot Dataset

The PKLot dataset was used as the primary source of parking-lot imagery and annotations.

**PKLot Dataset:**
https://web.inf.ufpr.br/vri/databases/parking-lot-database/

### OpenCV

OpenCV was used for image and video processing.

**OpenCV Documentation:**
https://docs.opencv.org/

### Streamlit

Streamlit was used to create the interactive web application.

**Streamlit Documentation:**
https://docs.streamlit.io/

---

# 👨‍💻 Project Author

**Dwij Vala**

AI / IBCP Year 2

ParkVision AI
Smart Parking Space Detection System

---

# 📌 Project Summary

ParkVision AI demonstrates how **artificial intelligence and computer vision can be applied to smart-city parking management**.

The project combines:

```text
Parking Dataset
       ↓
Data Preprocessing
       ↓
YOLOv8 Object Detection
       ↓
Empty / Occupied Classification
       ↓
Parking Statistics
       ↓
Congestion Analysis
       ↓
AI Insights
       ↓
Streamlit Application
```

The project demonstrates the complete machine-learning workflow from **data preparation and model development to application-level visualisation and analysis**.

---

## ⭐ ParkVision AI

> **See the space. Understand the parking. Park smarter.**
