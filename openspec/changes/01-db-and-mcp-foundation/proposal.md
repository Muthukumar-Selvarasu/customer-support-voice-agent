# Proposal: Database Schema, Seed Data & MCP Tool Catalog (`01-db-and-mcp-foundation`)

## Why

The platform cannot authenticate shoppers or look up orders until PostgreSQL `toolbox_db` and the MCP Toolbox SQL catalog exist. This is the first independently testable slice: schema, seed data, tool YAML, and env/dependency templates with no application runtime yet.

## What Changes

- Add `mcp_toolbox/seed.sql` with `users`, `customer_orders`, and `actions_log`, plus 10 demo users and 17 orders.
- Add `mcp_toolbox/tools.yaml` exposing `get-order-status`, `find-customer-orders`, `action-log`, and restricted `update-order-status`.
- Scope `get-order-status` to `WHERE order_id = $1 AND customer_email = $2` so another customer's row cannot be returned by order ID alone.
- Add `requirements.txt` and `.env.example` without `GOOGLE_CLOUD_PROJECT`, `google-cloud-dlp`, or `google-cloud-secret-manager`.
- Document optional `DATABASE_URL`, `TOOLBOX_URL`, `A2A_JUDGE_HOST`, `A2A_MASK_HOST`, and `PHOENIX_URL` with localhost defaults.

## Channels

- **In scope:** none (schema, YAML, and env templates only)
- **Out of scope:** Web Chat, Voice WebSocket, Terminal CLI, A2A Judge/Masker processes

## Capabilities

### New Capabilities
- `database-and-mcp-tools`: PostgreSQL schema, seed population, and MCP Toolbox SQL tool contracts.

### Modified Capabilities

## Impact

- `mcp_toolbox/seed.sql`, `mcp_toolbox/tools.yaml`, `requirements.txt`, `.env.example`
- `tests/unit/test_db_and_mcp.py`

## Out of Scope

- Python runtime (`src/core/`, FastAPI, A2A, voice)
- Live Postgres boot (schema files and YAML are validated statically in this slice)
