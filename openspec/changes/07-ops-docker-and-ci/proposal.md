# Proposal: Ops, Docker & CI (`07-ops-docker-and-ci`)

## Why

Groups 1–6 are application code. Operators still need `run.sh` to boot Postgres, MCP Toolbox, A2A Judge, A2A Masker, Phoenix, and the web/voice app as separate processes; a `Dockerfile` for Railway; CI that runs the full pytest suite; and a README that matches the `src/` layout without GCP lock-in.

## What Changes

- Copy `backup/run.sh` and `backup/README.md`, pointing at `src/` entrypoints.
- Strip `GOOGLE_CLOUD_PROJECT`, `GOOGLE_CLOUD_LOCATION`, and `MODEL_ARMOR_TEMPLATE_ID` from generated `.env` / `tools.yaml` heredocs.
- Keep tenant-scoped `get-order-status` SQL in `run.sh gen_config_files`.
- Add `Dockerfile` and `.github/workflows/ci.yml` that run `pytest` (`unit` + `integration` + `evals`).

## Capabilities

### New Capabilities
- `ops-and-deployment`: Local multi-process orchestration, container image, GitHub Actions CI, and operator README.

### Modified Capabilities

## Impact

- `run.sh`
- `README.md`
- `Dockerfile`
- `.github/workflows/ci.yml`

## Channels

- **In scope:** process supervision for Web Chat, Voice, CLI, A2A Judge, A2A Masker, MCP Toolbox, Postgres, Phoenix
- **Out of scope:** Railway project provisioning (documented defaults only), PSTN/SIP, Vercel

## Out of Scope

- Combining the 5–6 services into one process
- Committing `.env` or secrets
