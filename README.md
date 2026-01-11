# Bus-Research-373

## Title
**Development of an IoT-Based Smart Bus System with Machine Learning-Powered Enhancements for Owner Awareness**

---

## Abstract

Income leakage, unsafe driving behavior, and lack of operational opacity are major challenges in Sri Lanka’s private bus transportation system. This research proposes a **Smart Bus Monitoring and Analytics Platform** that integrates **IoT devices, computer vision, and machine learning models** to provide real-time passenger monitoring, automated revenue verification, anomaly detection, profit forecasting, and driver safety monitoring.

The system combines **edge-level IoT sensing**, **cloud-based AI analytics**, and **web-based dashboards** to improve **financial accountability, operational efficiency, and passenger safety** for bus owners and transport authorities.

---

## System Overview

The platform is designed as a **cyber-physical system** in which physical devices installed inside the bus continuously collect data and feed it to an intelligent backend for analysis and decision-making.

### Core Objectives

- Prevent income leakage through automated passenger tracking and anomaly detection  
- Predict profits and passenger demand using historical and real-time data  
- Detect unsafe driving behavior and alcohol usage  
- Guarantee zero data loss even in poor network conditions  
- Provide real-time and historical insights to bus owners  

---

## Overall System Architecture


---

## Research Components

### 1. Passenger Appearance Anomaly Detection (GAN-Based)  
**Contributor:** Sandev Jayaweera  
**Notebook:** `gan-for-passenger-appearance-anomaly-detection.ipynb`

This module uses a **Generative Adversarial Network (GAN)** to learn the normal appearance patterns of passengers entering the bus. Instead of storing raw images, the system extracts **deep feature embeddings** that describe visual characteristics.

The GAN learns what a “normal” passenger looks like and flags anomalies such as:
- Repeated boarding of the same person  
- Fare-evasion attempts  
- Unusual passenger behavior patterns  

Anomalies are detected using **reconstruction error and discriminator scores**.

**Technologies**
- TensorFlow / Keras  
- OpenCV  
- Deep feature embeddings + GAN-based anomaly detection  

---

### 2. Bus Travel Profit Prediction Using Machine Learning  
**Contributor:** Sanduni  
**Notebook:** `bus-travel-profit-prediction-using-ml.ipynb`

This component predicts:
- Route-wise revenue  
- Time-based profit trends  
- Passenger demand patterns  

using historical ticketing, journey, and passenger data. The model helps bus owners optimize **routes, schedules, and fleet utilization**.

**Technologies**
- Scikit-learn  
- Pandas, NumPy  
- Regression and ensemble learning models  

---

### 3. Driver Alcohol Level Detection  
**Contributor:** Jaladhi  
**Notebook:** `alcohol-level-detection.ipynb`

This module detects whether a driver is under the influence of alcohol using **sensor data combined with ML-based classification**. When unsafe alcohol levels are detected, alerts are generated for bus owners and authorities.

---

### 4. IoT-Based Passenger Event & Revenue Collection System  
**Contributor:** Nandun  

This is the **physical backbone** of the entire system.

**Hardware Architecture**

- **ESP32 microcontroller** as the main controller  
- **GPS module** to capture passenger boarding and exit locations and timestamps  
- **SD card module** to store data locally when internet is unavailable  

When connectivity returns, the ESP32 automatically uploads all stored data to the backend, ensuring **zero data loss**.

The backend uses this data to:
- Calculate trip-wise and date-wise revenue  
- Feed ML models for profit prediction  
- Validate passenger counts and detect fraud  

---

## Database Design (MongoDB)

Collections:
- `passenger_embeddings`  
- `journeys`  
- `gps_logs`  
- `revenue_records`  
- `alcohol_alerts`  
- `anomaly_events`  
- `system_logs`  

---

## Technology Stack

| Layer | Technology |
|------|-----------|
| Edge | ESP32, GPS, Camera, Alcohol Sensor, SD Card |
| Backend | Flask, Python |
| AI Models | TensorFlow, Scikit-learn |
| Database | MongoDB |
| Frontend | Web Dashboard |

---

## Privacy & Ethics

- No raw passenger images are stored  
- Only numerical embeddings are saved  
- GPS and revenue data are encrypted  
- Designed following **privacy-by-design principles**  

---

## Contributors

- **Sandev Jayaweera** – GAN-based Passenger Anomaly Detection  
- **Sanduni** – Profit Prediction ML Module  
- **Jaladhi** – Alcohol Level Detection  
- **Nandun** – ESP32-based IoT Data Collection & Revenue System  

---

