---
name: Send a Keyframe persona into a meeting
description: Create a meeting bot that joins a Zoom, Google Meet, or Microsoft Teams call as a persona, poll its status, and stop it.
api: openapi/keyframe-labs-sessions-openapi-original.json
operations: [createMeetBot, getMeetBotStatus, stopMeetBot]
---

# Send a Keyframe persona into a meeting

Use this skill to have a Keyframe persona join a live video meeting.

## Auth
- Bearer API key in the `Authorization: Bearer <api_key>` header.
- Base URL: `https://api.keyframelabs.com/v1`.

## Steps
1. `createMeetBot` (`POST /meet-bot`) with the meeting details. The response
   returns `bot_id` and `meeting_url`.
2. `getMeetBotStatus` (`GET /meet-bot/{bot_id}`) to poll `status` until the bot
   is in the meeting.
3. `stopMeetBot` (`DELETE /sessions`, `bot_id` parameter) to remove the bot when
   the persona should leave the call.

## Rules
- Errors use the `{ "detail": "<message>" }` envelope
  (errors/keyframe-labs-problem-types.yml).
- `404` means the `bot_id` is unknown; `502` is a transient upstream failure
  (the meeting-bot provider) — retry.
- `403` means the model is not permitted on your plan.
- Always call `stopMeetBot` to release the bot and stop metered minutes.
