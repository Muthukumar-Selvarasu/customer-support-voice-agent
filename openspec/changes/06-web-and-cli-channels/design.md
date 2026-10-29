# Design: Web Chat, Architecture UI & Operator CLI

## Context

See `proposal.md`. Depends on Groups 2–5. Copies `backup/cs_agent/web.py`, `agent_cli.py`, `static/*`, and `backup/diagram.png`. Closes PoC bug #5 (IDOR in `tool_result`) and CLI sanitizer/lowercase drift.

## Goals / Non-Goals

**Goals:**
- One FastAPI app for chat, inspector UI, architecture SVG, and voice mount.
- Tenant isolation in SQL (Group 1) **and** ADK callback (this slice).

**Non-Goals:**
- Merging Judge/Masker into this process.
- Container/CI (Group 7).

## Decisions

### Decision 1: Layout `src/api/` + `src/static/`
`web.py` and `cli.py` live under `src/api/`. Static assets under `src/static/`. `diagram.png` stays at repo root for README.

### Decision 2: Dual tenant enforcement
SQL `WHERE order_id = $1 AND customer_email = $2` is necessary but not sufficient (PoC leaked Bob's row in `tool_result` before the LLM refused). `_enforce_tenant_tool_callback` injects the session email/`user_id` and blocks `update-order-status`.

### Decision 3: CLI parity with web
`backup/cs_agent/agent_cli.py` sanitizes a local copy then runs the raw string and `.lower()`s output. Match web: run sanitized text, print masked text with original casing.

### Decision 4: Integration tests mock Toolbox/A2A when down
`GET /` and static routes always run. Login/chat/tenant tests skip or mock unless `DATABASE_URL` and Toolbox are available.

## Risks / Trade-offs

- **[Risk] Full chat tests need Gemini + Toolbox + A2A** → Mitigation: unit-test the tenant callback with a fake tool context; keep one optional live path.
