# Tasks: `06-web-and-cli-channels`

## 1. Static UI

- [ ] 1.1 Copy `backup/cs_agent/static/{index.html,architecture.html,architecture.svg}` to `src/static/` and `backup/diagram.png` to `diagram.png`, and verify `GET /`, `GET /architecture`, and `GET /architecture.svg` with `pytest tests/integration/test_api_and_tenant.py`

## 2. FastAPI app and CLI

- [ ] 2.1 Copy `backup/cs_agent/web.py` to `src/api/web.py`, read `TOOLBOX_URL`, A2A hosts/ports, `PHOENIX_*`, `HOST`, and `PORT` from env, add `_enforce_tenant_tool_callback`, mount Group 5 voice router, and verify login/logout/chat/stream plus tenant overwrite and `update-order-status` block with `pytest tests/integration/test_api_and_tenant.py`
- [ ] 2.2 Copy `backup/cs_agent/agent_cli.py` to `src/api/cli.py`, pass sanitized text to `runner.run()`, remove `.lower()` on masked output, and verify `validate_input` with `pytest tests/integration/test_api_and_tenant.py`
