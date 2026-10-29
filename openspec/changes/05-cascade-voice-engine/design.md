# Design: Cascade Voice Engine

## Context

See `proposal.md`. Depends on `02-core-auth-memory-telemetry` (`telemetry.py`) and injects the text pipeline at mount time (Group 6). Copies `backup/cs_agent/voice/*`.

## Goals / Non-Goals

**Goals:**
- Preserve PoC voice protocol (settle, barge-in, glass-box events).
- Unit-test framing, cleanup, interlock, and cost without live audio.

**Non-Goals:**
- Shopper login/session (Group 6).
- Changing STT (`gemini-3.1-flash-lite`) or TTS (`gemini-3.8-flash-tts`) models.

## Decisions

### Decision 1: Inject the pipeline
Keep `make_voice_router(pipeline=...)` so Group 5 tests can pass a fake async generator and do not import `web.py`.

### Decision 2: Remove the GCP masking warning
Delete `backup/cs_agent/voice/router.py` lines 70–74. Group 3 masking works without `GOOGLE_CLOUD_PROJECT`. Keep the MASK vs stream conflict warning.

### Decision 3: TestClient websocket for protocol tests
Use FastAPI `TestClient.websocket_connect` with a stub pipeline for settle/interrupt. Skip live STT/TTS unless `GOOGLE_API_KEY` is set.

## Risks / Trade-offs

- **[Risk] Settle/interrupt tests are timing-sensitive** → Mitigation: inject a shorter settle in tests or monkeypatch `_SETTLE_MS`.
