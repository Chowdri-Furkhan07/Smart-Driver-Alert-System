# 🚗 Smart Driver Alert System - Real-Time Drowsiness & Yawn Detection

> An AI-powered driver safety system that monitors eyes and mouth in real time via webcam. Using Eye Aspect Ratio (EAR) and Mouth Aspect Ratio (MAR) computed from Dlib's 68-point facial landmarks, it detects drowsiness and yawning simultaneously - triggering dual voice + beep alerts, auto-saving snapshots of every incident, and logging all session data to CSV for post-trip analysis.

![Python](https://img.shields.io/badge/Python-3.x-blue?style=flat&logo=python)
![OpenCV](https://img.shields.io/badge/OpenCV-4.x-green?style=flat&logo=opencv)
![Dlib](https://img.shields.io/badge/Dlib-68--Point%20Landmarks-red?style=flat)
![Jupyter](https://img.shields.io/badge/Notebook-Jupyter-F37626?style=flat&logo=jupyter)
![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?style=flat&logo=windows)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat)

---

## 📌 Overview

Driver fatigue is one of the leading causes of road accidents worldwide. This system delivers a **production-grade, camera-only safety solution** - no wearables, no additional hardware beyond a standard webcam.

The system runs a per-frame computer vision pipeline using **Dlib's 68-point facial landmark predictor**. It extracts eye and mouth landmark coordinates every frame to compute two independent fatigue signals:

- **EAR (Eye Aspect Ratio)** - detects prolonged eye closure (drowsiness)
- **MAR (Mouth Aspect Ratio)** - detects wide mouth opening (yawning)

On detecting either condition, the system fires a **dual alert** (voice via `pyttsx3` + beep via `winsound`), **auto-saves an incident snapshot** to disk, and **logs timestamp, EAR, MAR, and status** to a CSV file for post-session analysis.

---

## ✨ Features

- 👁️ **Drowsiness detection** via Eye Aspect Ratio (EAR) threshold over consecutive frames
- 😮 **Yawn detection** via Mouth Aspect Ratio (MAR) - independent of eye state
- 🔊 **Dual alert system** - simultaneous voice alert (`pyttsx3`) + audible beep (`winsound`) on separate threads
- 📸 **Auto-snapshot capture** - drowsy and yawn incidents saved to `drowsy_snaps/` with timestamps
- 📊 **CSV session logging** - every frame logs `Timestamp`, `EAR`, `MAR`, `Status` to `ear_log.csv`
- 📈 **Excel-compatible log** (`ear_log.xls`) for post-trip fatigue analysis
- 🎯 **68-point facial landmark tracking** via Dlib shape predictor
- 🖼️ **Live HUD overlay** - EAR and MAR values rendered on-screen in real time
- ⚡ **Multi-threaded alerts** - alerts run on daemon threads, zero impact on video FPS
- 💻 **Windows-native** with `winsound` for zero-latency system beep

---

## 🧠 How It Works

### Full Pipeline

```
Webcam captures live video frame
          ↓
OpenCV converts frame to grayscale
          ↓
Dlib HOG face detector locates face bounding box
          ↓
Dlib 68-point landmark predictor maps facial geometry
          ↓
          ├── Eye landmarks extracted (36-41 left, 42-47 right)
          │         ↓
          │   EAR computed & averaged across both eyes
          │         ↓
          │   EAR < 0.21 for 20+ consecutive frames?
          │         ↓ YES
          │   → Status = "DROWSY"
          │   → Save snapshot to drowsy_snaps/
          │   → Fire voice alert thread (pyttsx3)
          │   → Fire beep alert thread (winsound)
          │
          └── Mouth landmarks extracted (48-67)
                    ↓
              MAR computed
                    ↓
              MAR > 0.75?
                    ↓ YES
              → Status = "YAWNING"
              → Save snapshot to drowsy_snaps/
              → Fire voice + beep alert
          ↓
EAR, MAR, Status logged to ear_log.csv with timestamp
          ↓
Live HUD rendered: EAR value, MAR value, alert text
```

---

### EAR Formula (Eye Aspect Ratio)

```
        ||p2 - p6|| + ||p3 - p5||
EAR =  ─────────────────────────
              2 · ||p1 - p4||
```

| EAR Value | Eye State |
|---|---|
| ≥ 0.30 | Eyes open — Alert |
| 0.21 – 0.29 | Partially closed — Transitioning |
| < 0.21 for 20+ frames | **DROWSY → ALERT TRIGGERED** |

---

### MAR Formula (Mouth Aspect Ratio)

```
        ||p14 - p20|| + ||p15 - p19||
MAR =  ──────────────────────────────
               2 · ||p13 - p17||
```

| MAR Value | Mouth State |
|---|---|
| < 0.65 | Mouth closed — Normal |
| 0.65 – 0.75 | Mouth slightly open |
| > 0.75 | **YAWNING → ALERT TRIGGERED** |

---

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| Video Capture & Processing | OpenCV (`cv2`) |
| Face Detection | Dlib - HOG + SVM Frontal Face Detector |
| Facial Landmark Estimation | Dlib - 68-Point Shape Predictor |
| EAR / MAR Computation | NumPy + SciPy (`spatial.distance.euclidean`) |
| Voice Alert | pyttsx3 - Text-to-Speech Engine |
| Beep Alert | winsound - Windows System Audio |
| Multi-threading | Python `threading` module (daemon threads) |
| Session Logging | Python `csv` module → `ear_log.csv` |
| Incident Snapshots | OpenCV `cv2.imwrite()` → `drowsy_snaps/` |
| Development Environment | Jupyter Notebook / VS Code |
| Language | Python 3.x |

---

## 📁 Project Structure

```
Smart-Driver-Alert-System/
│
├── Drowsiness Detection.ipynb              # Main notebook implementation
├── shape_predictor_68_face_landmarks.dat   # Dlib pre-trained model (download separately)
├── requirements.txt                        # Python dependencies
├── ear_log.csv                             # Auto-generated session log (Timestamp, EAR, MAR, Status)
├── ear_log.xls                             # Excel-compatible session log
├── drowsy_snaps/                           # Auto-saved incident snapshots
│   ├── drowsy_<timestamp>.jpg
│   └── yawn_<timestamp>.jpg
└── README.md
```

---

## ⚙️ Setup & Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Chowdri-Furkhan07/Smart-Driver-Alert-System.git
cd Smart-Driver-Alert-System
```

### 2. Create a Virtual Environment (Recommended)

```bash
python -m venv .venv
.venv\Scripts\activate        # Windows
source .venv/bin/activate     # macOS / Linux
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

Or manually:

```bash
pip install opencv-python dlib numpy scipy pyttsx3 imutils
```

> **Windows users:** `dlib` requires CMake and Visual Studio Build Tools.
> Run `pip install cmake` first, or download a pre-built `.whl` from [Dlib GitHub Releases](https://github.com/davisking/dlib).

### 4. Download the Dlib Landmark Model

Download the pre-trained 68-point shape predictor (~100MB):

🔗 [shape_predictor_68_face_landmarks.dat.bz2](http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2)

Extract the `.dat` file and place it in the **project root directory**.

### 5. Run the System

```bash
jupyter notebook "Drowsiness Detection.ipynb"
```

Run all cells — the webcam activates and monitoring begins.

**To exit:** Press `q` in the OpenCV window.

---

## 📦 Requirements

```
opencv-python
dlib
numpy
scipy
pyttsx3
imutils
```

> `winsound` is built into Python on Windows - no installation needed.

---

## 📊 Session Logging — `ear_log.csv`

Every processed frame writes a row to `ear_log.csv`:

| Timestamp | EAR | MAR | Status |
|---|---|---|---|
| 2025-06-01 08:42:11.023 | 0.31 | 0.42 | Normal |
| 2025-06-01 08:43:05.441 | 0.18 | 0.41 | DROWSY |
| 2025-06-01 08:44:22.119 | 0.29 | 0.81 | YAWNING |

This log enables **post-trip fatigue analysis** - load into Excel, Pandas, or Power BI to visualize drowsiness patterns over time.

---

## 🔑 Key Concepts Demonstrated

| Concept | Implementation |
|---|---|
| Facial Landmark Detection | Dlib 68-point predictor on live grayscale frames |
| Geometric Feature Engineering | EAR (eyes) + MAR (mouth) - dual independent fatigue signals |
| Threshold-Based Classification | Consecutive-frame EAR check + single-frame MAR check |
| Multi-threaded Alerting | `threading.Thread(daemon=True)` for voice + beep - zero FPS impact |
| Incident Logging & Snapshotting | `csv` writer + `cv2.imwrite()` for every drowsy/yawn event |
| Safety-Critical System Design | Fail-safe reset logic — `alert_on` flag prevents duplicate threads |
| Real-Time HUD | Live EAR/MAR values + status text overlaid on video feed |

---

## 💡 Use Cases

- 🚗 **Commercial fleet monitoring** - long-haul truck and bus driver safety
- 🏭 **Industrial safety** - heavy machinery operators on night shifts
- ✈️ **Aviation** - pilot alertness monitoring in cockpit simulation
- 🏥 **Healthcare** - attention monitoring for ICU staff and shift workers
- 📚 **EdTech** - student engagement and focus tracking
- 🔬 **HCI / Safety Research** - baseline prototype for wearable-free fatigue detection

---

## 🔮 Future Enhancements

- [ ] Head pose estimation - detect forward nodding using 3D landmark projection
- [ ] REST API endpoint - stream alerts to a fleet management dashboard
- [ ] SMS / push notification - integrate Twilio for remote fleet alerts
- [ ] Power BI / Tableau integration - live EAR/MAR dashboard from CSV log
- [ ] Cross-platform audio - replace `winsound` with `playsound` for macOS/Linux
- [ ] Embedded deployment - optimize for Raspberry Pi 4 / NVIDIA Jetson Nano
- [ ] IR camera support - enable detection in low-light / night-driving conditions

---

## 👤 Author

**Chowdri Furkhan**

Artificial Intelligence & Machine Learning

[![GitHub](https://img.shields.io/badge/GitHub-Chowdri--Furkhan07-181717?style=flat&logo=github)](https://github.com/Chowdri-Furkhan07)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-chowdri--furkhan-0A66C2?style=flat&logo=linkedin)](https://linkedin.com/in/chowdri-furkhan)

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
