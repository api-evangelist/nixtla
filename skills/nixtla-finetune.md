---
name: Fine-tune and reuse a TimeGPT model
description: Fine-tune TimeGPT on your own series, save it, then reuse or manage the saved model for later forecasts.
api: openapi/nixtla-openapi-original.json
operations:
  - v2_finetune_v2_finetune_post
  - v2_finetuned_models_v2_finetuned_models_get
  - v2_finetuned_model_v2_finetuned_models__finetuned_model_id__get
  - v2_finetuned_models_delete_v2_finetuned_models__finetuned_model_id__delete
---

# Fine-tune and reuse a TimeGPT model

## Auth
- Base URL `https://api.nixtla.io`; header `Authorization: Bearer <NIXTLA_API_KEY>`.

## Steps
1. `POST /v2/finetune` (`v2_finetune_v2_finetune_post`) with your series and a `finetune_depth`. The response (`FinetuneOutput`) returns a `finetuned_model_id`.
2. Reuse it: pass that `finetuned_model_id` in later `POST /v2/forecast` calls.
3. List your saved models with `GET /v2/finetuned_models` (`v2_finetuned_models_v2_finetuned_models_get`).
4. Inspect one with `GET /v2/finetuned_models/{finetuned_model_id}` (`v2_finetuned_model_v2_finetuned_models__finetuned_model_id__get`).
5. Delete one you no longer need with `DELETE /v2/finetuned_models/{finetuned_model_id}` (`v2_finetuned_models_delete_v2_finetuned_models__finetuned_model_id__delete`) — returns `204`.

## Conventions & errors
- `422` `HTTPValidationError` on invalid input; the model id is server-generated (no client idempotency key).
- Saved models persist across sessions until explicitly deleted — see `data-model/nixtla-data-model.yml`.
