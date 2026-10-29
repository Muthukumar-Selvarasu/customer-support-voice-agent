# Tasks: `02-core-auth-memory-telemetry`

## 1. Auth, memory, and prompt contract

- [ ] 1.1 Copy `backup/cs_agent/greet.py` to `src/core/greet.py`, resolve Postgres via `DATABASE_URL` then `PG*` then localhost defaults, and verify with `pytest tests/unit/test_core.py`
- [ ] 1.2 Copy `backup/cs_agent/memory.py` to `src/core/memory.py` (`search_memory` / `save_memory` scoped by `user_id`) and verify with `pytest tests/unit/test_core.py`
- [ ] 1.3 Copy `backup/cs_agent/prompts.py` to `src/core/prompts.py`, replace `parameters_json` with `parameters`, and verify `SQL_PROMPT_INSTRUCTION.format(USER_ID=...)` with `pytest tests/unit/test_core.py`

## 2. Telemetry

- [ ] 2.1 Copy `backup/cs_agent/telemetry.py` to `src/core/telemetry.py`, honor `PHOENIX_URL` / `PHOENIX_PUBLIC_URL`, skip `webbrowser.open()` in headless/Railway, return `_NoopTracer` when `TELEMETRY=false`, and verify with `pytest tests/unit/test_core.py`
