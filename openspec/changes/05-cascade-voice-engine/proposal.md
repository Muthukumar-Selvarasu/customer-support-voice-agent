# Proposal: Cascade Voice Engine (`05-cascade-voice-engine`)

## Why

The customer-facing spoken channel is STT → (later) text pipeline → TTS over WebSocket `/voice/ws`. PoC already proved multi-burst accumulate-and-combine (`1500 ms` settle) and barge-in. This slice copies those modules and unit-tests them without mounting the full shopper FastAPI app.

`router.py` receives the text pipeline by dependency injection, so Group 5 does not need Group 6's `web.py`.

## What Changes

- Add `src/voice/{stt.py, tts.py, cost.py, router.py, ui.js}`.
- Keep `STREAM_ENABLED = AGENT_RESPONSE_STREAM and not MASK`.
- Remove the startup warning that masking is a no-op without `GOOGLE_CLOUD_PROJECT` (Group 3 made masking self-contained).
- Preserve `1500 ms` settle, barge-in `interrupt`, PCM framing, and cost accounting.

## Capabilities

### New Capabilities
- `cascade-voice-engine`: Browser WebSocket voice, STT/TTS, settle/barge-in, streaming interlock, and per-turn cost.

### Modified Capabilities

## Impact

- `src/voice/{__init__.py, stt.py, tts.py, cost.py, router.py, ui.js}`
- `tests/unit/test_voice.py`

## Channels

- **In scope:** Voice WebSocket (`/voice/ws`) as a FastAPI router
- **Out of scope:** Web Chat HTTP routes, Terminal CLI, A2A process lifecycle

## Out of Scope

- `src/api/web.py` shopper session/chat (Group 6 mounts `make_voice_router`)
- Live Gemini STT/TTS in unit tests (frame/header/math tests; live calls skipped without `GOOGLE_API_KEY`)
