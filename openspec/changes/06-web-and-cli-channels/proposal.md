# Proposal: Web Chat, Architecture UI & Operator CLI (`06-web-and-cli-channels`)

## Why

Groups 1–5 are libraries and A2A/voice routers. Shoppers still need Web Chat (`POST /api/chat`, `/api/chat/stream`), login/logout with Mem0 flush, the `"What happened"` inspector, live `/architecture` SVG, and an operator CLI with the same sanitizer and casing as web.

This slice closes PoC bug #5 (IDOR `tool_result` leak) with ADK `before_tool_callback`, and the CLI sanitizer/lowercase drift.

## What Changes

- Add `src/static/{index.html, architecture.html, architecture.svg}` and `diagram.png`.
- Add `src/api/web.py` with env-driven `TOOLBOX_URL`, A2A hosts/ports, `PHOENIX_*`, `HOST`, `PORT`.
- Add `_enforce_tenant_tool_callback` to inject the session `user_id`/email into tool args and block `update-order-status`.
- Add `src/api/cli.py` from `agent_cli.py`: pass sanitized text into `runner.run()` and do not `.lower()` masked output.
- Mount Group 5 `make_voice_router` at `/voice/ws`.

## Capabilities

### New Capabilities
- `web-and-cli-channels`: Shopper HTTP/WS app, glass-box UI, architecture page, operator CLI, and ADK tenant callback.

### Modified Capabilities

## Impact

- `src/static/{index.html, architecture.html, architecture.svg}`
- `diagram.png`
- `src/api/{__init__.py, web.py, cli.py}`
- `tests/integration/test_api_and_tenant.py`

## Channels

- **In scope:** Web Chat (`POST /api/login`, `/api/logout`, `/api/chat`, `/api/chat/stream`), Voice WebSocket mount, Terminal CLI, `GET /` and `GET /architecture`
- **Out of scope:** Docker, `run.sh`, GitHub Actions (Group 7)

## Out of Scope

- Combining A2A Judge/Masker into the web process
- Direct `UPDATE` on `customer_orders`
