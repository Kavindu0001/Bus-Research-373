Bus-Research-373
Title
Development of an IoT-Based Smart Bus System with Machine Learning-Powered Enhancements for Owner Awareness

Abstract
Income leakage, unsafe driving behavior, and lack of operational transparency are major challenges in Sri Lanka’s private bus transportation system. This research presents a Smart Bus Monitoring and Analytics Platform that integrates IoT devices, computer vision, and machine learning models to provide real-time passenger tracking, revenue verification, anomaly detection, profit forecasting, and driver safety monitoring.
The system combines edge-level IoT data collection, cloud-based AI analytics, and web-based dashboards to improve financial accountability, operational efficiency, and passenger safety for bus owners.

System Overview
The platform is designed as a modular cyber-physical system where hardware and AI components work together.
Core Objectives
Prevent income leakage through automated passenger tracking and anomaly detection
Predict profits and demand using historical and real-time data
Detect unsafe driving and alcohol usage
Ensure zero data loss during low network coverage
Provide real-time and historical insights to bus owners

Architecture Diagram

System Overview
The platform is designed as a modular cyber-physical system where hardware and AI components work together.
Core Objectives
Prevent income leakage through automated passenger tracking and anomaly detection
Predict profits and demand using historical and real-time data
Detect unsafe driving and alcohol usage
Ensure zero data loss during low network coverage
Provide real-time and historical insights to bus owners

Research Components
1. Passenger Appearance Anomaly Detection (GAN-Based)
Contributor: Sandev Jayaweera
Notebook: gan-for-passenger-appearance-anomaly-detection.ipynb
This module uses a Generative Adversarial Network (GAN) to learn the normal visual patterns of passengers entering the bus. Instead of storing images, the system extracts deep feature embeddings that represent appearance patterns.
Anomalies such as:
Repeated boarding of the same person
Fare-evasion attempts
Suspicious movement patterns
are detected using GAN-based reconstruction and discriminator scoring.
Technologies
TensorFlow / Keras
OpenCV
Deep feature embedding + GAN anomaly scoring

2. Bus Travel Profit Prediction Using Machine Learning
Contributor: Sanduni
Notebook: bus-travel-profit-prediction-using-ml.ipynb
This component predicts:
Route-wise revenue
Time-based profit trends
Demand fluctuations
using historical ticketing and passenger data. The model supports bus owners in route planning, scheduling, and profitability analysis.
Technologies
Scikit-learn
Pandas, NumPy
Regression & ensemble ML models

4. IoT-Based Passenger Event & Revenue Collection System
Contributor: Nandun
This is the core cyber-physical layer of the system.
Nandun’s module ensures that no revenue data is ever lost and that all AI predictions are grounded in accurate physical-world data.
Hardware Architecture
ESP32 microcontroller acts as the central controller
GPS module captures:
Location of passenger boarding and exit
Time stamps for each event
SD card module stores all data locally when:
Internet is unavailable
Network is unstable
When connectivity returns, the ESP32:
Automatically uploads stored records to the backend
This guarantees 100% trip and revenue data integrity, even in rural or low-signal routes.
The backend uses this data to:
Calculate trip-wise revenue
Generate date-wise earnings
Feed ML models for profit prediction and anomaly detection
Bus owners view this information through a simple web dashboard.

Database Design (MongoDB)
Collections:
passenger_embeddings
journeys
gps_logs
revenue_records
alcohol_alerts
anomaly_events
system_logs
Optimized indexes support real-time queries and historical analysis.

Deployment Stack
Layer	Technology
Edge	ESP32, GPS, Camera, Alcohol Sensor, SD Card
Backend	Flask, Python
AI Models	TensorFlow, Scikit-learn
Database	MongoDB
Frontend	Web dashboard

Privacy & Ethics
No raw passenger images are stored
Only numerical embeddings are saved
GPS and revenue data are encrypted
Designed using privacy-by-design principles

## How to Run (Development)
bash
# Activate environment
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run backend
python app.py

Contributors
Sandev Jayaweera – GAN-based Passenger Anomaly Detection
Sanduni – Profit Prediction ML Module
Jaladhi – Alcohol Level Detection
Nandun – ESP32-based IoT Data Collection & Revenue System
