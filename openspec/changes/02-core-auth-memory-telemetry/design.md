# Design: Core Auth, Memory, Prompts & Telemetry

## Context

See `proposal.md`. Depends on `01-db-and-mcp-foundation`. Copies `backup/cs_agent/{greet,memory,prompts,telemetry}.py` into `src/core/`.

## Goals / Non-Goals

**Goals:**
- Leaf modules with no FastAPI/A2A/voice imports.
- Env-driven DB and Phoenix URLs for local and Railway.

**Non-Goals:**
- HTTP routes, sessions, or ADK `before_tool_callback` (Group 6).
- Starting Phoenix or Mem0.

## Decisions

### Decision 1: Package as `src/core/`
Move `backup/cs_agent/greet.py`, `memory.py`, `prompts.py`, and `telemetry.py` to `src/core/`. Later slices import `src.core.*`.

### Decision 2: DATABASE_URL first, then PG*, then cascade localhost defaults
Replace hardcoded `127.0.0.1` in `_pg()` so Railway `DATABASE_URL` works without a stray `PGUSER` hijacking local `./run.sh setup`.

### Decision 3: Fix `parameters_json` in the prompt
`backup/cs_agent/prompts.py` line 56 says `parameters_json`; `mcp_toolbox/tools.yaml` defines `parameters`. Use `parameters`.

### Decision 4: Skip browser open when headless
`telemetry.py` currently opens Phoenix in a browser. Skip that when `RAILWAY_ENVIRONMENT` or `PHOENIX_OPEN_BROWSER=false` is set.

## Risks / Trade-offs

- **[Risk] Auth tests need Postgres** → Mitigation: unit-test connection-string parsing without a live DB; mark live `authenticate_user` tests to skip when `DATABASE_URL` is unset.
