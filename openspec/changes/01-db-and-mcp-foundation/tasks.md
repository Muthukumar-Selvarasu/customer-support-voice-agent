# Tasks: `01-db-and-mcp-foundation`

## 1. Dependencies and environment template

- [ ] 1.1 Copy `backup/requirements.txt` to `requirements.txt`, remove `google-cloud-dlp`, `google-cloud-secret-manager`, and unused `google-auth`, add `pytest` and `pytest-asyncio`, and verify those packages are absent with `pytest tests/unit/test_db_and_mcp.py`
- [ ] 1.2 Copy `backup/.env.example` to `.env.example`, remove `GOOGLE_CLOUD_PROJECT`, `GOOGLE_CLOUD_LOCATION`, and `MODEL_ARMOR_TEMPLATE_ID`, keep `GOOGLE_API_KEY`, `MEM0_API_KEY`, `MASK`, `AGENT_RESPONSE_STREAM`, and `TELEMETRY`, add optional `DATABASE_URL`, `TOOLBOX_URL`, `A2A_JUDGE_HOST`, `A2A_MASK_HOST`, and `PHOENIX_URL` with localhost defaults, and verify with `pytest tests/unit/test_db_and_mcp.py`

## 2. Schema, seed, and MCP tools

- [ ] 2.1 Copy `backup/mcp_toolbox/seed.sql` to `mcp_toolbox/seed.sql` and verify it defines `users`, `customer_orders`, `actions_log`, 10 users, and 17 orders with `pytest tests/unit/test_db_and_mcp.py`
- [ ] 2.2 Copy `backup/mcp_toolbox/tools.yaml` to `mcp_toolbox/tools.yaml`, add `customer_email` to `get-order-status` (`WHERE order_id = $1 AND customer_email = $2`), and verify toolset `cs_agent_tools` with `pytest tests/unit/test_db_and_mcp.py`
