# Bus-Research-373

**Development of an IoT-Based Smart Bus System with Machine Learning-Powered Enhancements for Owner Awareness**

## Abstract

Income leakage, unsafe driving behavior, and lack of operational transparency are major challenges in Sri Lanka’s private bus transportation system. This research develops a Smart Bus Monitoring and Analytics Platform combining IoT sensing, privacy-preserving computer vision, and machine learning to provide reliable passenger monitoring, anomaly detection, revenue reconciliation, profit forecasting, and driver safety alerts. The platform is designed for reproducible research and field pilots.

---

## System Overview

The system comprises four integrated components developed by individual contributors and united through a secure backend and a canonical data model. Key capabilities include resilient edge logging, embedding-based visual analytics (no raw image retention), ML-based forecasting, and safety monitoring.

### Core Objectives

- Prevent income leakage through automated passenger counting and appearance anomaly detection
- Provide profit and demand prediction using historical and real-time data
- Detect unsafe driver behavior and alcohol usage
- Guarantee zero data loss under intermittent connectivity with SD-backed local persistence
- Provide real-time dashboards and audit trails for owners and authorities

---

## Professional architecture diagram

Below is a reusable diagram expressed as a Mermaid block (copy to a Mermaid-capable renderer or export to SVG/PNG for presentations):

```mermaid
flowchart LR
	subgraph Edge[Edge / Vehicle]
		A1[ESP32 Controller]\n- camera & sensors
		A2[Camera Module]\n    A3[GPS Module]\n    A4[Alcohol Sensor]\n    A5[SD Card (Local Store)]
		A1 --> A2
		A1 --> A3
		A1 --> A4
		A1 --> A5
	end

	subgraph Ingestion[Secure Ingestion]
		B1[MQTT / HTTPS Gateway]\n- TLS + Token Auth
		B2[Preprocessing & Validation]
		B1 --> B2
	end

	subgraph Backend[Backend Services]
		C1[Ingestion API]\n    C2[Feature Store & Queue]\n    C3[ML Inference Services]\n    C4[Business Logic & Recon]
		C1 --> C2
		C2 --> C3
		C3 --> C4
	end

	subgraph Storage[Data Layer]
		D1[MongoDB]
		D2[Object Store (models/embeddings)]
		D1 -.-> D2
	end

	subgraph UI[Dashboard & Alerts]
		E1[Web Dashboard]
		E2[Alerting / SMS / Email]
	end

	A5 -->|Batched Uploads| B1
	B2 --> C1
	C4 --> D1
	C3 --> D2
	C4 --> E1
	C4 --> E2

	classDef infra fill:#f9f,stroke:#333,stroke-width:1px;
	class Edge,Ingestion,Backend,Storage,UI infra;
```

If you prefer a PNG/SVG export, I can generate and add `docs/architecture.svg`.

---

## Research Components

### 1. Passenger Appearance Anomaly Detection (GAN-Based)

**Contributor:** Sandev Jayaweera

- Notebook: `gan-for-passenger-appearance-anomaly-detection.ipynb`
- Learns distribution of normal boarding appearances using GANs (or VAE alternatives) on deep embeddings.
- Anomaly scoring via reconstruction error and discriminator outputs; stores embeddings and scores (no raw images).

**Tech stack:** TensorFlow / Keras, OpenCV, embedding extractors.

### 2. Bus Travel Profit Prediction Using Machine Learning

**Contributor:** Sanduni

- Notebook: `bus-travel-profit-prediction-using-ml.ipynb`
- Performs feature engineering from journey logs, ticketing, and temporal covariates; evaluates regressors and ensembles for route-level and time-series forecasting.

**Tech stack:** Scikit-learn, Pandas, NumPy, XGBoost/LightGBM for baselines.

### 3. Driver Alcohol Level Detection

**Contributor:** Jaladhi

