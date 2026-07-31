---
name: Detect anomalies in time series with TimeGPT
description: Flag anomalous points across one or many historical time series, or monitor a live stream in real time, using Nixtla's TimeGPT.
api: openapi/nixtla-openapi-original.json
operations:
  - v2_anomaly_detection_v2_anomaly_detection_post
  - v2_online_anomaly_detection_v2_online_anomaly_detection_post
  - validate_api_key_validate_api_key_get
---

# Detect anomalies with TimeGPT

## Auth
- Base URL `https://api.nixtla.io`; header `Authorization: Bearer <NIXTLA_API_KEY>`.

## Historical (batch) anomaly detection
1. Build the request: series frequency, historical values, confidence `level`, and any exogenous variables.
2. `POST /v2/anomaly_detection` (`v2_anomaly_detection_v2_anomaly_detection_post`).
3. Read `AnomalyDetectionOutput` — each timestamp is flagged anomalous or not against the model's expected interval.

## Real-time (online) anomaly detection
1. For streaming data, use `POST /v2/online_anomaly_detection` (`v2_online_anomaly_detection_v2_online_anomaly_detection_post`) with a `detection_size` window; it uses cross-validation for more robust detection and supports univariate and multivariate series.
2. Read `OnlineAnomalyOutput`.

## Conventions & errors
- JSON request/response; `422` `HTTPValidationError` on bad input (see `errors/nixtla-problem-types.yml`).
- Tune sensitivity with `level`/`detection_size`; higher confidence levels flag fewer points.
