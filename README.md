# ML26-07 QR Code Recognition with Arduino Portenta H7

[![GitHub Actions Test Cases](https://github.com/Wings-hub/ML26-07-QR-Code-Recognition-with-an-Arduino-Portenta-H7/actions/workflows/GitHubActionsTestsCases.yml/badge.svg)](https://github.com/Wings-hub/ML26-07-QR-Code-Recognition-with-an-Arduino-Portenta-H7/actions/workflows/GitHubActionsTestsCases.yml)

[![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)](https://supabase.com)
[![Arduino](https://img.shields.io/badge/Arduino-009486?style=for-the-badge&logo=arduino&logoColor=white)](https://arduino.cc)
[![Supabase DB](https://img.shields.io/badge/Supabase%20DB-3ECF8E?style=for-the-badge&logo=postgresql&logoColor=white)](https://supabase.com/database)
[![TinyML](https://img.shields.io/badge/TinyML-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white)](https://www.tinyml.org)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![MicroPython](https://img.shields.io/badge/MicroPython-FFBD33?style=for-the-badge&logo=micropython&logoColor=black)](https://micropython.org)

<table>
<tr>
<td width="60%">

**Authors:** 
- Hemanth Jadiswami Prabhakaran (7026000)
- Nanda Srikanth Saroj Kumar Nanda (7026002)
- Subash Karapparambu Suresh Kumar (7026794)

**Supervisor:** Prof. Dr. Elmar Wings  
**University:** Hochschule Emden/Leer (University of Applied Sciences Emden/Leer)  
**Program:** Master's Business Analytics

</td>
<td width="40%">
<img src="./Manual/General/ProductSketch.png" alt="Project Image" width="100%"/>
</td>
</tr>
</table>
---

## 📋 Project Overview

This project implements an intelligent QR code recognition system using the **Arduino Portenta H7** microcontroller with CNN-based pre-filtering. The system uses a lightweight binary CNN classifier to pre-filter frames, only attempting full QR decoding when a potential QR code is detected with high confidence (>90%).

### Key Features

- **CNN Pre-filtering**: Binary classifier quickly rejects frames without QR codes
- **QR Support**: Versions 1-40 with all ECC levels (L, M, Q, H)
- **WiFi & Cloud**: Automatic Supabase database uploads
- **LED Feedback**: Real-time visual status indicators
- **Modular Design**: Clean architecture with comprehensive testing

---
## 📚 Documentation

### Quick Access Documents

| Document | PDF | LaTeX Source |
|----------|-----|--------------|
| **Technical Report** | [📄 QRcodeRecognitionReport.pdf](./report/System/EdgeComputer/QRcodeRecognitionReport.pdf) | [QRcodeRecognitionReport.tex](./report/System/EdgeComputer/QRcodeRecognitionReport.tex) |
| **User Manual** | [📄 QRcodeRecognitionManual.pdf](./Manual/QRcodeRecognitionManual.pdf) | [QRcodeRecognitionManual.tex](./Manual/QRcodeRecognitionManual.tex) |
| **Assembly Guide** | [📄 QRcodeRecognitionAssembly.pdf](./Assembly/QRcodeRecognitionAssembly.pdf) | [QRcodeRecognitionAssembly.tex](./Assembly/QRcodeRecognitionAssembly.tex) |
| **Project Poster** | [📄 QRcodeRecognitionPoster.pdf](./Poster/QRcodeRecognitionPoster.pdf) | [QRcodeRecognitionPoster.tex](./Poster/QRcodeRecognitionPoster.tex) |
| **Final Presentation** | [📄 QRcodeRecognitionPresentation.pdf](./Presentations/QRcodeRecognitionPresentation/QRcodeRecognitionPresentation.pdf) | [QRcodeRecognitionPresentation.tex](./Presentations/QRcodeRecognitionPresentation/QRcodeRecognitionPresentation.tex) |
| **Literature Review** | [📄 QRcodeRecognitionLiteraturePresentation.pdf](./Presentations/QRcodeRecognitionLiteraturePresentation/QRcodeRecognitionLiteraturePresentation.pdf) | [QRcodeRecognitionLiteraturePresentation.tex](./Presentations/QRcodeRecognitionLiteraturePresentation/QRcodeRecognitionLiteraturePresentation.tex) |

---

## 🔧 Hardware & Software

| Component | Specification |
|-----------|---------------|
| **Microcontroller** | Arduino Portenta H7 (Cortex-M7 @ 480MHz) |
| **Camera** | Himax HM01B0 (320×240 QVGA) |
| **WiFi** | IEEE 802.11 b/g/n (2.4 GHz) |
| **Firmware** | OpenMV v4.7.0+ with TensorFlow Lite |
| **ML Framework** | TensorFlow Lite INT8 quantization |
| **Cloud DB** | Supabase (PostgreSQL) |

---

## 📁 Repository Structure

```
ML26-07-QR-Code-Recognition/
├── Code/                              # Source code and scripts
│   ├── DataSet/                       # CNN training pipeline
│   ├── GenerateTestQRcodes/           # QR test dataset generation
│   ├── QRcodeRecognitionCode/         # OpenMV deployment code
│   ├── Testing/                       # Pytest test suite
│   └── ProjectAnalysisVisualizations/ # Performance analysis
├── Documents/                          # Literature and references
├── Manual/                            # User manual
├── Assembly/                          # Hardware assembly guide
├── Poster/                            # Project poster
├── Presentations/                     # Project presentations
├── report/                            # Technical report
└── ProjectManagement/                 # Project planning
```

---

## 🚀 Quick Start

### 1. Hardware Setup
- Connect Portenta Vision Shield to Portenta H7
- Connect RGB LED to pins 1, 2, 3
- See [Assembly Guide PDF](./Assembly/QRcodeRecognitionAssembly.pdf) for details

### 2. Configure System
```python
# Edit Code/QRcodeRecognitionCode/config.py
wifiSsid = "YOUR_WIFI_SSID"
wifiPassword = "YOUR_PASSWORD"
supabaseUrl = "https://your-project.supabase.co"
supabaseKey = "your-anon-key"
```

### 3. Deploy to OpenMV
- Copy files from `Code/QRcodeRecognitionCode/` to OpenMV
- Ensure `qrClassifier.tflite` is present
- Power on and scan QR codes!

**LED Status:**
- 🔵 Blue: Connecting WiFi
- 🟢 Green: Ready to scan
- 🔵 Cyan: Lockout period
- ⚪ White: Uploading (Success)
- 🟡 Yellow: CNN load Failed !
- 🔴 Red: Error !
- 🟣 Purple: WiFi failed !

---

## 🧠 CNN Model

```
Input: 96×96 grayscale
    ↓
Conv2D(16) + MaxPool → 48×48
Conv2D(32) + MaxPool → 24×24
Conv2D(64) + MaxPool → 12×12
    ↓
Dense(64) + Dropout(0.5)
    ↓
Output: 0.0-1.0 (QR confidence)
```

**Specs**: ~600KB, 137ms inference, >90% accuracy

---

## 🔄 Training Your Own Model

```bash
# 1. Data Collection
# Capture images to Code/DataSet/RawDataCapture/

# 2. Preprocessing
cd Code/DataSet
python cropAndConvertImages.py

# 3. Training
pip install -r requirements.txt
python trainBinaryCNN.py

# 4. Test & Convert
python testH5file.py
python convertToTFlite.py
python testTFliteFile.py

# 5. Deploy
# Copy qrClassifier.tflite to OpenMV
```

---

## 🧪 Testing

```bash
cd Code/Testing

# Quick test
pytest -v -m "not slow"

# Full coverage
pytest --cov=.. --cov-report=html
```

**CI/CD**: GitHub Actions runs tests automatically on push/PR

---

## 📊 Performance

| Metric | Value |
|--------|-------|
| **CNN Accuracy** | >90% |
| **Inference Time** | ~137ms |
| **Model Size** | 600-700 KB |
| **FPS with CNN** | ~6-7 FPS |
| **QR Versions** | 1-40 (all) |
| **ECC Levels** | L, M, Q, H (all) |

---


### Literature & References

- **Bibliography Database**: [MyLiterature.bib](./Documents/MyLiterature.bib)

### Code Documentation

- **CNN Training Pipeline**: [Code/DataSet/README_DataSet.md](./Code/DataSet/README_DataSet.md)
- **OpenMV Deployment**: [Code/QRcodeRecognitionCode/README_Main.md](./Code/QRcodeRecognitionCode/README_Main.md)
- **Testing Guide**: [Code/Testing/READMEtesting.md](./Code/Testing/READMEtesting.md)
- **QR Test Generation**: [Code/GenerateTestQRcodes/README_TestGen.md](./Code/GenerateTestQRcodes/README_TestGen.md)

---

## 🛠️ Troubleshooting

**WiFi Issues (Purple LED)**
- Check credentials in `config.py`
- Use 2.4 GHz network only
- Move closer to access point

**CNN Model Not Found**
- Ensure `qrClassifier.tflite` in OpenMV root
- Set `cnnEnabled = False` to test without CNN

**Low Detection Rate**
- Lower `cnnThreshold` to 0.8-0.85
- Improve lighting
- Center QR code in view

**Upload Failures (Red LED)**
- Verify Supabase credentials
- Check internet connectivity
- Review Supabase table schema

---

## 🏗️ System Architecture

```
main.py (Orchestrator)
    ↓
┌───────────┼───────────┐
↓           ↓           ↓
CNN+QR    WiFi+Cloud   LED
Scanner   Manager      Control
    ↓           ↓           ↓
    └───────────┼───────────┘
                ↓
          config.py
```

---

## 🎓 Academic Context

**Course**: ML26-07 (Machine Learning Project)  
**Term**: Winter Semester 2025/2026  
**University**: Hochschule Emden/Leer  
**Department**: MSR-Labor, Abteilung Maschinenbau  

**Learning Outcomes**: Embedded ML (TinyML), Computer Vision, CNN Optimization, IoT Systems, Cloud Integration, Software Engineering

---

## 📄 License

Open-source components:
- **OpenMV Firmware**: MIT License
- **MicroPython**: MIT License
- **TensorFlow Lite**: Apache 2.0 License
- **quirc Library**: ISC License


---

**Repository**: [https://github.com/Wings-hub/ML26-07-QR-Code-Recognition-with-an-Arduino-Portenta-H7](https://github.com/Wings-hub/ML26-07-QR-Code-Recognition-with-an-Arduino-Portenta-H7)

*Last Updated: January 2025*
