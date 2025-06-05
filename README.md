```
# 🔒 XAI-IDS-IoT: Explainable AI-Based DDoS Attack Detection for IoT

A lightweight, explainable Intrusion Detection System designed for IoT and traditional networks. Combines **autoencoders** and **SHAP** for anomaly detection and explainability. Built to perform efficiently on resource-constrained devices.

---

## 🌐 Overview
- Based on the research: **"Explainable AI-Based DDoS Attack Identification Method for IoT Networks"** (Computers 2023, 12, 32).
- Detects DDoS attacks using **autoencoders** for anomaly detection.
- Applies **SHAP** for interpreting model predictions.
- Designed to run on low-power devices like Raspberry Pi 4.

---

## 📊 Key Features

| Feature                     | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| 🔢 Anomaly Detection    | Lightweight autoencoder trained on benign traffic.                         |
| 🔍 Explainable AI        | Uses SHAP to explain feature contributions to detections.                 |
| 📊 Feature Extraction     | 21 features (volumetric, TCP exhaustion, application-layer) via CICFlowMeter. |
| 🚼 Privacy Focus         | Analyzes headers (not payloads) to preserve privacy.                      |
| 🛠️ Optimized Design     | Suitable for IoT, tested on RPi 4 and ZenBook.                           |
| 🔄 Threshold Detection   | Identifies attacks using reconstruction error and feature thresholds.     |

---

## 📅 Dataset: USB-IDS
- Contains 17 labeled CSVs with benign and DDoS traffic (e.g., HULK).
- Extracted using **CICFlowMeter**.

---

## 📄 Requirements
```

Python 3.8+
TensorFlow Lite
Keras
SHAP
Pandas
NumPy
Scikit-learn

```
- 🌀 Feature extraction: CICFlowMeter
- 🚀 Hardware: ASUS ZenBook / Raspberry Pi 4

Install packages:
```

pip install tensorflow keras shap pandas numpy scikit-learn

````

---

## 🚧 Installation
```bash
git clone https://github.com/SreekarSBS/XAI-IDS-IoT.git
cd xai-ddos-detection
````

1. Install CICFlowMeter.
2. Place the USB-IDS dataset into the `data/` directory.
3. Create virtual environment:

```bash
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install -r requirements.txt
```

---

## 🔄 Workflow

### 1. 🎨 Feature Extraction

```bash
python preprocessing/normalize.py
```

Use CICFlowMeter to extract and normalize 84 features from `.pcap` files.

### 2. 💪 Train Autoencoder

```bash
python train_autoencoder.py --data data/benign.csv --output models/autoencoder.h5
```

* 2 hidden layers (10 and 32 neurons)
* ReLU + Adam optimizer
* 40 epochs

### 3. ⚡ Detect Anomalies

```bash
python detect_anomalies.py --model models/autoencoder.h5 --data data/test.csv --output results/anomalies.csv
```

### 4. 🔬 Explain with SHAP

```bash
python explain_anomalies.py --anomalies results/anomalies.csv --model models/autoencoder.h5 --output results/shap_values.csv
```

### 5. 🧠 Map to DDoS Indicators

```bash
python detect_ddos.py --shap results/shap_values.csv --output results/ddos_detections.csv
```

---

## 📁 Project Structure

```
xai-ddos-detection/
├── data/                # USB-IDS dataset
├── models/              # Trained autoencoder
├── results/             # Outputs: anomalies, SHAP values, etc.
├── preprocessing/       # Normalization scripts
├── train_autoencoder.py
├── detect_anomalies.py
├── explain_anomalies.py
├── detect_ddos.py
├── requirements.txt
└── README.md
```

---

## 🌟 Results

**USB-IDS (HULK variants):**

| Attack Type     | Accuracy |
| --------------- | -------- |
| Hulk No Defense | 0.98     |
| Hulk Evasive    | 1.00     |
| Hulk Reqtimeout | 1.00     |

**Compared to Baselines:**

| Model           | Accuracy |
| --------------- | -------- |
| Decision Tree   | 0.97     |
| Random Forest   | 0.98     |
| Deep Neural Net | 0.66     |

**Top Influential Features:**

* Flow packets/s
* Backward packets/s
* Max backward packet length
* Forward packet length std

---

## 📈 Future Work

* Real-time IoT deployment with simulated attacks
* Multi-attack & multi-dataset support
* Further resource optimization

---

## 📚 Citation

```
@article{kalutharage2023explainable,
  title={Explainable AI-Based DDoS Attack Identification Method for IoT Networks},
  author={Kalutharage, Chathuranga Sampath and Liu, Xiaodong and Chrysoulas, Christos and Pitropakis, Nikolaos and Papadopoulos, Pavlos},
  journal={Computers},
  volume={12},
  number={2},
  pages={32},
  year={2023},
  publisher={MDPI}
}
```

---

## 🚑 Contact

For issues or contributions, [open an issue](https://github.com/SreekarSBS/XAI-IDS-IoT/issues) or contact the maintainers.

---

## ✅ License

Licensed under the **MIT License**. See `LICENSE` for details.

```
```
