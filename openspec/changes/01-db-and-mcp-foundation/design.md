# Design: Database Schema, Seed Data & MCP Tool Catalog

## Context

See `proposal.md`. This slice copies `backup/mcp_toolbox/seed.sql`, `backup/mcp_toolbox/tools.yaml`, `backup/requirements.txt`, and `backup/.env.example`, ignoring `backup/PRD.md`.

## Goals / Non-Goals

**Goals:**
- Commit schema and MCP tool contracts that later slices can import.
- Tenant-scope `get-order-status` at the SQL layer from the first slice.

**Non-Goals:**
- Starting Postgres or MCP Toolbox processes.
- Python application code.

## Decisions

### Decision 1: Keep seed.sql as the schema source
Copy `backup/mcp_toolbox/seed.sql` with the three tables and demo rows. Later ops slice (`07`) wires `run.sh` to apply it.

### Decision 2: Tenant filter on get-order-status
Change `backup/mcp_toolbox/tools.yaml` `get-order-status` from `WHERE order_id = $1` to `WHERE order_id = $1 AND customer_email = $2`.

### Decision 3: Strip unused GCP packages from requirements
Remove `google-cloud-dlp`, `google-cloud-secret-manager`, and unused `google-auth` from `requirements.txt`. Add `pytest` and `pytest-asyncio`. Drop `GOOGLE_CLOUD_PROJECT`, `GOOGLE_CLOUD_LOCATION`, and `MODEL_ARMOR_TEMPLATE_ID` from `.env.example`.

## Risks / Trade-offs

- **[Risk] Later slices still need a running Postgres** → Mitigation: Group 1 tests parse YAML/SQL files; Group 2+ integration tests can skip if `DATABASE_URL` is unset.
