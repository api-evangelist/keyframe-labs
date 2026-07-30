---
name: Start a Keyframe persona session
description: Create a real-time persona video session, optionally choosing a voice and LLM model, and connect to the returned realtime endpoint.
api: openapi/keyframe-labs-sessions-openapi-original.json
operations: [listVoices, listLlmModels, createSession]
---

# Start a Keyframe persona session

Use this skill to spin up a live, lifelike persona video call for an agent or
application.

## Auth
- Create a bearer API key in the platform dashboard
  (https://platform.keyframelabs.com/api-keys).
- Send it on every request: `Authorization: Bearer <api_key>`.
- Base URL: `https://api.keyframelabs.com/v1`.

## Steps
1. (Optional) `listVoices` (`GET /voices`) to pick a `voice_id`, and
   `listLlmModels` (`GET /llm-models`) to pick a `model_id`.
2. `createSession` (`POST /sessions`) to start the session. The response returns
   `server_url`, `participant_token`, and `agent_identity`.
3. Connect your client to `server_url` using `participant_token` to join the
   realtime persona call.

## Rules
- Errors come back as `{ "detail": "<message>" }` (see
  errors/keyframe-labs-problem-types.yml).
- `403` means the selected model is not allowed on your plan — pick a permitted
  model or upgrade.
- `503` means no capacity / concurrency limit reached — retry with backoff.
- There is no idempotency-key contract; do not assume automatic de-duplication
  of `POST /sessions` (see conventions/keyframe-labs-conventions.yml).
