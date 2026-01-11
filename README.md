# 25-26J-373

# SmartBusAI: IoT-Based Smart Bus System with Machine Learning-Powered Owner Awareness

## 👥 Team Members

### Group Leader: Sandev Jayaweera – GAN & Computer Vision  
### Member 1: Sanduni – Profit Prediction (ML)  
### Member 2: Jaladhi – Driver Alcohol Detection  
### Member 3: Nandun – ESP32 IoT & Revenue Collection  

---

## Overview

**SmartBusAI** is an IoT-enabled, machine-learning powered decision support platform designed to eliminate revenue leakage, improve safety, and optimize profitability in Sri Lanka’s private bus transportation system.  

The system integrates **AI vision, sensor-based IoT, GPS tracking, and predictive analytics** to provide **real-time passenger tracking, anomaly detection, revenue validation, profit forecasting, and driver safety monitoring**.

---

## Problem Statement

Sri Lanka’s private bus industry suffers from:

- Manual ticketing and under-reported income  
- Passenger fraud and repeated boarding  
- Unsafe driver behavior  
- No reliable method to verify trip-wise earnings  
- Poor operational planning  

Bus owners depend on conductors and drivers for revenue reporting, which leads to **income leakage and lack of transparency**.

---

## Purpose

SmartBusAI aims to:

- 🚍 Automatically count passengers using AI and IoT  
- 💰 Verify real revenue per trip  
- 📊 Predict profits and demand  
- 🍺 Detect alcohol-impaired driving  
- 📡 Guarantee zero data loss using SD-based buffering  
- 🖥 Provide a real-time dashboard for owners  

---

## System Overview Diagram

                  +-----------------------+     +------------------------+
|   In-Bus Sensors      |     |      Camera Units      |
|-----------------------|     |------------------------|
| - GPS Module          |     | - Passenger Camera     |
| - Alcohol Sensor     |     | - Driver Camera        |
| - IR / Entry Sensors |     +-----------+------------+
+-----------+-----------+                 |
            |                             |
            v                             v
+----------------------------------------------------+
|          Edge Layer – ESP32 Controller             |
|----------------------------------------------------|
| - Passenger event detection                        |
| - GPS tagging                                      |
| - Local SD-card storage                            |
| - Data synchronization                             |
+----------------------+-----------------------------+
                       |
                       v
+----------------------------------------------------+
|              Backend Server (Flask)                |
|----------------------------------------------------|
| - GAN anomaly detection                            |
| - Profit prediction ML                             |
| - Alcohol detection logic                          |
| - API & authentication                             |
+----------------------+-----------------------------+
                       |
                       v
+----------------------------------------------------+
|                   MongoDB                          |
|----------------------------------------------------|
| - Passenger embeddings                             |
| - GPS & trip logs                                  |
| - Revenue records                                  |
| - Alerts & anomalies                               |
+----------------------+-----------------------------+
                       |
                       v
+----------------------------------------------------+
|            Web Dashboard for Bus Owners            |
|----------------------------------------------------|
| - Live monitoring                                  |
| - Revenue reports                                  |
| - Alerts & analytics                               |
+----------------------------------------------------+

---

## Components

### 👤 GAN-Based Passenger Anomaly Detection (Sandev)

✔ Learns **normal passenger appearance patterns**  
✔ Uses **GAN + deep embeddings** instead of storing images  
✔ Detects:
- Repeated passengers  
- Fare evasion  
- Suspicious behavior  

✔ Privacy-preserving visual intelligence  

---

### 💰 Bus Travel Profit Prediction (Sanduni)

✔ Predicts:
- Route-wise revenue  
- Time-based demand  
- Profit trends  

✔ Uses ML on historical passenger and journey data  
✔ Supports route planning and scheduling  

---

### 🍺 Driver Alcohol Detection (Jaladhi)

✔ Uses sensor-based ML classification  
✔ Detects unsafe alcohol levels  
✔ Sends alerts to bus owners and authorities  

---

### 📡 IoT Passenger Event & Revenue Collection (Nandun)

✔ ESP32 microcontroller as core controller  
✔ GPS records boarding & exit location + time  
✔ SD card stores data when internet fails  
✔ Auto-syncs when network returns  
✔ Guarantees **100% trip & revenue integrity**  

✔ Provides:
- Trip-wise revenue  
- Daily earnings  
- Ground truth for ML models  

---

## Dependencies

### Edge & Hardware
- ESP32
- GPS Module
- Alcohol Sensor
- Camera
- SD Card Module

### Backend
- Flask (Python)
- REST APIs
- SocketIO

### Database
- MongoDB

### Machine Learning
- TensorFlow / Keras
- Scikit-learn
- OpenCV
- Pandas, NumPy

---

## Expected Outcomes

- Passenger identification accuracy: **80+%**  
- Fraud/anomaly detection: **High precision using GANs**  
- Revenue accuracy: **Near-100% (IoT-verified)**  
- Alcohol detection accuracy: **>90%**  
- Profit prediction accuracy: **75–85%**

---

## Social & Economic Impact

- Eliminates income leakage  
- Protects passengers from unsafe drivers  
- Increases profitability for bus owners  
- Supports digital transformation of Sri Lanka’s transport sector  

---

## Key Technologies Used

| Category | Tools |
|--------|------|
| Computer Vision | OpenCV, GANs |
| Machine Learning | TensorFlow, Scikit-learn |
| IoT | ESP32, GPS, Sensors |
| Backend | Flask, Python |
| Database | MongoDB |
| Data Handling | Pandas, NumPy |
| Version Control | GitHub |

---

## Ethical & Data Considerations

- No raw passenger images stored  
- Only anonymized embeddings saved  
- GPS & revenue encrypted  
- Complies with SLIIT research ethics  

---

## How to Run (Development)

```bash
source .venv/bin/activate
pip install -r requirements.txt
python app.py
