# Bus-Research-373

**Development of an IoT-Based Smart Bus System with Machine Learning Enhancements for Operational Transparency and Owner Awareness**

## Executive summary

This project delivers a production-ready design and prototype implementation for a Smart Bus Monitoring and Analytics Platform that addresses revenue leakage, unsafe driving, and operational opacity in private bus fleets. The system combines resilient edge data collection (ESP32 + sensors), privacy-preserving computer vision (feature embeddings, no raw images), server-side AI analytics (GAN-based anomaly detection, revenue forecasting), and a secure backend for storage and dashboards. The repository contains research notebooks, dataset schemas, and deployment guidance for reproducible evaluation and real-world pilot deployment.

## Key contributions

- A GAN-based passenger appearance anomaly detection pipeline that operates on deep embeddings (privacy-preserving).
- A route- and time-aware revenue and profit forecasting component using tabular ML models.
- An alcohol-detection module for driver safety using on-device sensors and ML classification.
- A fault-tolerant ESP32-edge data collection architecture that guarantees eventual consistency (local SD persistence and batched uploads).
- Comprehensive data schemas, evaluation protocols, and deployment guidance for field pilots.

## Research impact and novelty

- Introduces a practical, privacy-first approach to passenger anomaly detection by storing only embeddings and reconstruction metrics rather than raw images.
- Demonstrates integration of edge resilience (SD-backed logging) with cloud ML pipelines to ensure zero data loss under intermittent connectivity.
- Provides methods and baseline results for both anomaly detection and revenue forecasting suitable for transport operators and researchers.

## Repository contents

- `gan-for-passenger-appearance-anomaly-detection.ipynb` — research notebook with training, evaluation, and embedding extraction pipeline.
- `bus-travel-profit-prediction-using-ml.ipynb` — data preprocessing, feature engineering, and forecasting baselines.
- `alcohol-level-detection.ipynb` — sensor data processing and classification experiments.
- `edge/` — (recommended) firmware sketches and integration notes for ESP32, GPS, and SD logging.
- `data/schema.md` — canonical MongoDB collection schemas and example documents.
- `deploy/` — deployment manifests, Dockerfiles, and helm charts (if applicable).
- `README.md` — this document.

If some folders are missing, see the Notebooks above for experimental code and the `data/schema.md` for required collection designs.

## System architecture (high level)

1. Edge devices (ESP32) collect sensor data: camera frame embeddings, GPS timestamps, SD-backed logs, and alcohol sensor readings.
2. Edge batches and signs payloads; when network is available, data is uploaded to the backend via a secure API (TLS + token auth).
3. Backend ingests events into MongoDB (collections listed below) and forwards features to ML services for inference and storage of anomaly/revenue events.
4. Dashboards and alerting services surface anomalies, revenue reconciliations, and driver safety alerts to owners.

## Data model

Primary MongoDB collections (canonical fields):

- `passenger_embeddings`: { _id, journey_id, timestamp, embedding: [float32], recon_error, discriminator_score, embedding_hash }
- `journeys`: { _id, bus_id, route_id, start_ts, end_ts, gps_summary }
- `gps_logs`: { journey_id, ts, lat, lon, speed }
- `revenue_records`: { journey_id, ticket_count, computed_revenue, reported_revenue, reconciliation_status }
- `alcohol_alerts`: { journey_id, driver_id, ts, sensor_value, model_score, alert_sent }
- `anomaly_events`: { journey_id, ts, anomaly_type, score, evidence_ref }
- `system_logs`: { ts, device_id, event_type, message }

See `data/schema.md` for JSON Schema definitions and indexes (sharding/TTL recommendations).

## Methodology and evaluation

- Passenger Anomaly Detection: train a GAN (or VAE) on embeddings of 'normal' boarding frames. Use reconstruction error + discriminator confidence to score anomalies. Evaluate with precision/recall, AUROC, and event-level F1 on curated test sets (synthetic repeat boarding, occlusion, and fare-evasion scenarios).
- Revenue Forecasting: compare baselines (linear regression, RandomForest, XGBoost) using RMSE, MAE, and quantile coverage for demand prediction. Run cross-validation by route and day-of-week folds to avoid temporal leakage.
- Alcohol Detection: evaluate accuracy, precision/recall, and ROC; calibrate thresholds for alert policies to trade off false positives and operational risk.

Reproducibility notes: all experiments include seed control, dataset splits saved to `data/splits/`, and model checkpoints saved to `models/`.

## Getting started — development and reproduction

Prerequisites: Python 3.10+, pip, Docker (optional), and a MongoDB instance (local or remote).

Quick setup (development):

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Run the GAN notebook locally in Jupyter or use `nbscripts/run_gan.py` (if provided) to reproduce training with the default config:

```bash
jupyter lab
# or run training script
python nbscripts/run_gan.py --config configs/gan_default.yaml
```

Deployment (edge + backend) notes:

- Edge: flash ESP32 firmware from `edge/firmware/` and configure device tokens in `edge/config.example.json`.
- Backend: provided Dockerfile builds a container exposing the ingestion API. Use `deploy/docker-compose.yml` for single-host pilots.

## Security, privacy, and ethics

- No raw images are persisted; only numeric embeddings and compact audit trails are stored.
- All network traffic must use TLS; device tokens must be rotated periodically.
- GPS and revenue data are sensitive: use field-level encryption in MongoDB for PII, and restrict dashboard access via RBAC.
- The system design includes differential access controls and an opt-out mechanism where required by local regulation.

## Performance and scalability guidance

- Shard `passenger_embeddings` by `bus_id` or `route_id` for high-volume deployments.
- Use a GPU-enabled inference service for embedding extraction at scale; for small pilots, on-device embedding extraction is supported.
- Batch ingestion and idempotency keys prevent duplicate records after intermittent uploads.

## Evaluation baseline results (example)

- Passenger-anomaly AUROC: 0.92 (validation set)
- Revenue-forecast RMSE: route-level 45.7 LKR
- Alcohol-detection accuracy: 0.88 (threshold tuned)

Include exact experiment logs and seeds in `experiments/` for traceability.

## Maintenance, monitoring, and alerting

- Operational metrics: ingestion latency, upload success rate, embedding queue depth, model inference latency.
- Recommended alerts: data-starvation (no uploads for X hours), spike in anomaly rates, device offline > 24h.

## How to contribute

- Fork the repository and open PRs to `member-sandev` branch.
- Add unit tests for new components and update `data/schema.md` when changing collection shapes.
- Label issues with `infra`, `model`, or `edge` to triage work.

## Authors and contacts

- Sandev Jayaweera — Passenger anomaly detection. Email: (add institutional contact).
- Sanduni — Revenue forecasting.
- Jaladhi — Alcohol detection.
- Nandun — Edge systems and firmware.

## Licensing and citations

- This repository is provided for research and pilot deployments. Add a LICENSE file with your preferred license (e.g., MIT or Apache-2.0) before production use.
- Cite this work: (provide formal citation when available).

## Next steps (for a pilot)

1. Prepare sample dataset and populate `data/` with sanitized records.
2. Run notebooks to reproduce baseline results and save artifacts to `experiments/`.
3. Deploy a single-bus pilot with one ESP32 and monitor ingestion and anomaly rates for 2 weeks.

---


