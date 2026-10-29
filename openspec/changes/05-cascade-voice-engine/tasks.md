# Tasks: `05-cascade-voice-engine`

## 1. Voice modules

- [ ] 1.1 Copy `backup/cs_agent/voice/{__init__.py,stt.py,tts.py,cost.py,ui.js}` to `src/voice/` and verify `pcm16_to_wav`, `_clean`, `split_sentences`, `cost.usd`, `estimate_tokens`, and `usage_tokens` with `pytest tests/unit/test_voice.py`
- [ ] 1.2 Copy `backup/cs_agent/voice/router.py` to `src/voice/router.py`, keep `STREAM_ENABLED = AGENT_RESPONSE_STREAM and not MASK`, remove the `GOOGLE_CLOUD_PROJECT` masking warning, inject the pipeline via `make_voice_router`, and verify settle (`1500 ms`) plus barge-in `interrupt` with `pytest tests/unit/test_voice.py`
