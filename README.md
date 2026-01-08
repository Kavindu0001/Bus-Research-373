# Bus-Research-373

## Title

**Development of an IoT-Based Smart Bus System with Machine Learning-Powered Enhancements for Owner Awareness**

## Abstract

Income leakage, unsafe driving behavior, and lack of operational transparency are major challenges in Sri Lanka’s private bus transportation system. This research proposes a **Smart Bus Monitoring and Analytics Platform** that integrates **IoT devices, computer vision, and machine learning models** to provide real-time passenger monitoring, anomaly detection, profit prediction, and driver behavior analysis. The system aims to improve financial accountability, operational efficiency, passenger safety, and data-driven decision-making for bus owners.

---

## System Overview

The proposed system is composed of four independent but interconnected research components, each developed by a team member and integrated under a unified backend and database.

### Core Objectives

* Prevent income leakage through automated passenger counting and anomaly detection
* Provide profit and demand prediction using historical and real-time data
* Detect unsafe driver behavior and alcohol usage
* Offer real-time dashboards for owners and authorities

---

## Architecture Diagram (High-Level)

```
+-------------------+        +---------------------+
|   IoT Devices     |        |  Camera Modules     |
|-------------------|        |---------------------|
| - IR / Sensors    |        | - Passenger Camera  |
| - GPS Module      |        | - Driver Camera     |
| - Alcohol Sensor  |        +----------+----------+
+---------+---------+                   |
          |                             |
          v                             v
+-----------------------------------------------+
|        Edge Processing (ESP32 / Local AI)     |
|-----------------------------------------------|
| - Passenger counting                          |
| - Image preprocessing                         |
| - Sensor data filtering                       |
+-------------------+---------------------------+
                    |
                    v
+--------------------------------------------------+
|            Backend Server (Flask API)            |
|--------------------------------------------------|
| - GAN / Siamese / ML inference                   |
| - Business logic                                 |
| - Authentication                                 |
| - REST & Socket APIs                             |
+-------------------+------------------------------+
                    |
                    v
+--------------------------------------------------+
|              MongoDB Database                    |
|--------------------------------------------------|
| - Passengers                                     |
| - Journeys                                       |
| - Sensor Logs                                    |
| - Alerts & Anomalies                             |
+-------------------+------------------------------+
                    |
                    v
+--------------------------------------------------+
|           Web Dashboard / Admin UI               |
|--------------------------------------------------|
| - Live monitoring                                |
| - Reports & analytics                            |
| - Alerts visualization                           |
+--------------------------------------------------+
```

---

## Research Components

### 1. Passenger Appearance Anomaly Detection (GAN-Based)

**Contributor:** Sandev Jayaweera
**Notebook:** `gan-for-passenger-appearance-anomaly-detection.ipynb`

* Uses a **Generative Adversarial Network (GAN)** to learn normal passenger appearance patterns
* Detects anomalies such as repeated passengers, unusual boarding patterns, or potential fare fraud
* Stores **learned embeddings**, not raw images, to preserve privacy

**Tech Stack:**

* TensorFlow / Keras
* OpenCV
* GAN architecture with discriminator-based anomaly scoring

---

### 2. Bus Travel Profit Prediction Using Machine Learning

**Contributor:** Sanduni
**Notebook:** `bus-travel-profit-prediction-using-ml.ipynb`

* Predicts route-wise and time-based profit using historical ticketing and passenger data
* Supports decision-making for route optimization and scheduling
* Evaluates multiple ML models and performance metrics

**Tech Stack:**

* Scikit-learn
* Pandas, NumPy
* Regression & ensemble models

---

### 3. Driver Alcohol Level Detection

**Contributor:** Jaladhi
**Notebook:** `alcohol-level-detection.ipynb`

* Detects alcohol consumption using sensor data integrated with ML classification
* Triggers real-time alerts to bus owners and authorities
* Designed for integration with in-bus breathalyzer hardware

**Tech Stack:**

* Machine Learning classifiers
* Sensor data preprocessing
* Threshold-based and ML-based detection

---

## Database Design (MongoDB)

Collections:

* `passengers` – passenger embeddings and metadata
* `journeys` – trip-level data
* `images` – processed image references (no raw storage)
* `alerts` – anomaly and safety alerts
* `system_logs` – operational logs

Indexes are created for fast retrieval and real-time analytics.

---

## Deployment Overview

* **Edge Layer:** ESP32 + camera + sensors
* **Backend:** Flask API with ML inference
* **Database:** MongoDB
* **Frontend:** Web dashboard (future extension)

---

## Privacy & Ethics

* No raw passenger images are permanently stored
* Only numerical embeddings and anonymized features are saved
* Designed in compliance with ethical AI and privacy-by-design principles

---

## How to Run (Development)

```bash
# Activate environment
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run backend
python app.py
```

---

## Future Enhancements

* Unified dashboard for all modules
* Real-time bus crowd heatmap visualization
* Dynamic fare and schedule optimization
* Government authority integration

---

## Contributors

* **Sandev Jayaweera** – Passenger Anomaly Detection (GAN)
* **Sanduni** – Profit Prediction Module
* **Jaladhi** – Alcohol Level Detection Module

---

## License

This project is developed for academic research purposes under the Final Year Research Project (52-week duration).
