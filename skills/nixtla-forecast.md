---
name: Forecast a time series with TimeGPT
description: Produce a multi-horizon forecast for one or many time series using Nixtla's TimeGPT foundation model, zero-shot (no training).
api: openapi/nixtla-openapi-original.json
operations:
  - v2_forecast_v2_forecast_post
  - get_model_params_model_params_get
  - validate_api_key_validate_api_key_get
---

# Forecast a time series with TimeGPT

Use this skill to generate a forecast from the Nixtla Forecast API.

## Auth
- Base URL: `https://api.nixtla.io` (set `NIXTLA_BASE_URL` to override; TimeGPT-2 uses `https://api-preview.nixtla.io`).
- Send `Authorization: Bearer <NIXTLA_API_KEY>` on every request.
- Optionally verify the key first with `GET /validate_api_key` (`validate_api_key_validate_api_key_get`).

## Steps
1. (Optional) Call `GET /model_params` (`get_model_params_model_params_get`) with your model and series frequency to learn the model's `input_size` and `horizon` so you supply enough history.
2. Assemble the request body: series frequency, historical values per series, the forecast horizon `h`, and any exogenous variables. To run against a fine-tuned model, include its `finetuned_model_id`.
3. `POST /v2/forecast` (`v2_forecast_v2_forecast_post`). Select the model via the `nixtla-model` request header if not defaulting.
4. Read `ForecastOutput` — predicted values plus any requested prediction intervals / feature contributions.

## Conventions & errors
- Requests/response are JSON; the SDK zstd-compresses bodies over 1MB. No idempotency key — a forecast is a pure compute call, safe to retry.
- A malformed payload returns `422` with a FastAPI `HTTPValidationError` (`{"detail":[{loc,msg,type}]}`) — see `errors/nixtla-problem-types.yml`.
- No pagination; the whole series batch is sent and returned in one call.
