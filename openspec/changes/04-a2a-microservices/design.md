# Design: A2A JSON-RPC Microservices

## Context

See `proposal.md`. Depends on `03-security-guardrails-and-evals`. Copies `backup/cs_agent/a2a/*` and `backup/cs_agent/agents/*`. Judge and Masker remain separate processes.

## Goals / Non-Goals

**Goals:**
- In-process testable A2A protocol (discovery + RPC).
- Env-driven bind addresses for Railway private DNS later.

**Non-Goals:**
- Combining Judge and Masker into the web process.
- Docker / `run.sh` (Group 7 starts these processes).

## Decisions

### Decision 1: Keep two A2A servers
`run_servers.py` starts Judge and Masker in daemon threads. Production (Group 7 / Railway) can run them as separate services using the same factories.

### Decision 2: Env overrides for host/port
Replace hardcoded `localhost`/`10002`/`10003` in `main()` with `A2A_JUDGE_HOST`, `A2A_JUDGE_PORT`, `A2A_MASK_HOST`, `A2A_MASK_PORT`.

### Decision 3: Mask agent calls Group 3 directly
`backup/cs_agent/agents/mask_agent.py` passes `project_id = os.getenv("GOOGLE_CLOUD_PROJECT", "")`. After Group 3 the signature is `mask_sensitive_data(text)` only.

### Decision 4: Integration tests use TestClient
Do not require ports 10002/10003 to be free. Mount `A2AServer.app` in FastAPI `TestClient` / `httpx.AsyncClient`.

## Risks / Trade-offs

- **[Risk] Judge LLM calls make tests slow/flaky** → Mitigation: stub `call_judge_agent` / `call_mask_agent` in RPC tests; keep one optional live test skipped without `GOOGLE_API_KEY`.
