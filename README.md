# ClearSkin AI 🔬📱

**ClearSkin AI** is a cross-platform mobile application that uses deep learning to help users get a preliminary assessment of common skin conditions from a photo. Built as a Final Year Project, it combines an **on-device TFLite model** for offline-first inference with a **FastAPI backend** for higher-accuracy cloud-based predictions, giving users fast, accessible insight into skin lesions while always encouraging professional medical consultation.

> ⚠️ **Disclaimer:** ClearSkin AI is an educational/academic project and is **not** a diagnostic medical device. All results are AI-generated and must be confirmed by a licensed dermatologist.

---

## ✨ Features

- 📸 **Capture or upload** a photo of a skin lesion directly from the camera or gallery
- 🧠 **Dual inference pipeline** — tries the FastAPI backend first, automatically falls back to an on-device TFLite model if the API is unreachable
- 🚫 **Skin-image validation** — the backend rejects non-skin images (e.g. random objects, screenshots) instead of forcing a meaningless prediction, using a skin-pixel-ratio check before classification
- 🩺 **7-class skin condition classifier** trained on the HAM10000 dataset:
  - Actinic Keratoses
  - Basal Cell Carcinoma
  - Benign Keratosis
  - Dermatofibroma
  - Melanoma
  - Melanocytic Nevi
  - Vascular Lesions
- 📊 **Confidence meter & full probability breakdown** across all classes
- 📝 **Rich condition details** — description, appearance, causes, symptoms, treatments, prevention, prognosis, and when to see a doctor
- 📄 **PDF report generation** — save and share scan results
- 🕘 **Scan history** stored locally on-device
- 🌐 **Multi-language support**
- ⚡ **Offline-capable** via the bundled TFLite model when no network/API connection is available

---

## 🏗️ Architecture

```
┌─────────────────┐         ┌──────────────────────┐
│   Flutter App    │  HTTP   │   FastAPI Backend     │
│  (Mobile Client)  │ ──────▶ │  (TFLite inference)   │
└─────────────────┘         └──────────────────────┘
        │
        │ fallback if API unreachable
        ▼
┌─────────────────┐
│  On-device TFLite │
│      Model         │
└─────────────────┘
```

**Why this matters:** the same TFLite model architecture powers both the backend and the on-device fallback, so predictions stay consistent whether the user is online or offline — while the backend additionally performs skin-image validation before returning a result.

### Tech Stack

| Layer | Technology |
|---|---|
| Mobile App | Flutter (Dart) |
| Backend API | FastAPI (Python) |
| ML Model | TensorFlow Lite (CNN, trained on HAM10000) |
| Local Storage | SharedPreferences |
| State/Navigation | GoRouter |
| PDF Export | Custom PDF report service |

---

## 📁 Project Structure (high level)

```
lib/
├── core/                  # Shared config, theming, utilities
├── model/                 # ScanResult and other data models
├── services/
│   ├── skin_analysis_service.dart   # API + TFLite inference logic
│   └── pdf_report_service.dart      # PDF generation
├── screens/
│   ├── scan_page.dart                # Capture/upload + analyze
│   └── result_page.dart              # Display scan results
backend/
├── main.py                # FastAPI app & /predict endpoint
├── model/                 # TFLite model assets
└── utils/                 # Skin-pixel-ratio validation, preprocessing
```

---

## 🚀 Getting Started

### Prerequisites
- Flutter SDK (≥3.x)
- Python 3.10+ (for backend)
- Android Studio / Xcode (for device builds)

### Run the Backend

```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8000
```

### Run the Flutter App

```bash
flutter pub get
flutter run
```

> Update the `_apiBaseUrl` constant in `skin_analysis_service.dart` to point to your backend's local network IP or deployed URL.

### Download the APK

A pre-built release APK is available under [Releases](../../releases) for direct installation on Android devices.

---

## 🧪 How It Works

1. User captures or selects a photo of a skin lesion.
2. The image is sent to the FastAPI `/predict` endpoint.
3. The backend computes a **skin pixel ratio** to verify the image actually contains skin before running classification — if the ratio is too low, it returns a `422` response and the app shows a "not a skin image" message instead of a fabricated result.
4. If validation passes, the TFLite model returns a predicted class, confidence score, and full disease detail payload (symptoms, treatment, prevention, etc.).
5. If the API is unreachable, the app falls back to running the same model on-device.
6. Results are displayed with a confidence meter, risk level, and detailed condition information, and can be exported as a PDF.

---

## 👥 Team

This project was developed as a Final Year Project by a team of [N] students:
Muhammad Ameer Hamza — Flutter App Developer(Backend: FastApi)



**Supervisor:** Dr Hasnat Ahmad
**Institution:** University of Engineering and Technology Taxila

---

## 📜 License

This project is submitted as part of an academic Final Year Project and is intended for educational purposes only.

---

## 🙏 Acknowledgements

- [HAM10000 Dataset](https://www.kaggle.com/datasets/surajghuwalewala/ham1000-segmentation-and-classification) for skin lesion training data
- Flutter & FastAPI communities
