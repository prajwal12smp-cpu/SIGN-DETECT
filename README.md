# SignDetect AI 🤟
### High-Precision Real-Time Sign Language Recognition & Speech Synthesis System

[![Python 3.11](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![MediaPipe](https://img.shields.io/badge/MediaPipe-0.10.14-0097A7?style=for-the-badge&logo=google&logoColor=white)](https://developers.google.com/mediapipe)
[![OpenCV](https://img.shields.io/badge/OpenCV-4.10+-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)](https://opencv.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.5+-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![Flask](https://img.shields.io/badge/Flask-3.0+-000000?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com/)

**SignDetect AI** is an end-to-end, edge-friendly Computer Vision and Machine Learning platform that bridges the communication divide for the deaf and hard-of-hearing communities. By leveraging **Google MediaPipe Hands (21 3D spatial keypoints)** and an ensemble **Random Forest Classifier**, the system tracks hand gestures in real time, normalizes coordinate geometry, and translates static American Sign Language (ASL) gestures into text and **instant real-time spoken voice (TTS)** via standard webcams without requiring specialized hardware.

---

## 🌟 Key Features

- 🎥 **Real-Time Video Vision Stream (30 FPS)**: Low-latency MJPEG webcam streaming with dynamic HUD scanning brackets, resolution detection, and stream controls.
- 🖐️ **Sub-Pixel Landmark Extraction**: Google MediaPipe Hands tracking 21 key biometric hand landmarks per frame with occlusion tolerance.
- 📐 **Scale & Translation Invariance**: 42-dimensional geometric vector normalization $(x_i - \min(X), y_i - \min(Y))$ allowing accurate detection anywhere in the camera frame.
- 🔊 **Instant Real-Time Vocalization (Auto-Speak TTS)**: Automatically speaks detected words, letters, and numbers the instant they are recognized using the browser's native Web Speech API, with smart anti-spam debouncing.
- 📝 **Live Sentence Builder**: Sequentially accumulates recognized signs into full sentences with **Speak Sentence**, **Copy to Clipboard**, **Space**, and **Clear** controls.
- 📜 **Chronological Event Log**: Real-time timestamped detection history feed.
- 🎨 **Modern Glassmorphic Dark UI**: Ultra-sleek dashboard built with deep space aesthetics, neon accents, responsive mobile navigation, and informative architecture walkthroughs.
- 🔄 **Cross-Port & Live Server Compatibility (CORS)**: Seamlessly operates directly on Flask (`http://127.0.0.1:5000`) or static servers (e.g. VS Code Live Server `port 5500`).

---

## 📖 Supported Gestures (14 Classes)

| Category | Gestures / Signs | Description & Detection Tips |
| :--- | :--- | :--- |
| **Phrases & Words** | `HELLO` 👋 | Raised open palm with spread fingers facing the camera |
| | `YES` 👍 | Closed fist with upright thumb extended |
| | `NO` 🙅 | Index and middle fingertip pinch towards thumb |
| | `THANKS` 🙏 | Flat hand starting near chin moving outward |
| | `SORRY` 🙇 | Closed fist in gentle circular motion on chest |
| | `PLEASE` 🤲 | Flat open palm making circular motion on chest |
| **Alphabet Letters** | `A`, `B`, `C`, `D` 🔤 | Standard ASL static manual alphabet signs |
| **Numerical Digits** | `1`, `2`, `3`, `4` 🔢 | Standard ASL manual counting signs |

---

## 🛠️ Architecture & Detection Pipeline

```mermaid
graph LR
    A[Webcam Input 640x480] --> B[OpenCV Frame Capture BGR to RGB]
    B --> C[MediaPipe Hands 21 3D Landmarks]
    C --> D[Feature Normalization 42D Vector]
    D --> E[Random Forest Classifier Ensemble]
    E --> F[Flask JSON API & MJPEG Stream]
    F --> G[Interactive Web Dashboard & Real-Time TTS]
```

1. **Optical Capture**: OpenCV continuously buffers webcam frames at 30 FPS.
2. **Keypoint Extraction**: MediaPipe identifies 21 $(x, y, z)$ landmarks per hand (wrist, MCP, PIP, DIP, and fingertips).
3. **Spatial Normalization**: Coordinates are converted into a scale-invariant 42-element vector:
   $$\vec{V} = \left[ x_0 - \min(X), y_0 - \min(Y), \dots, x_{20} - \min(X), y_{20} - \min(Y) \right]$$
4. **Machine Learning Classification**: A trained Random Forest model predicts the gesture class in $<1\text{ ms}$.
5. **Speech & UI Dispatch**: The prediction is displayed on the HUD and vocalized in real time.

---

## 📂 Project Structure

```text
SIGN-DETECT-master/
├── data/                       # Collected training image datasets categorized by class ID (0-13)
├── Front-end/                  # Web Frontend application assets
│   ├── index.html              # Modern Landing page & gesture showcase
│   ├── demo.html               # Live Detection Studio (Camera HUD, TTS, Sentence Builder)
│   ├── how_it_works.html       # Visual pipeline & landmark architecture
│   ├── about.html              # WHO mission statistics, author & 4-phase roadmap
│   ├── LoginSignUp.html        # Authentication UI
│   ├── LoginSignUp.css         # Auth styles
│   ├── style.css               # Global glassmorphic dark design system
│   └── Firebase auth.js        # Authentication client logic
├── app.py                      # Flask web server & streaming backend
├── collect_imgs.py             # Script to record raw webcam image samples for new signs
├── create_dataset.py           # Extracts 21 MediaPipe landmarks into data.pickle
├── train_classifier.py         # Trains the Random Forest model and serializes model.p
├── inference_classifier.py     # Standalone OpenCV desktop testing script
├── data.pickle                 # Preprocessed dataset containing 42-dimensional landmark vectors
├── model.p                     # Serialized Random Forest classifier
├── requirements.txt            # Project dependencies
└── README.md                   # Project documentation
```

---

## 🚀 Quick Start Guide

### 1. Prerequisites
- Python 3.9, 3.10, or 3.11 installed.
- A functional webcam.

### 2. Clone the Repository
```bash
git clone https://github.com/Prajwal-Shivashimpar/SIGN-DETECT.git
cd SIGN-DETECT-master
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Run the Application
```bash
python app.py
```
 
## 🏋️ How to Train Custom Gestures

To add new custom hand signs or expand the dataset:

1. **Collect Images for a New Class**:
   ```bash
   python collect_imgs.py
   ```
   *(Press `'Q'` when ready to record 100 frame samples for each sign class)*

2. **Extract Landmark Features**:
   ```bash
   python create_dataset.py
   ```
   *(Processes all images in `./data` with MediaPipe Hands and saves `data.pickle`)*

3. **Train Classifier**:
   ```bash
   python train_classifier.py
   ```
   *(Trains an ensemble Random Forest model and saves `model.p`)*

4. **Verify Locally or in Web App**:
   - Run desktop test: `python inference_classifier.py`
   - Run web app: `python app.py`

---

## 🗺️ Development Roadmap

- [x] **Phase 1: Foundation**: 14 static gesture dataset, MediaPipe landmark extraction, Random Forest training.
- [x] **Phase 2: Live AI Studio & Speech**: 30 FPS MJPEG camera feed, real-time auto-speech synthesis (TTS), sentence builder, and modern glassmorphic dashboard.
- [ ] **Phase 3: Deep Dynamic Sequences**: Temporal gesture recognition (LSTM / Transformer) for motion-based signs and two-handed detection.
- [ ] **Phase 4: Bidirectional AR Translation**: Speech-to-Sign 3D animated avatars and WebXR/smart glasses integration.

---

### 🎥 Demo Video



https://github.com/user-attachments/assets/f87e34a2-19ec-4f13-98df-bb9bdaf4081e





---

## 👨‍💻 Author & Acknowledgements

- **Author**: [Prajwal Shivashimpar](https://github.com/Prajwal-Shivashimpar)
- **Built With**:
  - [Google MediaPipe](https://developers.google.com/mediapipe) for high-fidelity hand landmark tracking
  - [OpenCV Foundation](https://opencv.org/) for real-time computer vision
  - [Scikit-learn](https://scikit-learn.org/) for machine learning algorithms
  - [Flask](https://flask.palletsprojects.com/) for web streaming services


