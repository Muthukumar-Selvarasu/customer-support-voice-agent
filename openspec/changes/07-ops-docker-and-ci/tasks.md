# Tasks: `07-ops-docker-and-ci`

## 1. Local orchestration and docs

- [ ] 1.1 Copy `backup/run.sh` to `run.sh`, point process entrypoints at `src/`, remove GCP keys from generated `.env` / `tools.yaml`, keep tenant-scoped `get-order-status`, and verify with `bash -n run.sh` plus `pytest tests/unit/test_db_and_mcp.py`
- [ ] 1.2 Copy `backup/README.md` to `README.md`, document `./run.sh`, `GOOGLE_API_KEY` / `MEM0_API_KEY` / `DATABASE_URL`, and Railway multi-service notes without `GOOGLE_CLOUD_PROJECT`, and verify those strings with `pytest tests/unit/test_db_and_mcp.py`

## 2. Container and CI

- [ ] 2.1 Add `Dockerfile` (Python 3.12, `requirements.txt`, web/voice default CMD) and `.github/workflows/ci.yml` (`pytest` on `tests/unit`, `tests/integration`, `tests/evals`), and verify the full suite with `pytest`