- Notebook: `alcohol-level-detection.ipynb`
- Processes raw sensor readings and uses calibrated ML classifiers and threshold policies to produce actionable alerts.

**Tech stack:** Sensor integration libraries, scikit-learn, lightweight on-device heuristics.

### 4. IoT Data Collection & Revenue Processing (Edge)

**Contributor:** Nandun

My part of the research focuses on the IoT data collection and revenue processing system. I use an ESP32 microcontroller as the primary controller to manage passenger event data and communicate with the backend server. A GPS module captures precise location and timestamps for passenger get-in and get-out events. To handle low or no network coverage, an SD card module stores all events and location data locally; when connectivity is restored the stored data is automatically uploaded to the backend, ensuring no trip or revenue data is lost. The backend processes collected records to calculate trip-wise and date-wise revenue, which owners can view through a simple web interface.

**Tech stack:** ESP32 (Arduino/ESP-IDF), GPS modules, SD card logging, secure HTTPS/MQTT clients.

---

## Data Model (MongoDB canonical collections)

- `passenger_embeddings`: { _id, journey_id, bus_id, ts, embedding, recon_error, discriminator_score, embedding_hash }
- `journeys`: { _id, bus_id, route_id, start_ts, end_ts, ticket_count, reconciled_revenue }
- `gps_logs`: { journey_id, ts, lat, lon, speed }
- `revenue_records`: { journey_id, ticket_count, computed_revenue, reported_revenue, reconciliation_status }
- `alcohol_alerts`: { journey_id, driver_id, ts, sensor_value, model_score, alert_sent }
- `anomaly_events`: { journey_id, ts, anomaly_type, score, evidence_ref }
- `system_logs`: { ts, device_id, event_type, message }

For production, define JSON Schemas and indexes in `data/schema.md`, including TTL indexes for ephemeral logs and shard keys for `passenger_embeddings`.

---

## Deployment & Development (Quickstart)

Prereqs: Python 3.10+, pip, Node.js (for dashboard), Docker (optional), MongoDB instance.

Development setup:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Run backend locally:

```bash

export MONGO_URI="mongodb://localhost:27017/bus_research"
python app.py
```

Edge flashing (ESP32): follow `edge/README.md` (or `edge/firmware/` instructions) to flash and configure device tokens. Ensure `edge/config.example.json` is used to set device IDs and server endpoints.

---

## Privacy, Security & Ethics

- Do not store raw camera frames. Persist embeddings only and rotate any identification hashes regularly.
- All device↔server traffic must be TLS-encrypted and authenticated with rotating tokens.
- Use field-level encryption for PII in MongoDB and RBAC for dashboard access.
- Include opt-out and data-retention policies to comply with local regulations.

---

## Contributors & Individual Contribution Summary

- **Sandev Jayaweera** — Passenger appearance anomaly detection research (GAN architectures, embedding pipeline, experiments).
- **Sanduni** — Revenue and demand forecasting (feature engineering, model baselines, evaluation).
- **Jaladhi** — Driver alcohol detection (sensor integration, model training, alert policies).
- **Nandun** — Edge systems and IoT data collection (ESP32 firmware, GPS & SD logging, reliable upload & revenue reconciliation).

If you contributed and want your institutional contact or ORCID listed, add details to the `AUTHORS.md` file.

---

## Reproducibility & Experiments

- Save dataset splits under `data/splits/` and model checkpoints under `models/` with metadata files describing seeds and environment.
- Store experiment logs and metrics in `experiments/` for traceability; link each entry to the commit hash that generated it.

---

## Next steps (recommended for a pilot)

1. Sanitize and prepare a representative dataset; populate `data/` with anonymized records.
2. Create `data/schema.md` and `edge/README.md` if missing.
3. Export the Mermaid diagram as `docs/architecture.svg` and commit it for presentations.

---

## License

This repository is for academic research and pilot deployments. Add a LICENSE file (MIT or Apache-2.0 recommended) before commercial or production use.



